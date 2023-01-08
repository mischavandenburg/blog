---
title: How To Build and Deploy A Docker Container to An Azure VM Using Azure Pipelines 
date: 2023-01-08
tags:
- Azure
- DevOps
---
I wanted to build an application from a Dockerfile and deploy it to a VM. I used a default Svelte setup as an example app.

Naturally, Azure prefers that you deploy containers to services such as Azure Container Instances or App Services, so they don't provide modules for the pipelines to deploy to docker servers as far as I could tell. 

I searched for a long time but I could not find a solution. In the end I just ran shell commands from the pipeline to run the container on on the server.

```yaml
steps:
  - script: |
      sudo docker stop svelte-test
      sudo docker rm svelte-test
      sudo docker run --name svelte-test -p 8080:80 -d mischavandenburg/svelte-test:$(Build.BuildId)
```

You can find the full pipeline code, the app and Dockerfile in my lab repo:

https://github.com/mischavandenburg/lab/tree/main/azure-pipelines/docker-to-azure-vm
