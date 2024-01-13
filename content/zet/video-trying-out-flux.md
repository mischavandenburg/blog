---
title: "Trying out Flux"
date: 2023-12-26
tags:
- Kubernetes
- Flux
- Azure
draft: true
---

In the first part of the video I tried out Flux for the first time in combination with an AKS cluster. I was a little overwhelmed. I expected it to be very similar to ArgoCD but found things are done quite differently in Flux.

* flux requires Kustomization files
* not as straight forward to deploy helm charts directly from repo compared to ArgoCD
* no separate flux UI, azure portal IS the flux ui
* azure portal UI significantly slower than ArgoCD

Powerful:

* learned you can provision flux configurations with bicep
* this is enough for me to get sold on Flux
* possible to roll out AKS cluster WITH GitOps configuration entirely from code?!

After taking a break in the middle of the video I came back refreshed. During my break I decided to commit to learning Flux and to start using it in my homelab. 

## Reasons for learning flux

* I committed earlier to becoming an expert in the Microsoft ecosystem
* Microsoft chose Flux so it is likely that clients will be using Flux
* likley that there will be a Microsoft-first policy in greenfield projects
* this means using flux
* Flux is integrated in Azure


## Learning about Flux

* fluxConfigurations exist as resources in Azure. These are the first element you configure in the GitOps UI from AKS
* fluxConfigurations are associated with one Flux GitRepository custom resource and one or more Kustomization custom resources in a Kubernetes cluster
* use Cluster scope, namespace scope needs [additional configuration](https://learn.microsoft.com/en-us/azure/azure-arc/kubernetes/conceptual-gitops-flux2#multi-tenancy)

### Kustomization

https://learn.microsoft.com/en-us/azure/azure-arc/kubernetes/gitops-flux2-parameters#kustomization

>Kustomization is a setting created for Flux configurations that lets you choose a specific path in the source repo that is reconciled into the cluster. You don't need to create a `kustomization.yaml` file on this specified path. 

>By default, all of the manifests in this path are reconciled. However, if you want to have a Kustomize overlay for applications available on this repo path, you should create Kustomize files in git for the Flux configuration to make use of.

So a Kustomization in Azure is nothing more than a configuration saying "apply all the manifests found in the directory in this GitHub repo

### Flux and Helm

Normally you run helm install from your local machine. The repo is pulled to your local machine and a helm release is created on the cluster.

With Flux you have two CRDs:

* HelmRepository
* HelmRelease

The helm repository is equivalent to the `helm repo add` command locally. Now the repo config lives in the k8s cluster itself.

Then you create a HelmRelease file which points to the repo / chart and specifies which version you wish to deploy.

Custom values are given in the HelmRelease file. These can be added directly, or they can be read from config maps or secrets.

I really like that you can read from secrets. This way you can add custom secrets to almost any helm chart. I've had some problems with this recently where the chart did not support secret values which needed to be checked in to version control.

You can do auto updating of helm charts!

>If you want Helm Controller to automatically upgrade your releases when a new chart version is available in the release’s referenced HelmRepository, you may specify a SemVer range (i.e. >=4.0.0 <5.0.0) instead of a fixed version.





## Links:

202312261112
