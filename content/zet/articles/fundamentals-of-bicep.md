---
title: "Notes: Fundamentals of Bicep"
date: 2023-03-13
tags:
- Article
- Study
- Azure
- Coding
- Bicep
---

I'll be working with Bicep during my next contract, so I'm working through the Bicep modules on Microsoft Learn to prepare. I must say that these modules are particularly helpful. They are well structured and they provide you with free sandbox environments to practice deploying the templates you create.

# Why Bicep?

Resources in Azure are deployed by the Azure Resource Manager (ARM). These resources are JSON objects under the covers, and ARM templates are a way to generate these JSON objects. However, JSON is not really meant to be edited by humans, and the ARM templates are not very suitable for editing either. Thus, Bicep was developed to allow for a better editing experience and better readability and reusability.

Bicep templates are transpiled into JSON objects, which are sent to the Azure API to create resources with the Azure Resource Manager.

# Fundamentals of Bicep Notes

>A _parameter_ lets you bring in values from outside the template file. For example, if someone is manually deploying the template by using the Azure CLI or Azure PowerShell, they'll be asked to provide values for each parameter. They can also create a _parameter file_, which lists all of the parameters and values they want to use for the deployment. If the template is deployed from an automated process like a deployment pipeline, the pipeline can provide the parameter values.

> A _variable_ is defined and set within the template. Variables let you store important information in one place and refer to it throughout the template without having to copy and paste it.

## generating unique names

>Bicep has another function called `uniqueString()` that comes in handy when you're creating resource names. When you use this function, you need to provide a _seed value_, which should be different across different deployments but consistent across all deployments of the same resources.

```bicep
param storageAccountName string = uniqueString(resourceGroup().id)
```

-   Every time you deploy the same resources, they'll go into the same resource group. The `uniqueString()` function will return the same value every time.
-   If you deploy into two different resource groups in the Azure subscription, the `resourceGroup().id` value will be different, because the resource group names will be different. The `uniqueString()` function will give different values for each set of resources.
-   If you deploy into two different Azure subscriptions, _even if you use the same resource group name_, the `resourceGroup().id` value will be different because the Azure subscription ID will be different. The `uniqueString()` function will again give different values for each set of resources.

### combining strings

Can use string interpolation to generate a unique string with a recognizable hardcoded part:

`param storageAccountName string = 'toylaunch${uniqueString(resourceGroup().id)}'`

This can also be handy for generating correct names. For example, storage accounts may not begin with a number. 

## parameter decorators

### allowed parameters

```bicep
@allowed([
  'nonprod'
  'prod'
])
param environmentType string
```

The template cannot be deployed unless the nonprod or prod values are provided. 

@allowed is a *parameter decorator*: it gives Bicep information on what the parameter's value needs to be. 

You can also specify the allowed length of the parameter by using the following decorators:

```bicep
@minLength(5)
@maxLength(24)
param storageAccountName string
```

You can apply multiple decorators to a parameter by putting each on a separate line.

These min and maxLength decorators can also be used to limit the length of an array.

To limit int values:

```bicep
@minValue(1)
@maxValue(10)
param appServicePlanInstanceCount int
```

Finally, you can add descriptions to your parameters with the @description decorator:

```bicep
@description('The locations into which this Cosmos DB account should be configured. This parameter needs to be a list of objects, each of which has a locationName property.')
param cosmosDBAccountLocations array
```

## if statements

```bicep
var storageAccountSkuName = (environmentType == 'prod') ? 'Standard_GRS' : 'Standard_LRS'
var appServicePlanSkuName = (environmentType == 'prod') ? 'P2V3' : 'F1'
```

Let's unpack this:

? is a *ternary operator* and evaluates an if/then statement. The value after ? is used if the expression is true. If it's false, the value after : is used. 

So here, if the environmentType is prod, the SKU is set to Standard_GRS

## Objects in Bicep

You can use objects within resource definitions, within variables, or within expressions in your Bicep file.

Objects are the same as dictionaries in python:

```bicep
param appServicePlanSku object = {
  name: 'F1'
  tier: 'Free'
  capacity: 1
}
```

These are called "properties" of type string and int. Note that they are line separated, not comma separated like in python.

When referencing the parameter in the template, you can use dot notation to access the object properties:

```bicep
resource appServicePlan 'Microsoft.Web/serverfarms@2022-03-01' = {
  name: appServicePlanName
  location: location
  sku: {
    name: appServicePlanSku.name
    tier: appServicePlanSku.tier
    capacity: appServicePlanSku.capacity
  }
}
```

