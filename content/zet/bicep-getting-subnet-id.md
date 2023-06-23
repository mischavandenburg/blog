---
Title: How To Get The Id Of An Existing Subnet In Bicep
date: 2023-06-23
tags:
- Bicep
- Coding
- Azure
---

Did a refactor of some of our Bicep template code for our AKS clusters today.

Before, we were using a rather complicated line of code using string interpolation.

```bicep
var vnetSubnetId = '${resourceId(vnetResourceGroupName, 'Microsoft.Network/virtualNetworks', vnetName)}/subnets/${vnetSubnetName}'
```

This was hard to read and the Bicep linter gave the following warning in my editor and during deployment:

```
WARNING: D:\a\1\a\drop\Generic-templates\containers\azure-kubernetes-service\v4.0\templates\aks.bicep(117,7) : Warning use-resource-id-functions: If property "vnetSubnetID" represents a resource ID, it must use a symbolic resource reference, be a parameter or start with one of these functions: extensionResourceId, guid, if, reference, resourceId, subscription, subscriptionResourceId, tenantResourceId. Found nonconforming expression at vnetSubnetID -> vnetSubnetId [https://aka.ms/bicep/linter/use-resource-id-functions]
```

## Using Existing Resources

After some experimentation I managed to fix it as follows:

```bicep
@description('Virtual Network name')
param vnetName string

@description('Virtual Network resource group name')
param vnetResourceGroupName string

@description('Virtual Network subnet name')
param vnetSubnetName string

// Import the existing vnet and subnet to get the subnet id for deployment
resource vnet 'Microsoft.Network/virtualNetworks@2022-11-01' existing = {
  name: vnetName
  scope: resourceGroup(vnetResourceGroupName)
}

resource subnet 'Microsoft.Network/virtualNetworks/subnets@2022-11-01' existing = {
  name: vnetSubnetName
  parent: vnet
}
```

Interestingly, subnets are not actually a resource in Azure. When you open a Resource Group, it will not be listed under resources. However, they do get a resource id. This posed a challenge, but I solved it by importing the existing VNet as well and setting it as the parent of the subnet.

Now I can access the subnet id by using `subnet.id`. I need this later when deploying the AKS cluster itself.

## Links:

202306231206

https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/existing-resource

https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/linter-rule-use-resource-id-functions

[[bicep]]
[[coding]]
[[azure]]
