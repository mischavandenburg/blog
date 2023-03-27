---
title: "Notes: Advanced Bicep"
date: 2023-03-18
tags:

- Bicep
- Azure
- Coding
- Study
---

# Deploying to subscriptions and management groups

To tell Bicep which scope to deploy to, use the `targetScope` keyword, for example, managementGroup.

You're not specifying which management group exactly, this is done during deployment of the template file. 

targetScope can be set to resourceGroup, subscription, managementGroup or tenant.

If it is not set, Bicep assumes resourceGroup.

## create a resource group

```bicep
targetScope = 'subscription'

resource resourceGroup 'Microsoft.Resources/resourceGroups@2021-01-01' = {
  name: 'example-resource-group'
  location: 'westus'
}
```

To deploy you use `az deployment group create` for resource groups, but you use `az deployment sub create` for subscriptions, `mg` for management group and `tenant` for tenant.

## deployment scripts

> deploymentScripts resources are either PowerShell or Bash scripts that run in a Docker container as part of your template deployment. The default container images have either the Azure CLI or Azure PowerShell available. These scripts run during the processing of the ARM template, so you can add custom behavior to the deployment process.

Here is an example of a deployment script with some comments:

```bicep
resource myFirstDeploymentScript 'Microsoft.Resources/deploymentScripts@2020-10-01' = {
  name: 'myFirstDeploymentScript'
  location: resourceGroup().location

  // can be AzurePowershell or Azure CLI
  kind: 'AzurePowerShell'

  // the script will be run in a container. We need to provide a Managed Identity to give the script the required permissions
  identity: {
    type: 'UserAssigned'
    userAssignedIdentities: {
      '/subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourcegroups/deploymenttest/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myscriptingid': {}
    }
  }
  properties: {
    azPowerShellVersion: '3.0'

    // in Bicep we use ''' to indicate a multi line string
    scriptContent: '''
      $output = 'Hello Learner!'
      Write-Output $output

     // the $DeploymentScriptOutputs variable is created to return output back to the Bicep template
     // It needs to be a hash table
      $DeploymentScriptOutputs = @{}
      $DeploymentScriptOutputs['text'] = $output
    '''
    retentionInterval: 'P1D'
  }
}


output scriptResult string = myFirstDeploymentScript.properties.outputs.text
```

> You can also write deployment scripts in Bash. To create outputs from a Bash script, you need to create a JSON file in a location specified by the AZ_SCRIPTS_OUTPUT_PATH environment variable.

To include a script file, use the following:

```bicep
properties: {
  azPowerShellVersion: '3.0'
  scriptContent: loadTextContent('myscript.ps1')
  retentionInterval: 'P1D'
}
```
## deploying a managed identity and assigning a role

```bicep
var userAssignedIdentityName = 'configDeployer'
var roleAssignmentName = guid(resourceGroup().id, 'contributor')
var contributorRoleDefinitionId = resourceId('Microsoft.Authorization/roleDefinitions', 'b24988ac-6180-42a0-ab88-20f7382dd24c')

resource userAssignedIdentity 'Microsoft.ManagedIdentity/userAssignedIdentities@2018-11-30' = {
  name: userAssignedIdentityName
  location: resourceGroup().location
}

resource roleAssignment 'Microsoft.Authorization/roleAssignments@2020-04-01-preview' = {
  name: roleAssignmentName
  properties: {
    roleDefinitionId: contributorRoleDefinitionId
    principalId: userAssignedIdentity.properties.principalId
    principalType: 'ServicePrincipal'
  }
}

```
## template specs

When you have a lot of reusable templates, you can use Template Specs to enable your entire organization to deploy them.

You can convert a Bicep file to a template spec. The template spec is then deployed to Azure as a resource, and anybody with the right access and do deployments with the template spec from the portal or Azure CLI. Azure will handle the version control. 

You will lose any comments and whitespace.

Bicep modules are intended to be combined into larger deployments. Template specs are for sets of resources with a certain configuration.

Template specs can be used as a Bicep module. You use the following code to import it:

```bicep
module storageAccountTemplateSpec 'ts:f0750bbe-ea75-4ae5-b24d-a92ca601da2c/sharedTemplates/StorageWithoutSAS:1.0' = {
  name: 'storageAccountTemplateSpec'
}
```

