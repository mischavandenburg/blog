---
title: "Lab Project: GitOps with ArgoCD, Azure Pipelines and Minikube"
date: 2022-12-24T13:49:55+01:00
tags:
- DevOps
---
This weekend I had a lot of fun with a project. I wanted to learn more about GitOps and try out ArgoCD.

My goal was to be able to deploy an application from a GitHub repo to my local Kubernetes cluster running in minikube. There are many options I could have used, such as running Jenkins in my cluster. But I wanted to use Azure pipelines for practice, which complicates the deployment to my local cluster, because the cluster is not running on Azure. I also wanted to try out ArgoCD and learn more about GitOps.

The application is a simple web app that I wrote which displays a quote in the browser:

![](/app.png)

# GitOps and Structure

GitOps is used to automate the process of provisioning infrastructure. Infrastructure as code is used to generate the same environment every time the environment is deployed.

For my project I have two separate GitHub repos. [The first repo](https://github.com/mischavandenburg/static-quote-app) contains the code for a simple web app I created and the Dockerfile to generate the image. I call this my application repo. The other repo is [my GitOps repo](https://github.com/mischavandenburg/static-quote-app-gitops) which contains the manifest files to deploy the application in Kubernetes. I decided to leverage Helm to create my manifest files. This way I can create templates and define my desired values in a values.yaml file in the repo. 

Ultimately my goal was to use an Azure pipeline to build an image from my application repo and push it to Docker hub. This new image is given a new tag which needs to be stored. The first pipeline should trigger a new pipeline that makes a pull request to the GitOps repo to update the tag in my Helm chart. 

ArgoCD will then scan the GitOps repo and realize that the tag has been updated, and deploy the new tag to my cluster.

# Minikube

I used [minikube](https://minikube.sigs.k8s.io/docs/) to deploy my local Kubernetes cluster. Another option is [kind](https://kind.sigs.k8s.io/) (Kubernetes In Docker) but I wanted to use a VM approach this time.

# ArgoCD

[ArgoCD](https://argo-cd.readthedocs.io/) is a declarative GitOps continuous delivery tool for Kubernetes. This is the solution I used to continuously scan my GitOps repo. When ArgoCD detects a change in the desired state, it will compare it with the state in my running cluster and make changes accordingly. I found a really good [tutorial to run ArgoCD in minikube](https://redhat-scholars.github.io/argocd-tutorial/argocd-tutorial/02-getting_started.html).

![ArgoCD dashboard](/argocd-dashboard.png)

# Azure Pipelines

With my cluster running on my local machine and my repos set up, I needed to use Azure Pipelines to bring it all together. Building the image and pushing it to Docker Hub wasn't a big deal. But I had two big challenges in my desired setup: I needed to pass the new tag number to a new pipeline, and I needed to use Azure Pipelines to create a new PR to my GitOps repo.

### Passing a value from one pipeline to another

Interestingly, this wasn't as easy as it sounds, and from my internet searching it seemed that many people struggled with this. I decided to use the Variable Groups in Azure DevOps. However, after I finished writing my pipeline, I discovered I had no problems with reading the value from the Variable Groups, but it was impossible to update it using existing pipeline tasks. So I had to a bit of hacking to make it work. In the end I had to use the Azure CLI from within the pipeline to update my variable:

```yaml

- stage: update_tag
  jobs:
  - job: update_tag_variable 
    displayName: Update Tag Variable
    steps:
      - bash: |
          az pipelines variable-group variable \
          update --group-id 202 \
          --org $(System.CollectionUri) \
          --project $(System.TeamProject) \
          --name tag --value $(Build.BuildId)
        env:
          AZURE_DEVOPS_EXT_PAT: $(System.AccessToken)

```

This didn't feel like a very elegant solution, but it was the only solution I could come up with.

I also struggled a lot with permissions. I needed to find the correct service principal to assign the administrator rights to. [This post](https://stackoverflow.com/questions/52986076/having-no-permission-for-updating-variable-group-via-azure-devops-rest-api-from) really helped to solve my problem.


### Submitting a PR to a GitHub repo

When I started writing my pipeline I thought it would be very straightforward to just submit a PR to a repo, but I quickly discovered that this is not natively supported in Azure pipelines yet. In fact, I could not find a way to submit a PR at all. I had to settle for a solution that checks out the GitOps repo and creates a new branch. This new branch updates the tag in the values.yaml with the new tag that was passed from the previous pipeline. 

```yaml

variables:
- group: mischa-quote
- name: passed_tag
  value: $[variables.tag]
- name: branch_name
  value: "pipeline-$(passed_tag)"

pool:
  vmImage: ubuntu-latest

steps:
  - checkout: self
    persistCredentials: true
    clean: true
  - script: |
      git config --global user.email "mischa@pipeline.com"
      git config --global user.name "Mischa Pipeline"
      git switch -c "$(branch_name)"
      sed -i "s/tag:.*/tag: $(passed_tag)/" values.yaml 
      git add .
      git commit -m "Update tag to $(passed_tag)"
      git push --set-upstream origin "$(branch_name)"

```

This also felt a bit hacky to do with explicit shell commands, but it was the only way I could find to achieve my goal. I used sed to update the tag.

# Result

The resulting deployment pipeline is as follows. 

1. I make a commit to my application repo, which triggers a build pipeline in Azure DevOps:

![](/trigger-pipeline1.png)

2. This resulted in an image pushed to my Docker Hub:

![](/docker-hub.png)


3. The pipeline created a new branch in my GitOps repo. Unfortunately, I have to make the PR myself, but as you can see, the pipeline successfully updates the values.yaml with the new tag which we also saw in Docker Hub:

![](/new-branch.png)

![](/update-tag.png)

4. When I merged the pull request, ArgoCD detected the change and deployed a new pod with the new tag.

![](/argocd-sync.png)


5. Running a `kubectl describe` on the pod also verifies that we have the correct image:

![](/kubectl-tag.png)

# Conclusion

This was a fun challenge, but I learned a lot from solving the problems I encountered and my entire Saturday flew by in an uninterrupted flow state. I had some good practice in setting up Azure pipelines, learned about Helm, and did my first implementation of GitOps. Not bad for a day's work!



# Links
[Application GitHub repo](https://github.com/mischavandenburg/static-quote-app) 

[GitOps repo](https://github.com/mischavandenburg/static-quote-app-gitops)

[minikube](https://minikube.sigs.k8s.io/docs/)

[kind](https://kind.sigs.k8s.io/)

[ArgoCD](https://argo-cd.readthedocs.io/)

[tutorial to run ArgoCD in minikube](https://redhat-scholars.github.io/argocd-tutorial/argocd-tutorial/02-getting_started.html)
