---
title: Homelab Dashboards As Code
date: 2023-12-30
tags:
- 

draft: true
---

* extracted all dashboards from kube-prometheus-stack config maps and converted to json
* set up grafana chart to use persistent volume for default dashboards
* wrote script to import default dashboards to persistent volume
* after copying these dashboards are persistent so only needs to be done on fresh cluster deployment


## Links:

202312300912
