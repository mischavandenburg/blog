---
title: Kubernetes Gateway API & Azure Application Gateway for Containers
date: 2024-03-10
tags:
- Article
- Kubernetes
- Azure
- Networking
---

This document is the result of my research into the Gateway API. It aims to briefly describe the Gateway API for Kubernetes, a typical implementation of ingress traffic using NGINX in AKS and how this setup could benefit from implementing the Gateway API.

# Introduction

Gateway API is an official Kubernetes project focused on L4 and L7 routing in Kubernetes. This project represents the next generation of Kubernetes Ingress, Load Balancing, and Service Mesh APIs. From the outset, it has been designed to be generic, expressive, and role-oriented.

The overall resource model focuses on 3 separate personas and corresponding resources that they are expected to manage:

![](/ga1.png)

Up until now ingress traffic to Kubernetes clusters was handled by the Ingress resource. Although the Ingress API will not be deprecated, the Gateway API is where all the development and innovation happens in the Kubernetes project.

>Gateway API is the successor to the Ingress API.
>
>-Kubernetes documentation

# A Typical NGINX Ingress Solution

Ingress traffic is typically implemented as follows.

1. An AKS cluster is provisioned in its own resource group and added to an existing virtual network. Usually each subscription has its own virtual network.
2. We use the NGINX Ingress Controller which is deployed through ArgoCD
3. The NGINX Ingress controller creates a Kubernetes LoadBalancer resource and exposes the cluster on port 80 and 443, as you can see below:

```bash
Switched to context "aks-gwa-dev-admin".
mischa@mac-beast:~
(ins)$ kn
Context "aks-gwa-dev-admin" modified.
Active namespace is "nginx-ingress".
mischa@mac-beast:~
(ins)$ k get svc
NAME                                                    TYPE           CLUSTER-IP        EXTERNAL-IP    PORT(S)                      AGE
nginx-ingress-ingress-nginx-controller                  LoadBalancer   192.168.10.17     52.137.26.47   80:30974/TCP,443:30474/TCP   595d
nginx-ingress-ingress-nginx-controller-admission        ClusterIP      192.168.51.96     <none>         443/TCP                      595d
nginx-ingress-ingress-nginx-controller-metrics          ClusterIP      192.168.214.164   <none>         10254/TCP                    417d
nginx-ingress-ingress-nginx-vo-custom-default-backend   ClusterIP      192.168.226.137   <none>         80/TCP                       595d
mischa@mac-beast:~
(ins)$
```

4. When a LoadBalancer resource is created in the cluster, AKS creates an Azure Load Balancer in the resource group of the AKS cluster. This Azure Load Balancer receives a public or internal IP address dependent on the Ingress Controller configuration. You can have a normal/external controller and an internal controller which doesn't receive a public IP address.
5. An Ingress resource is created in Kubernetes to configure the FQDN and TLS settings. In the Ingress resource, a label selector is configured to route the traffic to the correct backend service, which will route the traffic to the correct pods.
6. A DNS record is created and linked to the IP of the Azure Load Balancer which was provisioned by the Ingress Controller
7. When the FQDN is approached in the browser, the Azure DNS zone will forward the request to the Azure Load Balancer. The NGINX Ingress Controller will then route the traffic to the backend pods which were selected in the Ingress resource in Kubernetes.

## Disadvantages Of This Situation

* Ingress API has less functionality
* Azure Loadbalancer only works on Layer 4, no layer 7 routing
* NGINX Ingress Controller maintenance and configuration

# Benefits of Gateway API

* natively supports traffic weighting, no extra annotations needed on ingress controller
  * example: sending 50% of the traffic to an application instance with a feature flag enabled
* natively supports header-based matching
* traffic can be routed to other resources outside of the cluster or other clusters
* extensible: allows for custom resources to be linked at various layers of the API, allowing customization
* simplifies networking configuration
* aids in transition towards service mesh
* set up to support role based division of labour. The platform team sets up the infrastructure and Gateway itself, and the developer teams can add routing configuration that suits their needs
* enables the developer teams to gain full control over their network configuration through GitOps

