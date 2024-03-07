---
title: Starting My Homelab
date: 2023-04-12
tags:
- Kubernetes
- Homelab
- Linux
- Hardware
- Study
- Article
---

This week I started a project which I've been putting off for too long. I finally started my homelab. Over the past year I've been collecting hardware here and there, and I've had the intention to start up a proper Kubernetes cluster at home. I got inspired by Rob Muhlenstein's [Homelab Init](https://www.youtube.com/playlist?list=PLrK9UeDMcQLpjUGg5z9Z6Un-axVx06-2J) playlist on YouTube which I'm working on.


There are a few reasons why I haven't started up until now:

* Focused on switching jobs and certifications
* Not knowing what to run on the cluster
* High electricity costs

Now that I found a new job with a great employer, I've changed my focus towards doing more hands-on learning in my free time by learning Go and diving deeper into Cloud Native technology. Energy prices have come down in the meantime as well.

I've reached a stage in my Go learning journey where I'm actually able to start making small deployable applications, and I want an environment where I can do that without high costs or without fearing to break something. I want to learn more about databases on Kubernetes, and I want to start writing small microservices and API's that are able to query these databases. 

My lab is going to be my playground, where I can deploy whatever I want and learn the technologies that interest me at that particular moment. 

# Hardware: Parts

For some reason the Raspberry Pi has become synonymous with homelabs. I get that it's fun to run a cluster on something that is not much larger than a 1kg pack of sugar. But I never really caught on to that whole scene yet. Maybe it's because I'm a bit late to the party and the Pi's have been scarce and very expensive lately?

In any case, I've been thankfully accepting old computers that friends were going to get rid of, and I've been keeping some of my own old hardware as well. I have enough motherboards and other parts to assemble around 3 nodes, which will probably have around 8GB RAM each, but possibilities to attach storage.

This is also what has been keeping me back for a while I think. There is quite a bit of work that I need to do to get these machines going, and probably I'll have to purchase a couple of other parts. However, I also have some functional hardware. 

## Gaming Desktop

I have an old gaming desktop with 16GB RAM, an Intel 6700K Skylake, 1070 video card and a couple of TB of storage. 

This has been my Arch Linux desktop for the past year, but now that I switched to my new MacBook, I'm not using it as much. I want to keep it as it is right now, but I could run a few Virtual Machines on there, and maybe consider turning it into a ProxMox server.

## Old Laptops

I have two old laptops. One Asus with 4GB of RAM and a Thinkpad T430 with 8GB RAM. The Thinkpad is actually surprisingly powerful. As a weekend project I installed Arch on it and I fitted it with a refurbished keyboard, and it is a very pleasant machine to work with. However, now that I have a very powerful laptop that I use as a desktop and portable device, it has become redundant.

# Old Laptops as Raspberri Pi's

Having these two old laptops lying around, it occurred to me that these machines were basically Raspberri Pi's with a large form factor and a higher power usage. Why would I need to spend hundreds of euros on these smaller computers if I could just use these laptops as a starting point for my lab?

Using laptops has the following advantages:

* No additional costs
* No building needed
* Easy to install Linux on them
* Built-in screen and keyboard for quick access when SSH does not work out
* Built-in batteries to handle short power disruptions (rare but possible)
* Can get going very quickly

# Choices and Goals

## Kubernetes

The first goal is to get a Kubernetes cluster running. I will do bare metal kubeadm installs, and later I want to learn more about Talos. Fortunately I feel very comfortable installing Kubernetes with kubeadm. I did plenty of practice for my CKA, and I recently installed it on free Oracle VM's. I've experimented a bit with K9S earlier, but I want to learn how to maintain on-prem Kubernetes. 

My goals is to learn to maintain production-grade clusters properly.

## Linux

Naturally I'll be using Linux as my base OS. After some consideration I chose to use Ubuntu Server 22. Some notes on that choice:

* I already have years of Ubuntu Server experience
* Good to keep building on what I have
* Working with managed Kubernetes on my day job requires me to keep Linux admin skills fresh
* Still the [most popular Linux distro](https://www.enterpriseappstoday.com/stats/linux-statistics.html)
* Google uses Ubuntu Server
* Well documented and plenty of questions on StackOverflow

## Infrastructure as Code & GitOps

Initially I'll configure the servers by hand, but I want to have the server configuration as code as Ansible playbooks eventually. However, I'll be using ArgoCD for all of my deployments on Kubernetes itself, so the server configuration is only a very small part of the setup. Just get Kubernetes running and do the rest with ArgoCD.

Perhaps I will expand with larger servers that run multiple VM's. Then it will be very relevant to start provisioning these with Ansible. 

## Networking

I want to learn more about networking and use static IP addresses for my servers. I need to figure out how my home network works exactly. Surprisingly, I've never taken the effort to actually know how the devices on my network get their IP addresses and how they communicate, even though I've learned plenty about it for my day job and do networking in an enterprise environment daily.

For Kubernetes I'll use Flannel to start out with, but I want to learn more about Cilium, Istio and other service mesh implementations. 

Another goal is to host my own DNS server for internal name resolution, probably CoreDNS.

## Deployment

I want to host my own container registry (Harbor) and use Tekton pipelines to for CI/CD, and I'm playing with the thought to host my own GitLab instance as well.

# Let's Go!

Another realization was that I don't need to have everything figured out before I begin. The beauty of cluster computing is that you can add to it as you go. I can start with a small cluster of two nodes and build it out as my needs grow. I don't expect to need more than a few GB of RAM in the foreseeable future, so these two laptops will be plenty to get going. 

![](/cluster-laptops.png)

## Links:

2023041213

[[homelab]]

[[homelab-network]]

[[linux]]

[[homelab-ubuntu-server]]

[[Kubernetes]]



