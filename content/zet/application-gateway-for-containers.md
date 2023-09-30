---
title: Application Gateway for Containers
date: 2023-09-30
tags:
- Azure
- Networking
- Kubernetes
---

{{< youtube iXjb_L722VvqAjHZ >}}

# Frontend

## Azure

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

This is an Azure resource. It lives in the VNet and handles TLS and makes the connections to and from the pods and frontend IP. This is the data path.

# Support

Azure CNI. Does not support kubenet or Azure CNI overlay yet, but it will support in the future.

# Backend

## Links:

202309301009
