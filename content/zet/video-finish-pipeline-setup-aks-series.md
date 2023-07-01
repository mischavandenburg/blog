---
title: "Video: Finishing Pipeline Setup  & Working on KeyVault Template - Azure Kubernetes Lab Series"
date: 2023-06-30
tags:
- AKS
- Kubernetes
- Bicep
- Coding
- Video
- DevOps
- Azure
---

{{< youtube eooZ3OHl5Mc >}}

* Finish deploying keyvault using pipeline
* Get the random name generation to work

# Lessons Learned

* Subscriptions need to be registered with resource providers, apparently

https://learn.microsoft.com/en-us/azure/azure-resource-manager/troubleshooting/error-register-resource-provider?tabs=azure-cli

* acccesPolicies are mandatory on KeyVaults, but not when RBAC is enabled
* Assign contributor role to Azure DevOps service connection to be able to create resource groups from pipeline

# Achieved

* Setting up connection between pipeline and Azure subscription
* Assign correct rights to the service connection so it is allowed to deploy new resource groups (and other resources)
* Learned about provider registrations
* Made progress on creating unique names for resources
* Successfully deployed new resource group and key vault from the pipeline

# Next time:

Look into random string creation with utcNow

https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/bicep-functions-date#utcnow

or newGuid

https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/bicep-functions-string#newguid

Links:

202306281806

## Links:

202306302206

https://youtu.be/eooZ3OHl5Mc
