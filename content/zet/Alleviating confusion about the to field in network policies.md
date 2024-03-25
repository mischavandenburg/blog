---
Title: Alleviating Confusion About The To Field In Network Policies
date: 2024-03-25
tags:
- Kubernetes
- Networking
- Study
---

When solving a killercoda challenge I ran into some confusion. Even though my solution worked, there was a difference which I wanted to get clear on. 

I wrote this:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: np
  namespace: space1
spec:
  podSelector: {}
  policyTypes:
  - Egress
  egress:
  - to:
    - namespaceSelector:
        matchLabels:
          kubernetes.io/metadata.name: space2
  - to:
    ports:
    - protocol: TCP
      port: 53
    - protocol: UDP
      port: 53
```

But the provided course solution was this:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: np
  namespace: space1
spec:
  podSelector: {}
  policyTypes:
  - Egress
  egress:
  - to:
     - namespaceSelector:
        matchLabels:
         kubernetes.io/metadata.name: space2
  - ports:
    - port: 53
      protocol: TCP
    - port: 53
      protocol: UDP
```

Do you see the difference? I used the `to:` field before the `ports:`  field.

My solution was correct, but it had me quite confused, why wouldn't they write `to:` here as well?

After some further digging I've finally understood that the `to:` field is only necessary when you apply a selector. If the `to:` field is omitted, it means that it will apply to any destination. So in my solution, the `to:` field doesn't achieve anything because I'm not applying any selector. It can therefore be omitted.

Each element under the `egress:` array is a separate rule and they are combined to reach the end result of allowing egress traffic to namespace space2, and UDP/TCP traffic to anywhere on port 53.

In this case, omitting the `to:` field makes sense because DNS is generally not limited to only one namespace. However, if you're only using the internal Kubernetes DNS you might want to limit it to the kube-system namespace. I've done this in a CiliumNetworkPolicy in my homelab:

```yaml
apiVersion: cilium.io/v2
kind: CiliumNetworkPolicy
metadata:
  name: commafeed-app
  namespace: commafeed
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
```


## Links:

**part of**:: [[Network Policies]]
[[CKS]]
[[networking-computers]]


202403250845
