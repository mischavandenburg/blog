---
title: Talos Linux Upgrade Guide July
date: 2024-07-31
tags:
- Kubernetes
- Homelab
- Talos
---

I upgraded my homelab cluster using Talos Linux today.

I made a mistake and forgot to use the custom-built image using iscsi-tools. 

Learning the hard way.

Here are my notes:

## Talos upgrade


> [!IMPORTANT]
> Remember to create a custom image including iscsi-tools

### Set up CLI environment and set databases to maintenance mode

```bash
export TALOS_CP="192.168.100.107"
export TALOS_W1="192.168.100.245"
export TALOS_W2="192.168.100.60"

k cnp maintenance set --all-namespaces
```


> [!WARNING]
> Use the `--preserve` flag on single-node control plane clusters.
> Only needed for the control plane node.


### Upgrade path:

1. from 1.6.4 to 1.6.8
2. from 1.6.8 to 1.7.5


### Upgrade 1

> [!NOTE]
> I made a mistake and did not update to the custom-built image including the iscsi-tools
> This caused my cluster storage to break.

```bash
talosctl upgrade --preserve --nodes $TALOS_CP -e $TALOS_CP --image ghcr.io/siderolabs/installer:v1.6.8
talosctl upgrade --nodes $TALOS_W1 -e $TALOS_CP --image ghcr.io/siderolabs/installer:v1.6.8
talosctl upgrade --nodes $TALOS_W2 -e $TALOS_CP --image ghcr.io/siderolabs/installer:v1.6.8
```

### Upgrade 2

I went to https://factory.talos.dev/ and created a new image including iscsi-tools.

Everything worked fine after that.

```bash
talosctl upgrade --preserve --nodes $TALOS_CP -e $TALOS_CP --image factory.talos.dev/installer/c9078f9419961640c712a8bf2bb9174933dfcf1da383fd8ea2b7dc21493f8bac:v1.7.5
talosctl upgrade --nodes $TALOS_W1 -e $TALOS_CP --image factory.talos.dev/installer/c9078f9419961640c712a8bf2bb9174933dfcf1da383fd8ea2b7dc21493f8bac:v1.7.5
talosctl upgrade --nodes $TALOS_W2 -e $TALOS_CP --image factory.talos.dev/installer/c9078f9419961640c712a8bf2bb9174933dfcf1da383fd8ea2b7dc21493f8bac:v1.7.5

```

## Kubernetes Upgrade

> [!WARNING]
> Make sure to upgrade the Talos CLI before proceeding.
> "It is advisable to use the same version of talosctl as the version of the boot media used." - Talos Docs



### Upgrade Path

1. From 1.29.0 to 1.29.3
2. From 1.29.3 to 1.30.3

> [!NOTE]

> [!NOTE]
> To trigger a Kubernetes upgrade, issue a command specifying the version of Kubernetes to upgrade to, such as:
> talosctl --nodes <controlplane node> upgrade-k8s --to 1.30.0
> Note that the --nodes parameter specifies the control plane node to send the API call to, but all members of the cluster will be upgraded.
> To check what will be upgraded you can run talosctl upgrade-k8s with the --dry-run flag


### Round 1

```bash
k cnp maintenance set --all-namespaces
kubent
talosctl --nodes $TALOS_CP -e $TALOS_CP upgrade-k8s --to 1.29.3 --dry-run
talosctl --nodes $TALOS_CP -e $TALOS_CP upgrade-k8s --to 1.29.3
```


### Round 2

```bash
k cnp maintenance set --all-namespaces
kubent
talosctl --nodes $TALOS_CP -e $TALOS_CP upgrade-k8s --to 1.30.3 --dry-run
talosctl --nodes $TALOS_CP -e $TALOS_CP upgrade-k8s --to 1.30.3
```








## Links:

202407311407
