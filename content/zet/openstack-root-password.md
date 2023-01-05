---
title: How to Reset a VM Root Password using the Openstack CLI
date: 2023-01-05
tags:
- Linux
- How-To
---
1. Download the Openstack RC file from the Openstack portal. Click your username in the top right corner to find it.
2. Source the RC file to make the environment variables avaialable to your current session: `source ~/my_openstack.sh`
3. Find the instance ID of your VM from the portal.
4. Run `openstack server set --root-password be3xxxx5-8348-418b-xxxb-c4xxxx575cd`
5. You will be prompted for the new password which will be set on the virtual machine.
