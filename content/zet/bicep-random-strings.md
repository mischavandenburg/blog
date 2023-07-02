---
Title: How To Generate Random Strings In Bicep
date: 2023-07-02
tags:
- Azure
- Bicep
- Coding
---

{{< youtube n9MRNnAJjZY >}}

In this video I explain how to generate random strings in Bicep and demonstrate a couple of deployments.

I use the uniqueString function combined with the utcNow function. But the caveat is that you can only use it as a default value for a parameter, as follows:

`param keyVaultName string = 'key-vault-${uniqueString(resourceGroup().id, utcNow())}'`

`uniqueString()` doesn't really generate a unique string. It is a hash function which generates a 13 character string based on the input you give it. so `uniqueString('coffee')` will always have the same output. 

To generate a truly unique string, you pass in the `utcNow()` function which generates a UNIX timestamp. However, Bicep does not allow you to use this function everywhere. It may only be used as a default value for a parameter.

## Links:

202307022107

https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/bicep-functions-string#uniquestring

[[bicep]]
[[coding]]
