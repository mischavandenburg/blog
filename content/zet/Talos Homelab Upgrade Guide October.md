---
title: Talos Homelab Upgrade Guide October
date: 2024-11-14
tags:
  - Talos
  - Kubernetes
---

> [!IMPORTANT]
> Remember to create a custom image including iscsi-tools when using the Synology CSI driver

### Set up CLI environment and set databases to maintenance mode

```bash
export TALOS_CP="192.168.100.107"
export TALOS_W1="192.168.100.245"
export TALOS_W2="192.168.100.60"
export TALOS_W3="192.168.100.228"

# Set databases to maintenance mode
k cnp maintenance set --all-namespaces
```

> [!WARNING]
> Use the `--preserve` flag on single-node control plane clusters.
> Only needed for the control plane node.

### Upgrade path

1. from 1.7.5 to 1.7.7
2. from 1.7.7 to 1.8.2

### Upgrade 1

```bash
export TALOS_IMAGE='factory.talos.dev/installer/c9078f9419961640c712a8bf2bb9174933dfcf1da383fd8ea2b7dc21493f8bac:v1.7.7'

talosctl upgrade --preserve --nodes $TALOS_CP -e $TALOS_CP --image $TALOS_IMAGE
talosctl upgrade --wait --debug --nodes $TALOS_W1 -e $TALOS_CP --image $TALOS_IMAGE
talosctl upgrade --wait --debug --nodes $TALOS_W2 -e $TALOS_CP --image $TALOS_IMAGE
talosctl upgrade --wait --debug --nodes $TALOS_W3 -e $TALOS_CP --image $TALOS_IMAGE
```

### Upgrade 2

```bash
export TALOS_IMAGE='factory.talos.dev/installer/c9078f9419961640c712a8bf2bb9174933dfcf1da383fd8ea2b7dc21493f8bac:v1.8.2'

talosctl upgrade --preserve --nodes $TALOS_CP -e $TALOS_CP --image $TALOS_IMAGE
talosctl upgrade --nodes $TALOS_W1 -e $TALOS_CP --image $TALOS_IMAGE
talosctl upgrade --nodes $TALOS_W2 -e $TALOS_CP --image $TALOS_IMAGE
talosctl upgrade --nodes $TALOS_W3 -e $TALOS_CP --image $TALOS_IMAGE

```

## Kubernetes Upgrade

> [!WARNING]
> Make sure to upgrade the Talos CLI before proceeding.
> "It is advisable to use the same version of talosctl as the version of the boot media used." - Talos Docs

### Upgrade Path

1. From 1.30.3 to 1.30.5
1. From 1.30.5 to 1.31.1

> [!NOTE]
> To trigger a Kubernetes upgrade, issue a command specifying the version of Kubernetes to upgrade to, such as:
> talosctl --nodes <controlplane node> upgrade-k8s --to 1.30.0
> Note that the --nodes parameter specifies the control plane node to send the API call to, but all members of the cluster will be upgraded.
> To check what will be upgraded you can run talosctl upgrade-k8s with the --dry-run flag

```bash
k cnp maintenance set --all-namespaces
kubent
talosctl --nodes $TALOS_CP -e $TALOS_CP upgrade-k8s --to 1.31.1 --dry-run
talosctl --nodes $TALOS_CP -e $TALOS_CP upgrade-k8s --to 1.31.1
```

## Links

202411140811
