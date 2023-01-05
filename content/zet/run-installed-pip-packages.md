---
title: How to run installed pip packages as binaries 
date: 2023-01-05
tags:
- Linux
- How-To
---

When you install a pip package which is meant to be run from the command line as a command, you might find that it is not available to you after installation.

If this happens, it might be that the path is missing from your PATH variable. Therefore, the shell does not source these binaries when initiated, and does not know that these executables exist.

You can find the location of your binaries by running `pip show package_name`

Usually the binaries will be located in ~/.local/bin on a UNIX based system.

To add this to your path, run:

`export PATH="$HOME/.local/bin:$PATH"`

Add this to your ~/.zshrc or ~/.bashrc to make sure this happens for each shell session.

Don't forget to source your updated ~/.zshrc if you chose to add it:

`source ~/.zshrc`

https://stackoverflow.com/questions/29980798/where-does-pip-install-its-packages
