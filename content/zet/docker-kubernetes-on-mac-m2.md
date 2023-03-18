---
title: Running Docker and Kubernetes on Mac M2
date: 2023-03-18
tags:
- Zettelkasten
- MacOs
- Kubernetes
- Productivity
- DevOps
---

The past few days I've been trying out a few options to run Docker containers and a Kubernetes clusters on my new MacBook Pro M2.

Unfortunately you can't just run `brew install docker` and expect it to work. Additionally, Docker desktop requires that you purchase a license if you use it for work purposes.

Minikube works fine as well, but the networking driver for qemu is not fully supported yet, and I haven't tried any of the other alternatives because I found something better.

Rancher Desktop provides everything that you need. It sets up a local VM where it will run a Kubernetes cluster using k3s. It will configure the containerd container engine for you which you can interact with using `nerdctl`.

To install:

```bash
brew install rancher

#after installing rancher, start it up and wait for it to boot the VM.

alias docker=nerdctl
docker run hello-world
```

And you're good to go. Rancher will add the rancher-desktop to your kube context.

To test your Kubernetes cluster:

```bash
k get pods
k get nodes

# test running a pod
k run nginx --image=nginx
k expose pod nginx --port=80 --type=NodePort

# inspect your services and look for 80:31066/TCP under PORT(S)
k get svc
curl localhost:31066
```

Or visit localhost:31066 in your browser. Replace 31066 with the port you found listed under your services.