Gateway API simplifies networking configuration by standardizing the way we define network rules. Thus, Gateway API aids in the transition from ingress towards service mesh. When the Gateway API is implemented, it will be much easier to adopt a service mesh networking architecture, or to migrate between service meshes.

Another advantage is that it can enable teams to implement specific header-based routing and traffic weighting between backend pools

A more specific list of Gateway API features:

* Header rewrite
* HTTPS traffic management:
* SSL termination
* End to End SSL
* Ingress and Gateway API support
* Layer 7 HTTP/HTTPS request forwarding based on prefix/exact match on:
  * Hostname
  * Path
  * Header
  * Query string
  * Methods
  * Ports (80/443)
* Mutual Authentication (mTLS) to backend target
* Traffic Splitting / weighted round robin
* TLS Policies
* URL rewrite

# Implementations

The Gateway API can be implemented [through various controllers](https://gateway-api.sigs.k8s.io/implementations/). Some notable implementations:

[NGINX Gateway Fabric (GA)](https://github.com/nginxinc/nginx-gateway-fabric)

[Traefik (alpha)](https://doc.traefik.io/traefik/routing/providers/kubernetes-gateway/)

[Cilium (beta)](https://cilium.io/)

These will all require an extra controller or even a CNI to be installed.

However, with the release of [Application Gateway for Containers](https://learn.microsoft.com/en-us/azure/application-gateway/for-containers/overview), Azure also offers a native implementation of the Gateway API in Azure Kubernetes Service. 

# Application Gateway for Containers

The implementation of Gateway API through Azure's solution is rather straightforward since we are already running on the Azure platform.

The AG for Containers is a cluster-specific resource which deploys two resources in Azure: a frontend and an association. The frontend is an entry point where traffic is received in the form of an FQDN. The association is the entry to a virtual network. An association is a 1:1 mapping of an association resource to an Azure Subnet that has been delegated.

Each Application Gateway for Containers frontend provides a generated Fully Qualified Domain Name managed by Azure. The FQDN may be used as-is or customers may opt to mask the FQDN with a CNAME record.

Before a client sends a request to Application Gateway for Containers, the client resolves a CNAME that points to the frontend's FQDN; or the client may directly resolve the FQDN provided by Application Gateway for Containers by using a DNS server.

When the client initiates the request, the DNS name specified is passed as a host header to Application Gateway for Containers on the defined frontend.

![](/ga2.png)

Image from Microsoft Documentation. I don't own this image.

## Gateway and Route Setup

A set of routing rules evaluates how the request for that hostname should be initiated to a defined backend target.

These resources are provided from within the cluster by creating a gateway with a Kubernetes manifest:

```yaml

kubectl apply -f - <<EOF
apiVersion: gateway.networking.k8s.io/v1beta1
kind: Gateway
metadata:
  name: gateway-01
  namespace: test-infra
  annotations:
    alb.networking.azure.io/alb-namespace: alb-test-infra
    alb.networking.azure.io/alb-name: alb-test
spec:
  gatewayClassName: azure-alb-external
  listeners:
  - name: http
    port: 80
    protocol: HTTP
    allowedRoutes:
      namespaces:
        from: Same
EOF
```

When the gateway is created, the AG for Containers will create the required frontend and association.

Then we can create a route that connects the gateway to the backend pods of our application. This example shows how easy it is to weight traffic 50/50 to two backends:

```yaml
kubectl apply -f - <<EOF
apiVersion: gateway.networking.k8s.io/v1beta1
kind: HTTPRoute
metadata:
  name: traffic-split-route
  namespace: test-infra
spec:
  parentRefs:
  - name: gateway-01
  rules:
  - backendRefs:
    - name: backend-v1
      port: 8080
      weight: 50
    - name: backend-v2
      port: 8080
      weight: 50
EOF
```

![](/ga3.png)

Image from Microsoft Documentation. I don't own this image.

### Azure Load Balancer Controller

In order to enable these resources on the cluster, the Azure Load Balancer controller needs to be installed via Helm. When it is connected to a Managed Identity it is able to provision the required resources in Azure.

There is an alternative deployment mode where the Azure resources (frontends and associations) can be provisioned through Terraform or Bicep and referred to from within the cluster. However, I think that it is best to deploy these resources from within the cluster because it enables the application teams to get full control over their traffic based on the GitOps workflow that they are already used to working with.

## Advantages of the Azure Application Gateway for Containers

* Natively supported for AKS on the Azure platform
* Microsoft support and SLA
* Increased performance, offering near real-time updates to add or move pods, routes, and probes
* Does not need any configuration on the controller
* Autoscaling
* Availability zone resiliency
* Azure resources can be provisioned from within the cluster
* The FQDN can be linked to the DNS zone using [External DNS](https://github.com/kubernetes-sigs/external-dns), Azure Front Door or Azure DNS zones
* Will likely be integrated into the AKS offering as an extension, which means we won't have to maintain the controller
* Easier to maintain because configuration is tailored to AKS out of the box

## Disadvantages of the Azure Application Gateway for Containers

I don't see any disadvantages over using the AG for Containers in terms of technical implementation. In fact, implementing the AG for Containers will be the most straightforward and easiest option. However, we must consider the price. As the AG for containers does not currently have WAF functionality, the premium NGINX Ingress Controller offering could be considered in the price.

Using NGINX or Traefik would still require an Azure LoadBalancer resource to provision an internal and/or external IP. When making a pricing comparison, the following things should be considered when using a different solution:

* cost of Azure Load Balancer (and the reduncany of the load balancer)
* cost of man hours needed to learn a new controller such as Traefik or NGINX
* time required to upgrade the controller and reading up on release notes and changelogs
* potential licence costs when adopting enterprise usage

## Lifecycle management

No matter which implementation we choose, the lifecycle management of the controller will at this point still be with us. When using the Azure Application Gateway for containers, it is installed with helm and updated through helm. We can use our ArgoCD setup for this. The same applies for different solutions such as traefik or nginx.

However, I expect that the ALB controller will be integrated within AKS in the future and that would remove the need for lifecycle management for the ALB controller.

# Impact Analysis

Since the Azure Application Gateway for Containers or the NGINX Gateway Fabric are deployed as separate controllers, these could co-exist with the current ingress setup. Normal Ingress resources should not be affected. 

When adopting the Gateway API, all the existing Ingress resources will need to be converted to Gateway API resources in our code base. We will need to decide if this needs to be done by the platform team or the developer teams.

One solution could be that we configure the Gateway and let the teams know that this is now a posiblity and that this should be used for new application deployments. The teams can then configure the Routes themselves after some training by the platform team.

# Conclusion

The Gateway API is the next iteration of the Ingress resource. It allows more functionality and aids in defining the roles between the platform and development teams. If one is in the process to revise the method of handling ingress traffic to the clusters, it would make sense to adopt the Gateway API in this process.

Although there are no plans for a service mesh at the time of writing, having the Gateway API implemented will make the transition to a service mesh much easier. In the meantime, the Gateway API already provides interesting functionality such as traffic weighting, which would be one of the reasons to implement a service mesh.

Since the Gateway API itself is a Kubernetes resource, it does not have feature parity and would work on any Kubernetes cluster that has a controller which allows the Gateway resource to be defined. The choice we need to make is which controller we will use for the implementation of the Gateway API on our AKS clusters.

Given the ease of implementation, native Azure support and simplified maintenance, the Application Gateway for Containers is the most logical choice.

# Links

https://gateway-api.sigs.k8s.io/

https://kubernetes.io/docs/concepts/services-networking/gateway/

https://learn.microsoft.com/en-us/azure/application-gateway/for-containers/overview

https://github.com/kubernetes-sigs/external-dns/blob/master/docs/tutorials/gateway-api.md

## Links:

202403101103
