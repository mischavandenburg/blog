---
title: How to install the Openstack CLI on Linux
date: 2023-01-05
tags:
- Linux
- How-To
---

Make sure to have pip installed.

Run `pip install python-openstackclient`

Pip will install a binary called "openstack" in ~/.local/bin

If the openstack command is not available in your session, you might need to add it to your PATH:

`export PATH="$HOME/.local/bin:$PATH"`

Add this to your ~/.zshrc or ~/.bashrc to make sure this happens for each shell session.

Don't forget to source your updated ~/.zshrc if you chose to add it:

`source ~/.zshrc`

https://docs.openstack.org/newton/user-guide/common/cli-install-openstack-command-line-clients.html
