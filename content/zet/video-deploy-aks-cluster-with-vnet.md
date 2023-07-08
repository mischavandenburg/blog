---
title: deploying aks cluster with custom vnet
date: 2023-07-07
tags:
- Kubernetes
draft: true
---

* New AKS resource group
* Use a dev prefix
* start using modules
* Deploy VNET and one subnet
* Each node pool in separate subnet
* Azure CNI enabled

# VNet planning

VNet cidr: 10.224.0.0/16

Subnet cidr: 

10.224.0.0/16 - 254 hosts

servicecidr 10.0.0.0/16
dns servcie ip addres 10.0.0.10
dns name prefix

## Links:

202307071807
