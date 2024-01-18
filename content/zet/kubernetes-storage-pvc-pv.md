---
title: "PersistentVolumeClaims Lifecycle in Kubernetes"
date: 2024-01-18
tags:
- Kubernetes
- Storage
- Azure
- AKS
---

I always thought that Persistent Volume Claims where deleted when you delete the pod which they are associated with. I was wrong. The lifecycle of PVCs is independent of Pods, and their behavior is largely governed by the Reclaim Policy set on the PVs. Here's what you need to know:

- **PVCs**: These are requests for storage, akin to how Pods request resources like CPU and memory. They exist independently and can be bound to Pods when needed.
- **PVs**: Provisioned by administrators or dynamically through Storage Classes, PVs provide the actual storage resources. Their lifecycle is not tied to any specific Pod.

# Understanding Reclaim Policies

The Reclaim Policy on a PV dictates its fate after a PVC is released. There are three policies to be aware of:

1. **Delete**: The PV and its underlying storage are deleted when the PVC is deleted. Importantly, if the PVC is not deleted, the PV remains.
2. **Retain**: When a PVC is deleted, the PV is not. Instead, it transitions to a `Released` state, but the data remains intact until manually deleted or repurposed.
3. **Recycle (Deprecated)**: The volume is scrubbed clean and made available for a new claim. Note: this policy is no longer recommended.

# Practical Implications

- PVCs won't cause PV deletion unless they themselves are deleted. This is crucial to understand, especially with a `Delete` Reclaim Policy, as it leads to the deletion of both the PV and the underlying storage, resulting in data loss.
- Carefully manage PVCs, especially in production environments. Ensure you have backups and fully understand the implications of deleting PVCs, especially when using a `Delete` Reclaim Policy.

## Links:

202401180901

[[kubernetes]]

[[storage]]
