---
title: Homelab Installing loki
date: 2023-12-30
tags:
- Homelab

draft: true
---

* installed loki using grafana/loki helm chart
* solved problem with basic auth
* learned about components which send logs to loki
* learned bout promtail
* grafana agent is recommended for Grafana Stack, use Promtail for Kubernetes deployments
* installed promtail as DaemonSet becuase it was recommended
* succesfully streaming logs from all nodes to Loki
* able to query logs using Explore in Grafana
* now able to build custom dashboards with combined logs and metrics


## Links:

202312301112
