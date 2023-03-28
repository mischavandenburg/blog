---
title: Kubernetes Resource Management for Pods and Containers - CPU and Memory
date: 2023-03-28
tags:
- Kubernetes
- Linux
- Cloud-Native
---

Pods have containers, and limits can be set on those containers.

# Requests

* used by the kube-scheduler to determine where the Pod will be placed
* containers can use more than requested resources if it is available on node

If a limit is specified, but no request, Kubernetes will use the limit value as the request value.

# Limits

* containers may never use more than the set limit
* enforced by kubelet and container
* host kernel will kill processes that attempt to allocate more than limit (OOM error)
* reactively: killed when exceeded
* enforcement: system prevents container to ever exceed limit

* If the node runs out of memory and the container exceeds its memory request, the pod will be evicted
* container runtimes don't terminate Pods or containers for excessive CPU usage


# Pods

The pod resource request and limit is the sum of the resource requests of the containers in the pod.

# CPU Units

Defined as an absolute amount of resource. 1000m = 1 CPU. 

This is always the same unit, regardless whether the host has 4 or 48 CPU's.

500m CPU = 0.5 CPU

# Memory Units

Can use P, T, G, M etc. 

Note that "m" is not megabyte. 0.8m = 0.8 bytes.

Use mebibytes Mi or megabytes M.

# Definiton Example

```yaml
spec:
  containers:
  - name: nginx
    image: nginx
    resources:
      requests:
        memory: 100Mi
        cpu: 250m
      limits:
        memory: 200Mi
        cpu: 500m
```

# Scheduling

The scheduler ensures that the sum of requests of the pods on the node does not exceed the available resources.

Even if a node has low resource usage, it will not accept pods that have requests which exceed the available resources.

# Nodes

use `k describe node` to see the resource status of the node.

## Links:

202303281903

https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/
