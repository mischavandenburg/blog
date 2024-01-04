---
title: "Video: Homelab E2 - Setting Up Monitoring + Studying k3s Networking  & Configuring Ingress"
date: 2024-01-04
tags:
- Kubernetes
- Homelab
- Video
- Networking
---

{{< youtube JjIB65CVXAo >}}

In this video I installed Prometheus and Grafana using helm and studied k3s networking.

My goal was to make Grafana approachable via ingress using a fake domain and after a bit of tinkering it worked.

* installed prometheus and grafana with kube-prometheus-stack helm chart
* reflected on why I use k3s
* gained understanding of k3s loadbalancing solution
* configured /etc/hosts file to resolve to fake domain
* configured k3s ingress to use fake local domain
* struggled with ingress but figured it out in the end
* successfully made grafana UI available on fake local domain grafana.homelab.nl

## Links:

202312261012

https://youtu.be/JjIB65CVXAo
