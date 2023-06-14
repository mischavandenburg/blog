---
title: What is Azure CNI Overlay for AKS?
date: 2023-06-14
tags:
- Azure
- Kubernetes
- AKS
- Networking
- DevOps
---

# CNI?

CNI stands for Container Network Interface. It allows communication between pods and services.

## Current Azure CNI limitations

Let's take a practical example. We have an enterprise environment where a large network is utilized, spanning multiple clouds and on-prem infrastructure hubs. To enable seamless communication across these sections, they must belong to the same network. As a result, specific IP address ranges are assigned to each section, with AWS, On-Prem A, and Azure each having their respective ranges.

Let's say Azure is assigned the following ranges:

10.60.0.0/16

10.61.0.0/16

10.62.0.0/16

This means that the networks in each of these ranges would have a maximum posisble amount of 65534 addresses per range.

With the current Azure CNI (i.e. the non-overlay version), all pods are assigned an IP address from one of these ranges. It also uses direct VNet routing.  Since the pods use VNet IP's, there is a maximum of 65.000 pods per cluster. In other words, there is a risk for IP exhaustion, which limits the scalability of your workloads. Moreover, pod subnets cannot be shared across clusters.

It is crucial to carefully plan the number of pods you expect to deploy. If the required number of IP addresses exceeds the available addresses in the subnet, you will not be able to run your pods.

Now, these ranges are large and you can anticipate the growth of your resources. For now we are fine. But to design an infrastructure which is truly scalable and extendable, you will need to look into different options. This is where the Azure CNI Overlay comes in. 

## Benefits of Azure CNI Overlay

An Overlay network is an abstracted, virtual network which is put on top of your current network infrastructure. Nodes are assigned IP addresses from the VNets that they are deployed in, but pods get assigned IP addresses from the Overlay network.

Pods are assigned addresses from a private CIDR which is logically separate from the VNet hosting the nodes. They do not use up the IP addressess of the VNets, which means that your workloads become nearly infinitely scalable within your assigned IP address ranges when you are operating in this type of corporate networking infrastructure with IP range limitations. You can scale up to literally thousands of nodes without worrying about IP exhaustion.

![](/cnioverlay.png)

Additionally, the Overlay network can also span across multiple AKS clusters. This opens up a whole world of possibilities where pods from separate workloads on separate clusters could communicate with each other directly using the high speed native direct routing of the Azure network.

In conclusion, the Azure CNI Overlay provides a powerful solution to address the challenges of IP exhaustion and scalability in Azure AKS. By implementing the overlay network, organizations can overcome the limitations of the non-overlay version of Azure CNI and achieve a truly scalable and manageable infrastructure.

Azure CNI Overlay is currently in preview, but I'm very excited about the developments. I'll be following them closely and I hope to be a part of its implementation at my current contract.

# Links:

https://learn.microsoft.com/en-us/azure/aks/azure-cni-overlay

202306131506

[[aks-networking-essentials]]
