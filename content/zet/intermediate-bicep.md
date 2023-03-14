---
title: "Notes: Intermediate Bicep"
date: 2023-03-14
tags:
- Zettelkasten
- Bicep
- Azure
- Coding
- Study
---

Today I finished the Intermediate Bicep module. Here are my notes.

# Child and Extension

> You can also use Bicep to refer to resources that were created outside the Bicep file itself. For example, you can refer to resources that your colleagues have created manually by using the Azure portal, or from within another Bicep template or module, even if they're in a different resource group or subscription. By using these features of Bicep, you can unlock the ability to create powerful templates that deploy all aspects of your Azure infrastructure.

Some resources are only deployed in context of their parent. For example:

Virtual network subnets 	Microsoft.Network/virtualNetworks/subnets
App Service configuration 	Microsoft.Web/sites/config
SQL databases 	Microsoft.Sql/servers/databases
Virtual machine extensions 	Microsoft.Compute/virtualMachines/extensions
Storage blob containers 	Microsoft.Storage/storageAccounts/blobServices/containers
Azure Cosmos DB containers

It does not make sense for a container to exist without a storage account. 

## Difference between child and extension

In summary, you define extensions with the `scope` keyword, and child resources are defined by nesting them or by using the `parent` keyword.

An extension resource is a resource that modifies another resource. For example, assigning a role to a resource. 

A child resource is a resource that exists only within the context of another resource, such as a subnet existing only within a vnet.


## Nested resource

```bicep
resource vm 'Microsoft.Compute/virtualMachines@2020-06-01' = {
  name: vmName
  location: location
  properties: {
    // ...
  }

  resource installCustomScriptExtension 'extensions' = {
    name: 'InstallCustomScript'
    location: location
    properties: {
      // ...
    }
  }
}
```

Here the extension resource is within the vm resource. The fully qualified domain name is `Microsoft.Compute/virtualMachines/extensions`, but it is not necessary because it inherits it from the parent. Therefore we only need to specify 'extensions' here.

No API version is specified either, this is also inherited.

> You can refer to a nested resource by using the :: operator. For example, you could create an output that will return the full resource ID of the extension:

```bicep
output childResourceId string = vm::installCustomScriptExtension.id
```

## Parent property

This is the second way to declare a child resource.

```bicep
resource vm 'Microsoft.Compute/virtualMachines@2020-06-01' = {
  name: vmName
  location: location
  properties: {
    // ...
  }
}

resource installCustomScriptExtension 'Microsoft.Compute/virtualMachines/extensions@2020-06-01' = {
  parent: vm
  name: 'InstallCustomScript'
  location: location
  properties: {
    // ...
  }
}
```

## dependsOn

use dependsOn to indicate a dependency. 

```bicep
dependsOn: [
  vm
]
```
## Extension resources

Extension resources are always attached to other Azure resources. They extend them with extra functionality. Some examples are role assignments, locks, and policy assignments. 

It doesn't make sense to deploy a lock by itself. It always has to be deployed to another resource, because it prevents deletion or modification of a resource.


Resources are defined almost the same way as normal resources, but you add the `scope` property to tell Bicep that it is attached to another resource in the template. 

```bicep
resource cosmosDBAccount 'Microsoft.DocumentDB/databaseAccounts@2020-04-01' = {
  name: cosmosDBAccountName
  location: location
  properties: {
    // ...
  }
}

resource lockResource 'Microsoft.Authorization/locks@2016-09-01' = {
  scope: cosmosDBAccount
  name: 'DontDelete'
  properties: {
    level: 'CanNotDelete'
    notes: 'Prevents deletion of the toy data Cosmos DB account.'
  }
}
```

Extensions have slightly different resource ID's. They consist of the parent resource ID, the separator `/providers/`, and the extension resource ID. 

> If you see a resource ID that starts with a normal resource ID and then adds /providers/ and another resource type and name, it means that you're looking at an extension resource ID.

### Existing resources

Bicep files often need to refer to resources that have been already created elswewhere. They might have been created in the portal or by another Bicep file. 

Here you use the `existing` keyword in Bicep. You are defining a resource that already exists, and therefore you are telling Bicep that it shouldn't try to deploy it. Think of it as a placeholder resource.

You can do the same for nested or child resources.

```bicep
resource storageAccount 'Microsoft.Storage/storageAccounts@2019-06-01' existing = {
  name: 'toydesigndocs'
}
```

### Existing resources outside of the resource group and subscription

```bicep
resource vnet 'Microsoft.Network/virtualNetworks@2020-11-01' existing = {
  scope: resourceGroup('f0750bbe-ea75-4ae5-b24d-a92ca601da2c', 'networking-rg')
  name: 'toy-design-vnet'
}
```

