---
title: Forcing a new cert from letsencrypt when too many have been issued
date: 2023-10-12
tags:
- Kubernetes
---

Learned a new trick to force a new cert from letsencrypt. Had this error message:

```
The certificate request has failed to complete and will be retried: Failed to wait for order resource "vcs-secret-j8gnj-3121219202" to become ready: order is in "errored" state: Failed to create Order: 429 urn:ietf:params:acme:error:rateLimited: Error creating new order :: too many certificates (5) already issued for this exact set of domains in the last 168 hours: sadfsadf.com, retry after 2023-10-12T18:54:27Z: see https://letsencrypt.org/docs/duplicate-certificate-limit/
```

I asked for too many certificates during a 168 hour period. However, there is a really easy fix. Just add a new subdomain and LetsEncrypt will treat it as an entirely new certificate!

For example, when using cert-manager:

```
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  creationTimestamp: '2023-10-12T11:09:36Z'
  generation: 2
  labels:
    argocd.argoproj.io/instance: vcs-dev
  name: vcs-secret
  namespace: vcs
  ownerReferences:
    - apiVersion: networking.k8s.io/v1
      blockOwnerDeletion: true
      controller: true
      kind: Ingress
      name: vcs-ingress
      uid: xxxxx
      resourceVersion: '226558404'
  uid: xxxx
spec:
  dnsNames:
    - vcs.mischavandenburg.com
    - test.mischavandenburg.com
  issuerRef:
    group: cert-manager.io
    kind: ClusterIssuer
    name: letsencrypt-prod
  secretName: vcs-secret
  usages:
    - digital signature
    - key encipherment
```

Just add another domain under spec.dnsNames and it will treat it as a new cert.

```
spec:
  dnsNames:
    - vcs.mischavandenburg.com
    - test.mischavandenburg.com
    - hello.mischavandenburg.com
```

## Links:

202310121310

[[Kubernetes]]
