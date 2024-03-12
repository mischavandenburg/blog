---
title: Comparing akv2k8s with Azure Key Vault Provider for Secret Store CSI Driver
date: 2024-03-11
tags:
- Article
- Azure
- Security
- Kubernetes
- AKS
---

In a recent analysis, I explored two notable solutions for synchronizing secrets from Azure Key Vaults to AKS (Azure Kubernetes Service) clusters: akv2k8s and the Azure Key Vault Provider for the Secret Store CSI Driver. Here, I present my findings and recommendations based on the functionality, maintenance requirements, and integration capabilities of these tools.

Akv2k8s, maintained by Sparebanken, is an open-source tool designed for the synchronization of secrets. Being dependent on an external tool for Kubernetes secrets synchronization is an undesirable situation and poses several challenges. Notably, the latest version of akv2k8s has been problematic, especially concerning the deployment of Postgres databases on our AKS clusters using the EDB operator. Akv2k8s alters the SecurityContext of pods in a way that causes them to fail.

Furthermore, upgrading akv2k8s to the latest version necessitates a transition to Workload Identity due to the deprecation of aad-pod-identity, a move that promises to be complex. Conversely, the Azure Key Vault Provider, directly offered by Microsoft, allows continued use of Managed Identities for authentication to Key Vaults, simplifying the integration process.

https://azure.github.io/secrets-store-csi-driver-provider-azure/docs/configurations/identity-access-modes/

# Azure Key Vault Provider for Secret Store CSI Driver

The Azure Key Vault Provider for Secret Store CSI Driver is the solution for syncing secrets offered by Microsoft Azure. It is based on the [kubernetes-sigs/secrets-store-csi-driver](https://github.com/kubernetes-sigs/secrets-store-csi-driver).

# Installation & Maintenance

The addon is installed with an Azure CLI command or from the portal for existing clusters. The addon can also be enabled from code when deploying a new cluster.

The addon is automatically updated when an AKS upgrade to a new minor version is performed. **Maintenance of the secret synching solution will not be required anymore and is handled by Azure Kubernetes Service.**

If an emergency update is necessary, outside of the normal upgrade schedule, it can be updated with the Azure CLI as follows:

`az aks addon update --addon virtual-node --name MyManagedCluster --resource-group MyResourceGroup --subnet-name VirtualNodeSubnet`

Addons can also be patched by upgrading to the latest AKS node image.

https://learn.microsoft.com/en-us/azure/aks/integrations
https://learn.microsoft.com/en-gb/azure/aks/node-image-upgrade

# Comparison

## Benefits Of Using The Official Solution

* The tool is built and maintained by Microsoft and an integral part of the AKS offering
* Authentication using a managed identity is supported out of the box
* Supports RBAC based authentication on Key Vaults
* Automatically updated when performing AKS upgrades
* One less component for us to maintain
* Can run alongside akv2k8s
* We can implement automatic secret rotation
* We are not dependent on an external project for our secrets management
* Does not require an initcontainer to inject secrets

## Cons

* Will take work to migrate all applications

# Migration Guide

Steps that describe how to migrate from the use of akv2k8s to the CSI drivers.

* enable the addon using the Azure CLI

`az aks enable-addons --addons azure-keyvault-secrets-provider --name aks-vo-dcp-dev --resource-group rg-vo-dcp-aks-dev-weu-001`

This does not restart any existing pods or nodes, thus it does not impact the running setup. akv2k8s and the AKV Provider can run simultaneously on the cluster and the workloads can be migrated one by one.

Enabling the addon creates a Managed Identity. This Managed Identity needs to get the Key Vault Administrator role on the desired Key Vault to be able to retrieve Secrets.

To import the secrets, a SecretProviderClass resource is created:

```yaml
apiVersion: secrets-store.csi.x-k8s.io/v1
kind: SecretProviderClass
metadata:
  name: azure-sync
spec:
  provider: azure
  parameters:
    usePodIdentity: "false"
    useVMManagedIdentity: "true" # Set to true for using managed identity
    userAssignedIdentityID: ceb19a0a-941a-4f33-839d-aeace8ffe205 # Set the clientID of the user-assigned managed identity to use
    KeyvaultName: mischakv01 # Set to the name of your Key Vault
    cloudName: "" # [OPTIONAL for Azure] if not provided, the Azure environment defaults to AzurePublicCloud
    objects: |
      array:
        - |
          objectName: ExampleSecret
          objectType: secret              # object types: secret, key, or cert
          objectVersion: ""               # [OPTIONAL] object versions, default to latest if empty
        - |
          objectName: env-secret
          objectType: secret              # object types: secret, key, or cert
    tenantId: d62ada1b-ca42-4fe2-b9e7-ceb843af0ad # The tenant ID of the key vault
  secretObjects: # [OPTIONAL] SecretObjects defines the desired state of synced Kubernetes secret objects
    - data:
        - key: username # data field to populate
          objectName: env-secret # name of the mounted content to sync; this could be the object name or the object alias
      secretName: foosecret # name of the Kubernetes secret object
      type: Opaque # type of Kubernetes secret object (for example, Opaque, kubernetes.io/tls)

```

The secret is then mounted in to the container during deployment. 

See this pod example:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: busybox-secrets-store-inline
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
              name: foosecret
              key: username
  volumes:
    - name: secrets-store01-inline
      csi:
        driver: secrets-store.csi.k8s.io
        readOnly: true
        volumeAttributes:
          secretProviderClass: "azure-sync"
```

In this example the secret is accessible as a text file at /mnt/secrets-store inside the container. It is also made available as an environment variable SECRET_USERNAME.

The applications on our platform are already using environment variables when using akv2k8s.

The application helm charts will need to be updated with the new SecredProviderClass, but the applications themselves don't need any changes. They will need to be redeployed with the updated configuration though.

Another note is that the secret must always be mounted even it is only used as an environment variable.

# Conclusions

Migrating to the Azure Key Vault Provider for Secret Store CSI Driver presents no disadvantages. There will be one less component to maintain, and it will always be compatible with the AKS offering.

It will require some work to convert the current workloads to use the CSI driver, but this is a one-time action which will not affect the applications while they are running. On the long term, this will pay itself back, because there is one less component to maintain.

## Links:

202403111503
