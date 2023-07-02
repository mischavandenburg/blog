---
Title: How To Generate Random Strings In Bicep
date: 2023-07-02
tags:
- Azure
- Bicep
- Coding
---

{{< youtube n9MRNnAJjZY >}}

In this video I explain how to generate random strings in Bicep.

I use the uniqueString function combined with the utcNow function. But the caveat is that you can only use it as a default value for a parameter, as follows:

param keyVaultName string = 'key-vault-${uniqueString(resourceGroup().id, utcNow())}'

I demonstrate with a few deployments.

## Links:

202307022107

[[bicep]]
[[coding]]
