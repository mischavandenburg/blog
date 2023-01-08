---
title: How to deploy to a Linux VM in Azure with Azure Pipelines
date: 2023-01-08
tags:
- Azure
- Linux
- DevOps
- How-To
---
To reach a VM from Azure Pipelines, you need to set up an environment.

Create your Linux VM in Azure.

In Azure DevOps, click envirnoments, new, and select the Virtual Machine option.

![](/env1.png)

A command is generated for you. SSH into your VM and run the command.

![](/env2.png)

Now the VM should show up under environments in Azure DevOps.

Set up a repo with an azure-pipelines.yml with these contents to test. under `environment`, set the same name as you did in Azure DevOps for your environment.

```yaml
trigger: 
- main

pool: 
   vmImage: ubuntu-latest

jobs:
- deployment: VMDeploy
  displayName: Deploy to VM
  environment: 
   name: dev
   resourceType: VirtualMachine
  strategy:
     runOnce:
        deploy:   
          steps:
            - script: echo "Hello world"
```


You can see it when the deploy runs on the VM:

![](/env3.png)
