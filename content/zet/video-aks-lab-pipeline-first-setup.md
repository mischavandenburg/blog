---
title: "Video: Setting Up A Simple Azure Pipeline To Deploy A Keyvault"
date: 2023-06-30
tags:
- Video
- AKS
- Kubernetes
- Azure
- DevOps
---

# Setting Up A Simple Azure Pipeline To Deploy A Keyvault

{{< youtube WnA8V3uq7P8 >}}

* Write KeyVault template
* Write pipeline code
* set up Azure DevOps pipeline

# Lessons Learned

* Always make sure to use `az deployment group` instead of `az group deployment`
* Because it has older Bicep version and will be deprecated
* Make sure to be in correct Directory to be able to sync subscriptions for service connection

## Links:

202306302206

https://youtu.be/WnA8V3uq7P8
