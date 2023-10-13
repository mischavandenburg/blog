---
title: Fixed an issue with akv2k8s overriding security context
date: 2023-10-13
tags:
- Kubernetes
---

This week I've been tackling an issue at work with akv2k8s.

We are running CloudnativePG, similar to EnterpriseDB. After upgrading akv2k8s to v2.5.0 our database pods were not coming up anymore due to an error with the initcontainer:

```
Error: container has runAsNonRoot and image has non-numeric user (nonroot), cannot verify user is non-root (pod: "vcs-pooler-rw-858bf7c954-vjzr4_vcs(e19bfa4c-26d8-4a6d-b4ad-bba8b52c01e6)", container: bootstrap-controller)
```

Pods have a security context where you specify how the containers in the pod should be run.

```
securityContext:
    runAsUser: 998
    runAsGroup: 996
    runAsNonRoot: true
    fsGroup: 996
```

Here we are telling the pod that we may not run as the root user and that we will run as user 998.

My error states that it cannot verify whether it is running as root or not because we are using a non-numeric user.

We were dumbfounded by this because the deployment and ReplicaSet neatly had their securitycontexts configured and the pods should inherit them. After a lot of searching I figured out that the securitycontext of the pods was actually empty, so somehow it must be overridden somewhere. 

The culprit was the akv2k8s envinjector. This tool injects the secrets into the environment of the pods. It took a lot of experimentation but we managed to narrow it down to the akv2k8s upgrade and eventually we found this GitHub issue:

https://github.com/SparebankenVest/azure-key-vault-to-kubernetes/issues/605

For some reason the envinjector is overriding the securitycontext of the pods. The issue was fixed by downgrading the helm chart back to 2.4.2

I did learn a lot in the process though!

## Links:

202310130610

https://www.enterprisedb.com/docs/postgres_for_kubernetes/latest/installation_upgrade/

https://github.com/SparebankenVest/azure-key-vault-to-kubernetes/issues/605

https://stackoverflow.com/questions/53949329/kubernetes-runasnonroot-failing-createcontainerconfigerror

https://github.com/cloudnative-pg/cloudnative-pg
