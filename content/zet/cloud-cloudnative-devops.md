---
title: The Difference Between DevOps, Cloud and Cloud Native
date: 2023-03-26
tags:

- Kubernetes
- DevOps
- Cloud-Native
---

I found [an excellent video](https://youtu.be/gyjRriOyw-k) by Rob Muhlenstein explaining the differences between Cloud, Cloud Native and DevOps. Here are the notes I wrote.

# Cloud

These are primarily *cloud services*. The external cloud. 

"Something as a Service".

* Amazon
* Azure
* GCP

# Cloud Native

This is Cloud Native: [The CNCF Landscape](https://landscape.cncf.io/)

Cloud Native is the technology that makes the cloud possible, and all the technology dependent on those services.

## Computing

* Edge Computing
* High Performance Computing

* Encapsulates all of the technologies that are involved with containerization of work, jobs and nodes
* Deployment of compute resources as nodes
* This is why Google's Borg was called Borg
* Computers are drones of a larger collective
* Every node puts all the resources into the collective. 

The collective is all the nodes combined, and Kubernetes is the Borg that orchestrates everything. It sees available resources and allocates the work that needs to be done.

Borg is the internal system developed at Google to run their infrastructure. You can read about it in the [Site Reliability Engineering](https://sre.google/books/) books and I highly recommend them.

> Kubernetes is /proc for the cloud

> Rob Muhlenstein

## Most Important Technologies

* Docker, Dockerfiles
* Kubernetes
* Helm
* Harbor
* Different registries, harbor, quay

* It is a lot of Python and POSIX shell
* Go for infrastructure application development

* Kubernetes and Helm have won the game

## Containers: Size Matters

* Size matters (again) in the cloud
* The smaller your container the better, because it takes less resources and less costs

# DevOps

DevOps is not the same as Cloud Native. It is one piece of it, a specific set of practices and actions that can be done within Cloud Native.

* How you write software and release it
* CI/CD
* Focused on getting the software out
* GitLab has become the one stop shop
* Purpose is to write software and get it published fast
* GitOps


# Summary

In summary, "cloud" stands for the services offered by cloud providers such as AWS, Azure and GCP. Cloud Native stands for all of the technology that makes these cloud services possible. DevOps is part of Cloud Native, but definitely not the same thing. DevOps is concerned with how software is written and released.

# Links:

202303262003

https://youtu.be/gyjRriOyw-k

https://landscape.cncf.io/

https://sre.google/books/

[[rwxrob]]

[[devops]]

[[kubernetes]]

[[cloud-native]]

[[cncf]]
