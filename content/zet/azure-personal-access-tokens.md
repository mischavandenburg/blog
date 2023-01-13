---
title: Azure DevOps Personal Access Tokens are always for authenticating into ADO
date: 2023-01-13
tags:
- Zettelkasten
- Azure
- DevOps
---
The PAT (Personal Access Token) often comes up during practice tests for the AZ-400.

One way to remember when to use a PAT is that these are only for authenticating **into** Azure DevOps, never to external services. 

For example, you might get a question on connecting your Azure DevOps project with a GitHub account from Azure DevOps, and PAT will show up as one of the alternative answers. By remembering that PATs are only for authenticating into ADO, you can elminate this alternative, and make your choice easier.

Personal Access Tokens are an alternative to passwords but should be treated in exactly the same way.

https://learn.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops&tabs=Windows


