---
title: "Installing packages in Devcontainers with Nix"
date: 2024-05-26
tags:
- Nix
- Linux
- Dev-environments
---

I've been obsessed with Devcontainers and Devpods recently, and I'm learning a lot about portable dev environments. This is truly the next level of config management and the future of dev environments in my opinion.

I relied on brew as my package manager, however, I ran into problems when I wanted to run my Linux dev containers on my M2 Silicon Mac. Linux brew is not supported on ARM architecture.

# Use case

I all I need to do is install a list of packages into a linux container. I was using brew because I need to install packages like k9s and flux which are sometimes not available in the standard package repositories.

After some tinkering with Nix I managed to solve my problem.

# Nixpkgs

My dotfiles contains this setup script:

```bash
#!/bin/bash
    export XDG_CONFIG_HOME="$HOME"/.config
    mkdir -p "$XDG_CONFIG_HOME"
    mkdir -p "$XDG_CONFIG_HOME"/nixpkgs

    ln -sf "$PWD/nvim" "$XDG_CONFIG_HOME"/nvim

    ln -sf "$PWD/.bash_profile" "$HOME"/.bash_profile
    ln -sf "$PWD/.bashrc" "$HOME"/.bashrc
    ln -sf "$PWD/.inputrc" "$HOME"/.inputrc
    ln -sf "$PWD/.tmux.conf" "$HOME"/.tmux.conf
    ln -sf "$PWD/config.nix" "$XDG_CONFIG_HOME"/nixpkgs/config.nix

    # install Nix packages from config.nix
    nix-env -iA nixpkgs.myPackages
```
It sets up the nixpkgs directory and the config.nix.

The config.nix contains the packages I want to install:

```nix
{
  packageOverrides = pkgs: with pkgs; {
    myPackages = pkgs.buildEnv {
      name = "mischa-tools";
      paths = [
        neovim
        go
        nodejs_22
        starship
        fd
        ripgrep
        lazygit
        kubectl
        k9s
        fluxcd
      ];
    };
  };
}
```

Finally, the `nix-env -iA nixpkgs.myPackages` command installs all of the packages into my environment.

I was lucky to find this in the Nix documentation:

https://nixos.org/manual/nixpkgs/stable/#sec-declarative-package-management

Had I not found this particular heading, I think I would have been lost in the weeds for a long time. But Nix has definitely piqued my interest and I look forward to learn more about it.

## Links:

Devpod dotfiles: https://github.com/mischavandenburg/dotfiles-devpod

202405261105
