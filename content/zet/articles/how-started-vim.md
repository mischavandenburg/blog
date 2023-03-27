---
title: "How and Why I Started Using Vim"
date: 2022-09-18
tags:
- Linux
- Neovim
- Article
---
If you are just starting your Linux journey, you might have noticed that a few camps exist in the Linux world. Just like in any other area of life, it seems that groups of human beings enjoy dividing themselves instead of living in harmony. There are camps centered around Linux distributions (I use Arch, btw) but also around text editors.

## The Beginning

The reason why I started to use vim is rather practical. When I was studying to become a Cloud Engineer, I had access to subscriptions on AWS and Azure to experiment with virtual machines. This was a perfect place to learn to work with Ansible. Many of the labs projects I did involved setting up a few virtual machines, and I destroyed many VMs when I made some big mistakes in the configuration. 

I was using Visual Studio Code at the time on my local machine, but I had to connect to a new virtual machine multiple times a day. It became very tiresome to set everything up with VSCode every time, or pulling the files to my local machine and copying them over again. So I just ssh'ed into the machines to edit the text files with the included editor, which happened to be vim. 

## Obsession in its Infancy

When you first use vim, it is a rather disorienting experience. But in every tutorial, I was told it would be difficult in the beginning but much faster and more effective in the end. I found this very appealing because I like to do things the hard way and challenge myself. 

I discovered that there were people out there who did all of their text editing and coding in vim. I met programmers who refuse to use anything else and people who write entire books in vim. So there had to be something to it. 

It also fitted very well with my intention of working on the command line as much as possible and moving away from GUI applications whenever possible. I like to move in this direction because I love the idea of controlling your entire workflow with your keyboard instead of using your mouse, and vim fits perfectly into this picture. 

## What I like after Nine Months

At this point, I've been using vim as my primary text editor for about nine months. In my current job, I work a lot with yaml files stored in private git repositories. 

I only work with these files from the command line, and I don't have any other code editor installed. I use ripgrep and fzf (fuzzy file finder) to search through the files, and I use neovim to edit them. When I need to search for files from within vim, I use the awesome Telescope plugin.

In these months, I've picked up a few tricks, and I am starting to see the power of vim. The best thing I like about it is that I don't have to leave my terminal window to do the tasks I need to do. Instead, I can search through the files I need to work with, open them, make adjustments, and commit them to the repository. Then I enter the command to run the ansible playbook, and it all happens in the same window, and I don't have to lift my fingers from the keyboard. 

#### Keyboard Shortcuts

Now that I am gaining more experience with vim, I'm picking up more advanced usages that significantly improve my workflow. For example, "da(" meaning "delete around parentheses" to quickly delete the text between two parentheses. Or "da<" to very quickly delete HTML tags. Another great feature is the visual block mode, where I can add comment tags to many lines simultaneously, for example. 

#### Searching and Navigation

Navigating large text files has become incredibly quick since I started using vim. Of course, it takes some getting used to, but it is a lovely experience to open a file, press / to search and enter the keyword and immediately arrive at the point I need to be—no scrolling with the mouse and no need to lift my hands from the keyboard.

I also love the ability to jump from sentence to sentence using ) or paragraphs using }.

#### Multiple Files

It takes a little while to get used to, but when you get into it, it is effortless to open up two files at a time if you need information from both. Often I need data from 4 or more files, and opening them quickly with keyboard commands has significantly improved my workflow speed. 

#### Customization

One of the things I enjoy most about vim is the ability to customize it exactly to my needs. I'm completely in charge of the plugins which are loaded into vim and which colors it uses, and this appeals a lot to me. However, it can be rather overwhelming in the beginning. To be honest, it is still overwhelming after ten months. It can be tough to get an idea of where to start, which plugins you need, and which settings you need to change. 

I just started with the base install of vim and started from there. Every time I required a particular functionality, I searched around to see if a plugin was available. Very often, someone out there had the same problem as you and created a plugin for it. For example, I recently installed a plugin for using emojis in vim 😄

## How to Get Started

The short answer is to simply start using vim for all of your text editing, whether it be coding or writing for pleasure. It is a cliche to say, but it will be hard in the beginning, but I promise you it will pay off in the end. 

The second thing I'd recommend is to run vimtutor on a Linux machine. Do this once a day for a couple of weeks, and you'll know how to edit text files on any Linux system for the rest of your life, which is a precious skill. 

Finally, don't spend too much time reading about all the available plugins. Your needs will become apparent to you as you start to use vim for all of your tasks, and you can search for plugins to address those needs. This way, you start with a minimal editor, which you'll build according to your needs.

# Good Luck!

