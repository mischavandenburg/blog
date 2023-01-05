---
title: Attaching the Ansible Language Server to yaml files in neovim (LSP)
date: 2023-01-05
tags:
- Neovim
---
When you have an Ansible language server installed, you might find that your yaml LSP will attach to your current buffer, but the ansible language server won't attach. 

You can fix this by setting the correct file type for the current buffer:

`:set ft=yaml.ansible`

You could also adjust the Ansible LSP so it attaches to all yaml files. However, this does not work out for me, because I edit different yaml files for different purposes every day. Not all yaml files are to be used with Ansible.

There is logic for the Ansible language server to figure out if you are working on Ansible yaml files based on the directory structure you're working in. 

So setting the filetype when I needed works well for me.

https://www.reddit.com/r/neovim/comments/tbd7g0/lsp_ansiblels_wont_attach_anymore/