>[!important]
>Keep in mind that you don't specify the type of each property within an object. However, when you use a property's value, its type must match what's expected. In the previous example, both the name and the tier of the App Service plan SKU must be strings.

### Example: tags

```bicep
param resourceTags object = {
  EnvironmentName: 'Test'
  CostCenter: '1000100'
  Team: 'Human Resources'
}

resource appServicePlan 'Microsoft.Web/serverfarms@2022-03-01' = {
  name: appServicePlanName
  location: location
  tags: resourceTags
  sku: {
    name: 'S1'
  }
}

resource appServiceApp 'Microsoft.Web/sites@' = {
  name: appServiceAppName
  location: location
  tags: resourceTags
  kind: 'app'
  properties: {
    serverFarmId: appServicePlan.id
  }
}
```

Here we take the tags for all the resources of the template as parameters. But we easily reuse all the tags for each resource by referencing the entire object.

## Arrays

Arrays are not typed in Bicep. You cannot specify that it must contain strings.

Example:

```bicep
param cosmosDBAccountLocations array = [
  {
    locationName: 'australiaeast'
  }
  {
    locationName: 'southcentralus'
  }
  {
    locationName: 'westeurope'
  }
]
```

This is an array of objects, which have an locationName property each. 

And you would access it by:

```bicep
resource account 'Microsoft.DocumentDB/databaseAccounts@2022-08-15' = {
  name: accountName
  location: location
  properties: {
    locations: cosmosDBAccountLocations
  }
}
```

## Specifying parameter values

When deploying a template file there are three options:

1. default values
2. command line
3. parameter file

### Parameter file

This is a json file. To deploy a template with a paramter file, use: 

```bash
az deployment group create \
  --template-file main.bicep \
  --parameters main.parameters.json
```

### priority

The order of priority is this, from high to low priority:

1. Parameters specified on the command line
2. Parameter file
3. Default values in template

## Securing parameters

It is best to use Managed Identities for Azure, but if you need to supply secret values to a deployment, use the @secure() decorator. These values aren't available in the deployment logs, and they won't be displayed on the screen when entered in the terminal.

## Loops

Defined with the for keyword. Usually you iterate over an array to create multiple instances of a resource.

### Copy loops

```bicep
param storageAccountNames array = [
  'saauditus'
  'saauditeurope'
  'saauditapac'
]

resource storageAccountResources 'Microsoft.Storage/storageAccounts@2021-09-01' = [for storageAccountName in storageAccountNames: {
  name: storageAccountName
  location: resourceGroup().location
  kind: 'StorageV2'
  sku: {
    name: 'Standard_LRS'
  }
}]
```
Notice that bicep requires "[" before the for, and a closing bracket.

### count loops

```bicep
resource storageAccountResources 'Microsoft.Storage/storageAccounts@2021-09-01' = [for i in range(1,4): {
  name: 'sa${i}'
  location: resourceGroup().location
  kind: 'StorageV2'
  sku: {
    name: 'Standard_LRS'
  }
}]
```
The range function takes two arguments. The first one specifies the starting value, and the second tells Bicep the number of values you want. 

If you use range(3,4), you will get 3, 4, 5 and 6. 

#### accessing the index

```bicep
param locations array = [
  'westeurope'
  'eastus2'
  'eastasia'
]

resource sqlServers 'Microsoft.Sql/servers@2021-11-01-preview' = [for (location, i) in locations: {
  name: 'sqlserver-${i+1}'
  location: location
  properties: {
    administratorLogin: administratorLogin
    administratorLoginPassword: administratorLoginPassword
  }
}]
```

The first value is zero, so you can add 1 to i if you want your names to be sqlserver-1, sqlserver-2 etc.

i is used here, but you can use any value you want.

### Filtering with loops

```bicep
param sqlServerDetails array = [
  {
    name: 'sqlserver-we'
    location: 'westeurope'
    environmentName: 'Production'
  }
  {
    name: 'sqlserver-eus2'
    location: 'eastus2'
    environmentName: 'Development'
  }
  {
    name: 'sqlserver-eas'
    location: 'eastasia'
    environmentName: 'Production'
  }
]

resource sqlServers 'Microsoft.Sql/servers@2021-11-01-preview' = [for sqlServer in sqlServerDetails: if (sqlServer.environmentName == 'Production') {
  name: sqlServer.name
  location: sqlServer.location
  properties: {
    administratorLogin: administratorLogin
    administratorLoginPassword: administratorLoginPassword
  }
  tags: {
    environment: sqlServer.environmentName
  }
}]
```

