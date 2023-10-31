---
title: Getting A Secret from an Azure Key Vault From The Command Line
date: 2023-10-31
tags:
- Azure
- command-line
---

I use this bash code to get secrets from Azure key vaults using the Azure CLI.

Make sure to set your subscription with `az account set -s 12321424`

```bash
kvname=kv-vo-rea-prd-weu-001
secretname=worker-ad-client-secret

az keyvault secret show --vault-name $kvname --name $secretname  --query value -o tsv
```



## Links:

202310312010

[[azure]]