You can refer to these as long as they are within your Azure AD tenant.

## Add child and extension resources to an existing resource

```bicep
resource server 'Microsoft.Sql/servers@2020-11-01-preview' existing = {
  name: serverName
}

resource database 'Microsoft.Sql/servers/databases@2020-11-01-preview' = {
  parent: server
  name: databaseName
  location: location
  sku: {
    name: 'Standard'
    tier: 'Standard'
  }
}
```

Use the `existing` keyword to refer to the resource, and then you add the child by specifying the `parent` property. 

Finally, to deploy an extension resource to an existing resource, use the `scope` keyword:

```bicep
resource storageAccount 'Microsoft.Storage/storageAccounts@2019-06-01' existing = {
  name: 'toydesigndocs'
}

resource lockResource 'Microsoft.Authorization/locks@2016-09-01' = {
  scope: storageAccount
  name: 'DontDelete'
  properties: {
    level: 'CanNotDelete'
    notes: 'Prevents deletion of the toy design documents storage account.'
  }
}
```

## Referring to an existing resource's properties

Define the resource and you can refer to its properties if the prperty isn't secure.

```bicep
resource applicationInsights 'Microsoft.Insights/components@2018-05-01-preview' existing = {
  name: applicationInsightsName
}

resource functionApp 'Microsoft.Web/sites@2020-06-01' = {
  name: functionAppName
  location: location
  kind: 'functionapp'
  properties: {
    siteConfig: {
      appSettings: [
        // ...
        {
          name: 'APPINSIGHTS_INSTRUMENTATIONKEY'
          value: applicationInsights.properties.InstrumentationKey
        }
      ]
    }
  }
}
```

When you need to access secure data, use the listKeys() function.

```bicep
resource storageAccount 'Microsoft.Storage/storageAccounts@2019-06-01' existing = {
  name: storageAccountName
}

resource functionApp 'Microsoft.Web/sites@2020-06-01' = {
  name: functionAppName
  location: location
  kind: 'functionapp'
  properties: {
    siteConfig: {
      appSettings: [
        // ...
        {
          name: 'StorageAccountKey'
          value: storageAccount.listKeys().keys[0].value
        }
      ]
    }
  }
}
```
The VScode extension will show hints to help you understand the data this function returns.

You need to have sufficient permissions to use the listKeys function.


## Child and extension

In this example we are attaching an extension to a child resource. 

```bicep
resource storageAccount 'Microsoft.Storage/storageAccounts@2019-06-01' existing = {
  name: storageAccountName

  resource blobService 'blobServices' existing = {
    name: 'default'
  }
}

/* Note: Here we are attaching to blobServices which itself is a child resource. So we are attaching an extension to a child resource. */
resource storageAccountBlobDiagnostics 'Microsoft.Insights/diagnosticSettings@2017-05-01-preview' = {
  scope: storageAccount::blobService
  name: storageAccountBlobDiagnosticSettingsName
  properties: {
    workspaceId: logAnalyticsWorkspace.id
    logs: [
      {
        category: 'StorageRead'
        enabled: true
      }
      {
        category: 'StorageWrite'
        enabled: true
      }
      {
        category: 'StorageDelete'
        enabled: true
      }
    ]
  }
}
```
# Structuring Bicep for Collaboration

## configuration maps

Using many different parameters can be confusing to the user of the template you're writing. One way of solving this is by creating a config map:

```bicep
@allowed([
  'Production'
  'Test'
])
param environmentType string = 'Test'

var environmentConfigurationMap = {
  Production: {
    appServicePlan: {
      sku: {
        name: 'P2V3'
        capacity: 3
      }
    }
    storageAccount: {
      sku: {
        name: 'ZRS'
      }
    }
  }
  Test: {
    appServicePlan: {
      sku: {
        name: 'S2'
        capacity: 1
      }
    }
    storageAccount: {
      sku: {
        name: 'LRS'
      }
    }
  }
}

resource appServicePlan 'Microsoft.Web/serverfarms@2020-06-01' = {
  name: appServicePlanName
  location: location
  sku: environmentConfigurationMap[environmentType].appServicePlan.sku
}
```

Here we are taking a parameter for the environment, but create an object that contains the settings for that particular environment. Note that we are accessing it like we'd access a dictionary in python: `sku: environmentConfigurationMap[environmentType].appServicePlan.sku`

## Naming

>In Bicep, you ordinarily use camelCase capitalization style for the names of parameters, variables, and resource symbolic names.

Resource names cannot be renamed after they're deployed in Azure.

## Comments

use // for single line comments, and /* */ for multi-line comments.

When adding comments to JSON files, you might have to save the file as jsonc to let the code editor know that comments are allowed.
