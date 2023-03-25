---
date: "2022-04-02T09:55:03Z"
tags:
- Linux
- Article
title: 'Yadm: Keep Track of Your Precious Dotfiles'
url: /yadm-keep-track-of-your-precious-dotfiles/
---

This week I learned about [yadm: yet another dotfile manager](https://yadm.io). It is the perfect way to keep track of all your custom configuration files, known as dotfiles.

Even if you have only a little bit of experience with Linux, you know that everything is managed in files. All configuration parameters are set or changed in text files stored on the hard disk. These files are usually located in your home directory and are hidden by default. This is indicated by prefixing the file with a period. So the configuration file for the vim editor is .vimrc, and for zshell you use the .zshrc. This is why configuration files are referred to as dotfiles.

### customisation

The more I work with Linux, the more I appreciate the ability to customize things. When I first started, I was pretty satisfied with the vanilla experience. You punch your commands into the terminal, and you do your tasks. What more could you need?

This started to change when I began working with senior engineers who built their custom setups over the years. I saw them opening 3 terminal windows in a perfect ratio with beautiful colorschemes or previewing files in a file browser directly in vim so they could split them vertically and edit them side by side.

I wanted to create a similar setup by adding settings and plugins to the .vimrc and .zshrc files. However, before going down this rabbit hole, I asked myself the following question. How can I bring this configuration with me to other machines? What happens if my laptop gets stolen and I lose my precious configuration files?

### yet another dotfile manager

Enter yadm. I had thought of putting my dotfiles in a GitHub repo, but this brings up a whole set of new challenges where you would need to create symbolic links across your system to have the files in their correct places. Yadm solves this problem.

Yadm turns your home directory ( ~/ ) into a Git repo which can be pushed to a host of your choice. You can add your files one by one, and yadm will track them. The best thing is that you can add the files from all over your system, and yadm will not bother with any of the other files in your home directory.

### you want git for your dotfiles

Setting up your configuration files in a git repository has a lot of advantages:

- configuration is saved in multiple places
- easily share your configuration across machines
- version control

Version control is especially useful. You will always be able to trace back that one plugin you used a few years ago, but you cannot remember the name of. And it is fun to watch your configuration grow over time.

### setting up yadm

[Installing yadm](https://yadm.io/docs/install) is a breeze. For my mac I just used

```bash
brew install yadm
```

or you can use the apt-get or dnf install equivalents if you are on Linux.

**You interact with yadm the same way you interact with git. You simply replace the word git with yadm in the commands.**

Then you navigate to your home directory and set up the repository. If you don’t have a repository yet:

```bash
yadm init
yadm add <important file>
yadm commit

yadm remote add origin <url>
yadm push -u origin <local branch>:<remote branch>
```

Or if you already have a dotfiles repository:

```bash
yadm clone <url>
yadm status
```

And that’s it. Now add your configuration files and push them to your hosted repo:

```bash
yadm add ~/.vimrc 
yadm add ~/.zshrc
yadm commit -m "first commit"
yadm push
```

You will notice that yadm expects you to add all the files every time you want to make a new commit. Use this command to stage all the files you added previously:

```bash
yadm add -u
```

### enjoy your synched customisation

Having your dotfiles in a GitHub repo makes it easy to set up your preferred settings on a new machine or environment. So install yadm and pull your repo, and off you go!

I hope you will enjoy it as much as I do. Crafting a customized setup takes a lot of time and effort, and now that I finally have an excellent solution to keep track of my files, I am ready to dive into the customization rabbit hole. 


Download yadm [here.](https://yadm.io) Here you will also find all the necessary information to install and configure your yadm.
