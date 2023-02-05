---
title: Setting up a Kubernetes cluster on an Ubuntu 20.04 VM with containerd and flannel
date: 2023-02-05
tags:
- Article
- Kubernetes
- Linux
---

You can get a free 24GB ram VM from Oracle. What better place for your own Kubernetes lab that is always available? See [this article](/zet/free-oracle-vm.md) to create your VM.

Here are the steps I took to install a single node kubernetes cluster on the Ubuntu VM.

## Installation

```bash
sudo apt-get update
sudo apt install apt-transport-https curl
```

Install containerd 

```bash
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install containerd.io
```

Remove the default containerd configuration, because it creates errors when running kubeadm init.

```bash
sudo rm -f /etc/containerd/config.toml
sudo systemctl status containerd.service
```

Install Kubernetes

```bash
sudo curl -fsSLo /etc/apt/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt install kubeadm kubelet kubectl kubernetes-cni
```

Avoid the error "/proc/sys/net/bridge/bridge-nf-call-iptables does not exist" on kubeinit (reference https://github.com/kubernetes/kubeadm/issues/1062). 

```bash
sudo modprobe br_netfilter
sudo echo 1 > /proc/sys/net/ipv4/ip_forward
```

## Start the cluster

Initialize the Kubernetes cluster for use with Flannel

```bash
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```

Copy to config as kubadm command says

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

Usually you wouldn't run pods on your control-plane node. However, since we are running a lab environment on a single VM, it's ok. To be able to schedule pods on the control-plane node, we need to remove the NoSchedule taint:

```bash
kubectl taint node instance-20230205-0909 node-role.kubernetes.io/control-plane:NoSchedule-
```

## Add a Container Networking Interface

Install Flannel to the cluster (reference https://github.com/flannel-io/flannel)

```bash
kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml
```
## Configure the server firewall

We use Uncomplicated Firewall. Run these commands:

```bash
sudo ufw allow 22
sudo ufw allow 6443/tcp
sudo ufw allow 2379:2380/tcp
sudo ufw allow 10250/tcp
sudo ufw allow 10259/tcp
sudo ufw allow 10257/tcp
sudo ufw enable
sudo ufw status
```

## Set up bashrc

Next, edit your bashrc with `vim ~/.bashrc` and add these lines:

```bash
source <(kubectl completion bash)
alias k=kubectl
complete -o default -F __start_kubectl k
```
Then run `source ~/.bashrc`

This configures autocompletion for kubectl, and sets up "k" as an alias for kubectl.

## Let's run a pod!

To see all pods running on your cluster:

`k get pods -A`

Now let's run a simple nginx pod and expose it:

```bash
k run nginx --image=nginx
k expose pod nginx --port=80 --type=NodePort
```

To find out which port it's running on, run `k get service`. In the PORT(S) column, there will be an nginx service exposing port 80 to a random port on the node in the range of 30000-32767.

In my case, it says "80:31878/TCP"

To see if we can reach the container, run:

`curl localhost:31878`

If everything went well, you will get back the HTML of the default index page served by NGINX:

```bash
ubuntu@instance-20230205-0909:~$ curl localhost:31878
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

To reach the pod from the browser, open your port in the security group configured for the subnet of your VM.

Good luck with your new lab environment!

## Links

https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/

https://kubernetes.io/docs/concepts/services-networking/service/

https://github.com/flannel-io/flannel