This will deploy -we and -eas, but not -eus2, because the environmentName does not match Production.

### Controlling loop execution

By default all the iterations of a loop are executed simultaneously. However, you don't always want this to be happening. 

To control the amount you can use the @batchSize decorator. 

```bicep
@batchSize(2)
resource appServiceApp 'Microsoft.Web/sites@2021-03-01' = [for i in range(1,3): {
  name: 'app${i}'
  // ...
}]
```
Here bicep will wait for the first two to be fully completed before it moves to the next.

To loop sequentially, meaning one at a time in order, use @batchSize(1)

### Variable loops

You can use loops to create arrays that you can use in the Bicep template.

```bicep
var items = [for i in range(1, 5): 'item${i}']
```

This produces an array containing the values item1, item2 up to 5 stored in the items variable.

Reminds me of list comprehensions in python.


Here is an example:

```bicep
param addressPrefix string = '10.10.0.0/16'
param subnets array = [
  {
    name: 'frontend'
    ipAddressRange: '10.10.0.0/24'
  }
  {
    name: 'backend'
    ipAddressRange: '10.10.1.0/24'
  }
]

var subnetsProperty = [for subnet in subnets: {
  name: subnet.name
  properties: {
    addressPrefix: subnet.ipAddressRange
  }
}]

resource virtualNetwork 'Microsoft.Network/virtualNetworks@2021-08-01' = {
  name: 'teddybear'
  location: resourceGroup().location
  properties:{
    addressSpace:{
      addressPrefixes:[
        addressPrefix
      ]
    }
    subnets: subnetsProperty
  }
}
```

The content of the subnetsProperty array would look like this:

```bicep
[
  {
    name: 'frontend',
    properties: {
      addressPrefix: '10.10.0.0/24'
    }
  },
  {
    name: 'backend',
    properties: {
      addressPrefix: '10.10.1.0/24'
    }
  }
]
```

### Output loops

To output the contents of the array:

```bicep
var items = [
  'item1'
  'item2'
  'item3'
  'item4'
  'item5'
]

output outputItems array = [for i in range(0, length(items)): items[i]]
```

## Modules

You can create modules in Bicep so the code becomes reusable. You can share the modules with other teams and use them for different outcomes. 

> Generally, it's not a good practice to create a module for every resource that you deploy. A good Bicep module typically defines multiple related resources. However, if you have a particularly complex resource with a lot of configuration, it might make sense to create a single module to encapsulate the complexity. This approach keeps your main templates simple and uncluttered.

So for example it would make sense to write a networking module and a database module that handles these resources.

Modules can be nested, but it can quickly become very complex.

To call a module in a template:

```bicep
module appModule 'modules/app.bicep' = {
  name: 'myApp'
  params: {
    location: location
    appServiceAppName: appServiceAppName
    environmentType: environmentType
  }
}
```

The modules are stored in the modules folder in your root directory.

### Parameters

Modules will take parameters, but it is good practice to leave out default values for parameters in modules. In templates it's good practice to add defaults wherever you can. Therefore it is best to leave them out in modules because templates usually have their own default values. This can get confusing if you have similar default values in the templates and modules.

### Module dependency

Bicep will figure out automatically if there is a dependency between modules. For example:

```bicep
@description('Username for the virtual machine.')
param adminUsername string

@description('Password for the virtual machine.')
@minLength(12)
@secure()
param adminPassword string

module virtualNetwork 'modules/vnet.bicep' = {
  name: 'virtual-network'
}

module virtualMachine 'modules/vm.bicep' = {
  name: 'virtual-machine'
  params: {
    adminUsername: adminUsername
    adminPassword: adminPassword
    subnetResourceId: virtualNetwork.outputs.subnetResourceId
  }
}
```

Here the virtualMachine module takes the subnetResourceId from the virtualNetwork module outputs.

Because it is defined like this, Bicep will wait with deploying the virtualMachine modul until the virtualNetwork module is finished, and pass in the required parameter.

It is important to note that this means that it will wait until the virtualNetwork module is completely finished. If it takes a long time to deploy the previous module, all the subsequent modules will have to wait until it's finished.

