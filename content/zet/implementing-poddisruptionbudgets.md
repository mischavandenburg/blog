---
title: Implementing Pod Disruption Budgets
date: 2023-11-27
tags:
- Kubernetes
- AKS
- Coding
---

When I was doing the first round of cluster upgrades ever at my current client, I noticed we were running a lot of pods with only 1 replica. I always try to lift my clients to the next level by leveraging Cloud Native technologies as much as possible. I'm therefore starting a project to always run applications with multiple replicas.

However, running multiple replicas is not the only necessary improvement here. Even though a pod is running with multiple replicas, that does not mean that Kubernetes will always keep them alive. When you do an AKS cluster upgrade, nodes are drained one by one and the pods are moved to a node with the higher k8s version. Technically, when draining a node, Kubernetes could kill both of the pods at the same time if they are running on the same node.

To prevent this we use [pod disruption budgets](https://kubernetes.io/docs/tasks/run-application/configure-pdb/)

# Helm

We're using a Helm chart to deploy with ArgoCD. I'm adding this to the templates directory:

```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: pyramid-backend-pdb
spec:
  minAvailable: 1
  selector:
    matchLabels:
      tier: backend
```

I'm selecing all the pods that have the label "tier: backend".

To verify that I'm targeting the right pods, I run:

`kubectl get pods -l tier=backend`

Currently my deployment has only two replicas:

```
(ins)$ k get deploy
NAME                         READY   UP-TO-DATE   AVAILABLE   AGE
pyramid-deployment-backend   1/1     1            1           297d
```

So when I check my PDB it will show no allowed disruptions.

```
(ins)$ k get pdb
NAME                  MIN AVAILABLE   MAX UNAVAILABLE   ALLOWED DISRUPTIONS   AGE
pyramid-backend-pdb   1               N/A               0                     13m
```

This is good, this is the expected behaviour. Kubernetes will now prevent me from draining the node because there are no allowed disruptions.

Then I scale up the deployment:

```
(ins)$ k scale deploy --replicas=2 pyramid-deployment-backend
deployment.apps/pyramid-deployment-backend scaled
```

And then I get an allowed disruption of 1:

```
(ins)$ k get pdb
NAME                  MIN AVAILABLE   MAX UNAVAILABLE   ALLOWED DISRUPTIONS   AGE
pyramid-backend-pdb   1               N/A               1                     15m
```

With this configuration, Kubernetes will never kill all of the pods simultaneously in case a node needs to be drained. It will make sure that one of the pods keeps running when it is rescheduling pods for an upgrade.

## Links:

https://kubernetes.io/docs/tasks/run-application/configure-pdb/

202311271411
