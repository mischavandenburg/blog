---
title: "Video: Homelab Episode 2 - Install Monitoring and Learning k3s Networking"
date: 2023-12-26
tags:
- Kubernetes
- Homelab
- Video
- Networking

draft: true
---

In this video I installed prometheus and grafana using helm and studied k3s networking.

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
