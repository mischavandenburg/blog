---
title: Lab VM project - Install ArgoCD to your Kubernetes cluster
date: 2023-02-05
tags:
- Article
- Kubernetes
- ArgoCD
- GitOps
---

This guide uses the official getting started guide with a few modifications. This installation is only for lab purposes. Running ArgoCD in a production environment requires more configuration.

## Install argocd and argocd cli

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

My VM is running on arm architecture, so I need these commands to install the argocd cli on ubuntu.

```bash
curl -sSL -o argocd-linux-arm64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-arm64
sudo install -m 555 argocd-linux-arm64 /usr/local/bin/argocd
rm argocd-linux-arm64
```

Change the service type to LoadBalancer

```bash
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```

Retrieve your passsword

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```

Find out which port argocd-server is running on

```bash
k get svc -A
```

Look for the argocd-server and see where port 80 is mapped to. In my case, it is 80:31372.

Open this port in your network security group for your VM, and you should be able to log in on ArgoCD in the browser by entering the VM ip followed by the port:

`http://143.44.179.11:31372`

## Links

https://argo-cd.readthedocs.io/en/stable/getting_started/
