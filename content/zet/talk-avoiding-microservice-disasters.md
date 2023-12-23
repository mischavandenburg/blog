---
title: "Talk: Avoiding Microservice Megadisasters by Jimmy Bogard"
date: 2023-12-23
tags:
- Azure
- Kubernetes
- cloud-native
- Architecture
---

Watched this very insightful talk on microservice architecture.

Some things I learned:

* microservices should be **autonomous**: they should have minimal dependencies on other microservices
* if they have dependencies they should only be 1 layer deep
  * a microservice should not be calling another microservice which calls another microservice
* dependencies can be reversed by pushing data towards the service
  * for example: a pricing database can be dumped and pushed to a catalog service once a day
  * data duplication is not a sin

Another powerful point he made is that the architecture is a reflection from the organization's structure. He explained that the company was organized in teams and that managers of teams were promoted based on the amount of people they managed. This meant that managers were making up services which needed people to build them, which led to a proliferation of services and dependencies.

In order to have a correct software strucutre, the organization must be organized correctly first.

{{< youtube gfh-VCTwMw8 >}}

## Links:

202312231912

https://youtu.be/gfh-VCTwMw8?si=QqHcbG_5ezx5qJX2
