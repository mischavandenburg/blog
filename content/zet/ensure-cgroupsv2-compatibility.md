---
title: Ensure Cgroupsv2 compatiblity when containerizing old apps
date: 2023-12-12
tags:
- Kubernetes
- cloud-native
- Azure
---

Are you currently working with containerizing older Java or .NET applications? From Kubernetes 1.29, the default cgroups implementation on Azure Linux AKS nodes will be cgroupsv2. Older versions of Java, .NET and NodeJS do not support memory querying v2 memory constraints and this will lead to out of memory (OOM) issues for workloads.

Please make sure that your older containerized applications are compatible with cgroupsv2 or you might be in quite some pain in the future. 

https://github.com/Azure/AKS/releases/tag/2023-11-28


## Links:

202312120812
