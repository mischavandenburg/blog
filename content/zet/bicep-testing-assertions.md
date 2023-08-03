---
title: Test Driven Development for Bicep is In The Works!
date: 2023-08-03
tags:
- Coding
- Bicep
---

During the [Bicep Community Call of July 2023](https://youtu.be/UuwhLi-Xe2U) I was introduced to the new experimental testing framework that the Bicep team is working on. After learning the fundamentals of the Go programming language I saw the value of test driven software development, and it will be an advantageous improvement if we can start applying this methodology to Infrastructure as Code as well.

Test driven software development is a software development practice that involves writing unit tests before writing the actual code, and then refactoring the code to pass the tests. Some of the advantages of test driven software development are:

* It improves the code quality and design by making it more modular, maintainable, and extensible.
* It reduces the number of bugs and defects by catching them early in the development process.
* It enhances the documentation of the code by providing a clear specification of what each function or module does.
* It increases the productivity and efficiency of the developers by providing immediate feedback and reducing debugging time.
* It ensures high test coverage and makes it easier to add new features or refactor the code in the future.

# Enabling the Bicep testing framework

To make the "test" keyword available in your Bicep editor, you need to enable some experimental features.

If you're using VScode, open or generate your bicepconfig.

```json
{
    "experimentalFeaturesEnabled": {
      "testFramework": true,
      "asserts": true
    }
}
```

Now the "test" keyword should work.

For more information, check out this link:

https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/bicep-config

# Asserts

The assert keyword will be used to verify output prior to deployment.

Let's say I have this very simple key vault template:

```bicep
param location string = resourceGroup().location

resource kv 'Microsoft.KeyVault/vaults@2023-02-01' = {
  name: 'mischa-key-vault-138'
  location: location
  properties: {
    sku: {
      name: 'standard'
      family: 'A'
    }
    tenantId: subscription().tenantId
    enableRbacAuthorization: true
  }
}

assert kvSKU = kv.sku.name == 'standard'
```

In this example, the assertion would run before the actual deployment happens, and if the assertion fails, the deployment is stopped. This is of course a rather silly example, but you hopefully get the idea.

Why is this useful? When you are running larger deployments from files that contain a lot of resources, sometimes the deployment will fail halfway through the template because there was some unexpected error, and a lot of time is wasted waiting for the preceding resources to be deployed. In the future we can hopefully use assertions to make sure the output will be as expected at compile time.

# Unit Tests in Bicep

Unit tests are a type of software testing where individual units or components of a software are tested to verify that they work as expected. A unit can be a function, a method, a module, or an object. In the Bicep testing framework, we can run unit tests on resources as well. Unit tests are usually written and run by the developers during the coding phase of the software development process. Unit tests help to improve the quality, design, and documentation of the code, as well as to catch bugs and defects early.

Now I'll provide an example of a unit test in Bicep. Let's say we have this Bicep template for an AKS cluster:

```bicep
@description('Name of the AKS cluster')
param clusterName string

@description('Prefix for deployment')
param prefix string

resource aksCluster 'Microsoft.ContainerService/managedClusters@2023-03-02-preview' = {
  name: '${prefix}-${clusterName}'
  location: location

...
```

To use a simple example, let's say I want to make sure that my cluster name will be as expected. I would write a unit test as follows:

```bicep
test testAKS 'aks.bicep' = {
  params: {
    clusterName: 'coffee'
    prefix: 'prod'
  }
}

assert testAKS = testAKS.resources.aksCluster.name == 'prod-coffee'
```

When I run this test.bicep file, it will take the aks.bicep file and feed the 'coffee' and 'prod' parameters to it. My aks.bicep file will create the string from the parameters and output the name. Then, I use the "assert" keyword to verify that the string is composed according to my expectations.

Note: at the time of writing 2023-08-03, the "assert" keyword was still not available in Bicep 0.20.4, but there might be a problem with my local configuration which I've asked the team about.

# Conclusion

I believe it will be a great development to apply test driven software development to Infrastructure as Code by implementing a test framework into Bicep. When I'm able to use this approach, I would begin with writing all of the tests for the template that I have in mind, and then I can continue to write the Bicep template itself. By using the tests I can become much more productive because I can speed up my development by using tests without having to do what-if deployments which can be rather slow and involve a lot of waiting.

It is great to see how the Bicep language is maturing and how it continues to introduce new features that make the lives of developers easier. I was really stoked about the new .bicepparam files, but I'm even more excited by the new testing framework.

## Links:

202308030808 

https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/bicep-config

https://youtu.be/UuwhLi-Xe2U

