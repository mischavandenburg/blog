---
title: Pod Disruption Budgets Can Mess With Your AKS Updates
date: 2023-09-27
tags:
- AKS
- Azure
- Kubernetes
---

Past week we've been struggling a bit with poorly configured pod disruption budgets. When you do an AKS upgrade, a new node is created and one of the old nodes is drained. 

If a deployment has a pod disruption budget which is incorrectly configured, it might show up as `ALLOWED DISRUPTIONS: 0`. When this happens, the node cannot be drained and you will get an error message in your events.

`k get poddisruptionbudgets.policy`

`k get events`

The error message will say something like "Too man eviction attempts, usually a pdb" (I lost the shell output so can't copy atm).

Kubernetes is in a situation where it needs to schedule the pod on another node, but it is unable to do do so because we are telling Kubernetes that it is not allowed to have any disruptions on the deployment.

Kubernetes is logical, it's doing like it's told. But it's frustrating because you can be sitting there waiting for 20 minutes wondering why your node isn't draining.

At my current gig we are not responsible for the content on the clusters and we should not meddle with the application teams' namespaces.

However, one solution is to either kill the pod manually or scale up the deployment to more replicas so there will be a higher amount of allowed disruptions.

## Links:

202309271809

[[kubernetes]]

[[azure]]
