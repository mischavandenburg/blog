---
title: Learned Cilium Network Policies
date: 2024-01-14
tags:
- Kubernetes
- Networking
- Cilium
---

Today I learned about Cilium network policies. These are much easier to implement than normal network policies because there are some tools available when creating the cilium policies. Network policies were probably my weakest Kubernetes skill and I tended to avoid them.

But now I'm exposing some apps to the internet in my homelab and I'm forced to think about security and what would happen if a hacker managed to get root privileges in a container even though I implemented strict security contests and enabled privilege escalation.

I installed Cilium as the Container Networking Interface in my homelab cluster. This also allows me to use the Hubble tool. This has proven to be of immmense value to see the traffic that is actually flowing around in the namespace you're working on.

![](/npolicy1.png)

Even with a small application of a few microservices, as the image shows, the traffic that flows between them and which needs to come from other namespaces can get pretty complex. But in the Hubble UI you get a clear view of what's happening, and you can see exactly when packets are being dropped.

I also found the [Network Policy Editor](https://editor.networkpolicy.io/) extremely valuable. It is such a relief to be able to compose the policy from a graphical interface, even though I'm a CLI guy and want to do everything in and from code, this has been a gamechanger.

Now I have created the following policy for my linkding app, which allows the EDB operator to do its thing on my databases, Prometheus to monitor my application, but a hacker could not do anything outside of the namespace if they managed to break out.

```yaml
apiVersion: cilium.io/v2
kind: CiliumNetworkPolicy
metadata:
  name: linkding-app
  namespace: linkding
spec:
  endpointSelector:
    matchLabels:
      policy-type: "app"
  ingress:
    - fromEndpoints:
        - {}
    - fromEndpoints:
        - matchLabels:
            io.kubernetes.pod.namespace: monitoring
    - fromEndpoints:
        - matchLabels:
            io.kubernetes.pod.namespace: postgresql-operator-system
  egress:
    - toEndpoints:
        - matchLabels:
            io.kubernetes.pod.namespace: kube-system
            k8s-app: kube-dns
      toPorts:
        - ports:
            - port: "53"
              protocol: UDP
          rules:
            dns:
              - matchPattern: "*"
    - toEndpoints:
        - {}
    - toEntities:
        - world
      toPorts:
        - ports:
            - port: "443"
        - ports:
            - port: "80"
        - ports:
            - port: "7844"
---
apiVersion: cilium.io/v2
kind: CiliumNetworkPolicy
metadata:
  name: linkding-database
  namespace: linkding
spec:
  endpointSelector:
    matchLabels:
      policy-type: "database"
  ingress:
    - fromEndpoints:
        - {}
    - fromEndpoints:
        - matchLabels:
            io.kubernetes.pod.namespace: monitoring
    - fromEndpoints:
        - matchLabels:
            io.kubernetes.pod.namespace: postgresql-operator-system
  egress:
    - toEndpoints:
        - {}
    - toEndpoints:
        - matchLabels:
            io.kubernetes.pod.namespace: kube-system
            k8s-app: kube-dns
      toPorts:
        - ports:
            - port: "53"
              protocol: UDP
          rules:
            dns:
              - matchPattern: "*"

    - toEntities:
        - kube-apiserver
      toPorts:
        - ports:
            - port: "6443"

    - toEntities:
        - world
      toPorts:
        - ports:
            - port: "443"
---
apiVersion: cilium.io/v2
kind: CiliumNetworkPolicy
metadata:
  name: deny-all
  namespace: linkding
spec:
  endpointSelector: {}
  ingress:
    - {}
  egress:
    - {}
```


## Links:

202401142001
