---
title: "Video: Learning Flux and Installing To Homelab"
date: 2023-12-26
tags:
- Flux
- Kubernetes
- Homelab
draft: true
---

In this video I set up Flux running in a local cluster on my MacBook by following the getting started guide. Then I learn about how to structure the repo according to Flux methodology. I implement this structure in my homelab repo and deploy flux to my homelab cluster. Then I manage to configure Grafana and the Weave UI to be accessbible via ingress using a custom fake domain.

Notes:

* kustomization resources live in the cluster
* source resources also live in the cluster
* To suspend updates for a kustomization, run the command flux suspend kustomization <name>.
* To resume updates run the command flux resume kustomization <name>.
* use `flux reconcile source git podinfo` to force a sync, nn waiting

* love the fact that helm releases are still accessible and visible on the cluster
* flux is just managing helm for you based on code
* love the fact that weave dashboard is also part of the cluster manifests

* learned about Kubernetes controllers
* an operator is a controller
* basically everything that enables CRD's to run is a controller

"A kubernetes controller is a control loop that watches the shared state of the cluster through the apiserver and makes changes attempting to move the current state towards the desired state"

## Sources

>A Source defines the origin of a repository containing the desired state of the system and the requirements to obtain it (e.g. credentials, version selectors). For example, the latest 1.x tag available from a Git repository over SSH.

>All sources are specified as Custom Resources in a Kubernetes cluster, examples of sources are GitRepository, OCIRepository, HelmRepository and Bucket resources.

https://fluxcd.io/flux/concepts/#sources

## Weave UI

Follow this guide

https://docs.gitops.weave.works/docs/installation/weave-gitops/

* sources are located in the clusters directory in a monorepo structure

## Repo

Decided to fully commit to Flux and their practices.

Set up the repo according to this guide:

https://fluxcd.io/flux/guides/repository-structure/

And following this example:

https://github.com/fluxcd/flux2-kustomize-helm-example


## Links:

202312261612

https://kubernetes.io/docs/concepts/architecture/controller/



