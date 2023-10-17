---
title: Some notes on upgrading AKS clusters running EnterpriseDB
date: 2023-10-17
tags:
- Kubernetes
---

# Upgrading clusters with databases

When upgrading this type of cluster it is better to upgrade the control plane first and then upgrade the node pools one by one. 

The EDB operator prevents you to drain nodes with databases on them with PodDisruptionBudgets.

In order to be able to run the AKS upgrade you will need to set the following:

`k cnp maintenance set --all-namespaces --reusePVC`

* cnp is a kubectl plugin which you need to install to manage the EDB operator
* `--reusePVC` can only be used if all the nodes are in the same availability zone
* use this command to list all the zones: `kubectl describe nodes | grep -e "Name:" -e "topology.kubernetes.io/zone"`

When the upgrade is complete, run `k cnp maintenance set --all-namespaces`

## Getting the status

You can get all information about the database cluster:

`k cnp status pyramid-cluster-database -n mycommodity`

Most deployments have the same name pyramid-cluster-database.

## Problems

Sometimes a database replica will not come up properly and you can get the following error message:

`(FATAL: the database system is starting up (SQLSTATE 57P03))`

To fix this, first disable reusing PVC by running:

`k cnp maintenance set -n problemdatabase`

"problemdatabase" is the namespace where the database cluster is deployed.

Then delete the PVC of the problematic pod. This should trigger a new replica to be created from scratch. Depending on the size of the DB it can take a lot of time. 


## Links:

202310171410

[[kubernetes]]
