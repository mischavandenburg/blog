---
Title: "Video - How To Install Prometheus & Grafana In Your Homelab"
date: 2023-12-25
tags:
- Kubernetes
- Homelab
- Grafana
---

{{< youtube 3AINqaBwOYs >}}

In this video I'll be installing Prometheus and Grafana in a Kubernetes cluster running in Rancher Desktop on my MacBook.

There are many options available out there but this is the easiest one I found to get up and running quickly.

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

`helm install prometheus-stack prometheus-community/kube-prometheus-stack --namespace=prometheus-stack --create-namespace`

`helm show values prometheus-community/kube-prometheus-stack > prometheus-default-values.yaml`

## Opening the Grafana UI

`k port-forward svc/prometheus-stack-grafana 3000:80`

Then you can open it by entering `localhost:3000` in your browser.

The default credentials are admin:prom-operator

## Links:

202312250812

https://youtu.be/3AINqaBwOYs?si=maN1rfbg4pzBc3xI

https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack#configuration
