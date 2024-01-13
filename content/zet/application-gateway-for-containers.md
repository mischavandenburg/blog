---
title: Video notes - Application Gateway for Containers
date: 2024-01-05
tags:
- Azure
- Networking
- Kubernetes
---

{{< youtube slCjHV8z9Wk >}}

# App Gateway

It has App Gateway in the name, but it is an entirely new solution. The App Gateway is the only thing it has in common with Azure Application Gateway.

# Resources

Two types of resources. Azure resources and k8s resources.

The App Gateway for Container is an azure resource which listens to changes in k8s resources through the ALB controller. AGWFC is the control plane.

# Frontend

## Azure

Frontend is a public IP and fqdn. Both are managed resources, you don't see them in your subscription.

You can have multiple frontends in one gateway to save money. Teams could share the app gateway but use different frontend IP addresses or FQDNs.

Control plane: Azure App Gateway for Containers

Data plane: association with kubernetes pods.

The association is made to the subnet in the Azure VNet.

Each association is in one subnet, and the subnet should at least have /24 or 256 addresses.

# Kubernetes

ALB controller consists of two pods. Controller pod and a bootstrap pod.

Controller communicates to the Azure gateway resource. It talks directly to the App Gateway, not to the Azure Resource Manager, which is why you're able to have sub-second updates.

The bootstrap contains the CRDs etc, it does not do very much.

## Creating resources

There is a managed option that will talk to ARM and create the resources for you. Or you can choose to deploy them yourself. It depends whether you want to control everything from Kubernetes. If you have all your Azure resources in Infrastructure as Code it probably makes more sense to create the App Gateway resources from there instead of from Kubernetes.

# Association

Links frontend with a subnet in a VNet. This will typically be the same vnet that the AKS cluster is in. Could technically be a peered VNet but probably uncommon.

This is an Azure resource. It lives in the VNet and handles TLS and makes the connections to and from the pods and frontend IP. This is the data path.

The traffic is not routed within the cluster, but in Azure by the association.

Client talks to the front end, passes to the association, the association is doing the work, and then routing it to the cluster.

# Support

Azure CNI. Does not support kubenet or Azure CNI overlay yet, but it will support in the future.

# Kubernetes Resources

Application Load Balancer: this name was chosen because k8s people don't know the concept of app gateway.

# Benefits

* ssl offloading
* traffic splitting
* clearer separation between platform team and app team
* platform team manages the gateway itself
* automatic default health probes

# Benefits of Azure

* not suing cluster resources for load balancing. this happens in azure
* native azure metrics available


# misc

* AGIC does not support ingress


## Links:

202309301009
