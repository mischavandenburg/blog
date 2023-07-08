---
Title: Wrote A Script To Delete All Resource Groups In An Azure Subscription
date: 2023-07-08
tags:
- Bash
- Azure
- Coding
- DevOps
---

In the Azure portal you can't select multiple resource groups for deletion.

I have a sponsored subscription to play around in which I sometimes wish to clean completely. 

I wrote this script to delete all resource groups using bash.

```bash
#!/bin/bash

# script to delete all resource groups in a subscription using Azure CLI

# get the current subscription name to confirm
subscription=$(az account show --query name -o tsv)

echo "Use this script with caution!"
echo "You are about to delete all resource groups in the subscription: $subscription"

# prompt for confirmation
read -p "Are you sure? (y/n) " will_delete

if [[ $will_delete == [Yy]* ]]; then
	echo "Deleting resource groups..."

	groups=$(az group list --query "[].name" -o tsv)

	# Loop through each group name and delete it
	for group in $groups; do
		az group delete --name "$group" --yes --no-wait
	done

	echo "All resource groups have been deleted."
	exit 0
fi

echo "Exiting without deleting any resource groups."
echo "Probably wise."
```


## Links:

202307081507
