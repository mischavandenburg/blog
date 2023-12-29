---
Title: Homelab Secret Management With GitOps and Azure Key Vault
date: 2023-12-29
tags:
- Kubernetes
- Homelab
- Azure
- AKS
- Security
- GitOps
- Flux
---

In this blog post, I want to share with you how I set up secrets management for my [home lab](https://github.com/mischavandenburg/homelab/). I use my home lab to explore new technologies, but I also try to keep it in line with the practices I would use when setting up environments for clients. I focus on Microsoft Azure and the ecosystem they provide for cloud native applications. Secrets management is an important aspect of any cloud-native application, as it allows you to securely store and access sensitive information such as passwords, tokens and certificates.

Since Flux is the integrated solution for GitOps in Azure Kubernetes Service I also adopted it for my home lab. Flux supports and recommends encrypting secrets in git using SOPS, which is a tool that uses asymmetric encryption to protect secrets. This means that you can store encrypted secrets in your git repository and only decrypt them when they are applied to the cluster. This sounds like a convenient and secure way to manage secrets, right?

Well, not quite. After doing some research, I decided to use a different approach: storing my secrets in Azure Key Vault and syncing them to my cluster with the Azure Key Vault Provider. This is the recommended best practice by Microsoft, as explained in this article:

>TLDR: Referencing secrets in an external key vault is the recommended approach. It is easier to orchestrate secret rotation and more scalable with multiple clusters and/or teams.

https://microsoft.github.io/code-with-engineering-playbook/continuous-delivery/gitops/secret-management/secret-management-gitops/

SOPS has some drawbacks that made me choose the Azure Key Vault route for my home lab. First of all, SOPS requires you to encrypt each secret value manually in the command line, which can be tedious and error-prone. Secondly, SOPS relies on a single encryption key that is stored in your local machine or in a cloud KMS. If you lose this key, you will lose access to all your encrypted secrets in your Git repo. Thirdly, SOPS does not provide a backup or recovery mechanism for your secrets, nor does it offer fine-grained access control or auditing capabilities.

Here are some of the reasons why I chose this approach over SOPS:

* Secrets are stored in an external source, which means that they are not exposed in git, even if encrypted.
* Azure Key Vault provides backup and recovery features, such as soft delete and restore, which can help in case of accidental deletion or corruption of secrets.
* Azure Key Vault allows fine-grained authentication and authorization to the vaults, using Entra ID identities and policies. This means that I can control who can access or modify my secrets, and audit their actions.
* I don't need to encrypt or decrypt values in the command line, which can be error-prone or leak information. I can use the Azure CLI or the Azure Portal to manage my secrets in the vault.
* If I lose my encryption key, all my encrypted secrets in git are useless with SOPS. With Azure Key Vault, I can still access my secrets using my Entra ID or recover them from backup.
* This approach mimics a solution that I would use for an enterprise production environment

# Azure Key Vault Provider

To sync my secrets from Azure Key Vault to my AKS cluster, I used the Azure Key Vault Provider for Secrets Store CSI Driver. This is an open source project that enables you to mount secrets from external sources (such as Azure Key Vault) as volumes in your pods using the Container Storage Interface (CSI) specification.

I followed [the provider documentation](https://azure.github.io/secrets-store-csi-driver-provider-azure/docs/) to install the provider in my local cluster using a Flux HelmRelease.

With the provider installed I can use the following configuration to sync a secret named grafana-admin-password from my Key Vault and make it available as a volume and environment variable in an example busybox pod:


```yaml
apiVersion: secrets-store.csi.x-k8s.io/v1
kind: SecretProviderClass
metadata:
  name: azure-kv-secrets
  namespace: monitoring
spec:
  provider: azure
  parameters:
    keyvaultName: "mischa-homelab-k8s" # the name of the KeyVault

    objects: |
      array:
        - |
          objectName: grafana-admin-password
          objectType: secret              # object types: secret, key, or cert

    tenantId: 6ddecc48-41b1-48de-bfde-2efd29fae9c7

  secretObjects: # [OPTIONAL] SecretObjects defines the desired state of synced Kubernetes secret objects
    - data:
        - key: password # data field to populate
          objectName: grafana-admin-password # name of the mounted content to sync; this could be the object name or the object alias
      secretName: grafana-custom-secret # name of the Kubernetes secret object
      type: Opaque # type of Kubernetes secret object (for example, Opaque, kubernetes.io/tls)
---
kind: Pod
apiVersion: v1
metadata:
  name: busybox-secrets-store-inline
  namespace: monitoring
spec:
  containers:
    - name: busybox
      image: registry.k8s.io/e2e-test-images/busybox:1.29-1
      command:
        - "/bin/sleep"
        - "10000"
      volumeMounts:
        - name: secrets-store01-inline
          mountPath: "/mnt/secrets-store"
          readOnly: true
      env:
        - name: SECRET_USERNAME
          valueFrom:
            secretKeyRef:
              name: grafana-custom-secret
              key: password
  volumes:
    - name: secrets-store01-inline
      csi:
        driver: secrets-store.csi.k8s.io
        readOnly: true
        volumeAttributes:
          secretProviderClass: azure-kv-secrets
        nodePublishSecretRef: # Only required when using service principal mode
          name: secrets-store-creds # Only required when using service principal mode. The name of the Kubernetes secret that contains the service principal credentials to access keyvault.
```

I hope you found this blog post useful and informative. If you have any questions or feedback, please feel free to leave a comment below or contact me on Twitter @mischa_vdburg

## Links:

202312290912

https://microsoft.github.io/code-with-engineering-playbook/continuous-delivery/gitops/secret-management/secret-management-gitops/

https://github.com/mischavandenburg/homelab/


- [Azure Key Vault documentation](https://docs.microsoft.com/en-us/azure/key-vault/)
- [Flux documentation](https://fluxcd.io/docs/)
- [Azure Key Vault Provider for Secrets Store CSI Driver documentation](https://learn.microsoft.com/en-us/azure/aks/csi-secrets-store-driver)

https://azure.github.io/secrets-store-csi-driver-provider-azure/docs/

