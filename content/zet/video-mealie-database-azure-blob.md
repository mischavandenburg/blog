---
title: "Video: configuring database for mealie"
date: 2024-01-01
tags:
- Video
- Homelab
- Kubernetes
- 
draft: true
---

* configured Azure SAS token for blob storage
* configured secrets in key vault for database and mealie application
* set up secret sync objects in k8s cluster using the Azure Key Vault CSI provider
* reflected on necessity to mount secrets as volumes in order to have them available as environment variables using the CSI driver
* looked at GitHub issue for the above

* had a lot of trouble with restoring backups from Object Store
* backups were failing with mysterious error messages
* caused  me to go through the EDB documentation extensively
* i learned A LOT about the EnterpriseDB operator for Kubernetes
* feel much more comfortable managing EDB postgres clusters on Kubernetes

* successfully achieved to restore backups from object store (Azure Blob Storage)
* successfully restored backups from in-cluster Backup resource

* feel very comfortable running databases in my homelab now
* feel very confident in understanding of backup and restoring with EDB operator
* significantly helped me in my day job as well

# glad I chose EDB operator over CloudnativePG

* EDB docs much more extensive and comfortable to read
* very good API reference
* EDB experience probably most relevant in job market

https://github.com/kubernetes-sigs/secrets-store-csi-driver/issues/298

## Links:

202401010901
