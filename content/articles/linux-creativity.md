---
date: "2022-09-10T12:22:28Z"
tags:
- Linux
- Personal
title: 'Building my Own OS: Linux as a Creative Activity'
---

*NOTE: In this article, I use a rather broad definition of “Operating System.” I do not intend to appear as if I wrote and compiled my own Linux kernel, nor do I understand the inner workings of the kernel written in C. Instead, with “building my OS from scratch,” I intend to convey that I used a minimal Linux distribution as a starting point and started building from there.*

## introduction

I started using GNU/Linux as a daily driver about six months ago, and I have not regretted the decision ever since. There has not been a single use case where I needed to use Windows for anything at all.

As I was getting more used to daily driving Linux, I noticed how much I enjoyed the ability to customize my operating system and workflow. Finally, after spending a weekend going down the customization rabbit hole, I had a good-looking terminal and customized neovim to perform as I needed it.

Not much later, I came across Arch Linux and the idea of building your own operating system from the ground up. I was instantly intrigued and knew I wanted to do the same. A few months have passed since I first came across Arch Linux, and now I am writing this blog post in neovim on my custom OS that I created from scratch. The font, the spacing, the colors, everything is exactly how I like it, and I love using it.

When you first install Arch Linux, all you get is a black screen with a blinking cursor. However, the experience of creating a fully functional graphical environment from “nothing” has been extremely satisfying, and I learned so much about GNU/Linux in the process. I also realized that this could be seen as a creative activity, like a painter creating his masterpiece from a blank canvas or a sculptor carefully chipping away at a block of marble.

## not just graphics

When I say customization, I am not just referring to the visual aspects of the operating system. The things going on “under the hood” must also be carefully configured when you use a minimal distro such as Arch Linux.

Arch Linux comes with very few packages preinstalled, and every time you wish to add something to your system, you need to install it and enable the service in systemd. For example, after I did the installation and created my user account, I needed to run a command with root privileges. To my great surprise, even the “sudo” command was unavailable and needed to be installed.

This is the aspect I learned most from. Whenever I desired a certain functionality from my operating system, I needed to install and enable it. This has given me a much better understanding of the processes and daemons running on my system. It has also given me a greater appreciation of all the elements needed to provide a working environment.

## graphic violence

When you create an Arch Linux installation USB and boot it up, you are greeted with a command line and nothing else.

![](/creative1.png)

When you install something more beginner-friendly, such as Ubuntu or Manjaro KDE, your installation will include a graphical desktop environment. But on Arch Linux, you must install and configure this yourself. Furthermore, to be able to render a graphical environment, you will also need to install and configure a display server such as Xorg.

When I started on my journey, I intended to create something that used minimal resources with a minimal look. Having used GNOME on Manjaro for a few months, I was very satisfied, but I wanted to try a tiling window manager to shave down even more resource usage. After some research, I ended up with the Awesome Window Manager. Here are some screenshots of the final result:

![](/creative2.png)

This is what my desktop looks like when I boot up.

![](/creative3.png)
Here I’m editing my window manager configuration file, while I have a browser open and keep an eye on my system resources

![](/creative4.png)

my music listening setup, using mpd + ncmpcpp, cava and sptlrx. the lyrics are shown in real time as the music is played.

## creativity

[The Cambridge Dictionary](https://dictionary.cambridge.org/dictionary/english) defines creativity as “the ability to produce original and unusual ideas, or **to make something new or imaginative**.”

When you embark on a journey, such as creating your operating system, you will probably start with a particular intention or a goal that you will work towards. With this goal in mind, you can start searching for the tools and color schemes you need to create the system that you have in mind. The result is a unique combination of tools, colors, fonts, and programs specifically tailored to your needs and wants and chosen by you.

Is this any different from a painter starting with a blank canvas or a musician starting with a fragment of a melody, ending up with a complete symphony? Entering commands into a computer terminal might not strike everybody as a creative activity. Still, I have found that it is a very effective and satisfying way of expressing myself and creating something I love to use daily. As an IT professional, I spend most of my time behind my computer. Doesn’t it make sense to put effort into building something customized to your needs?

#### resources

If you want to start building your own OS, I recommend these resources:

[Arch Linux](https://archlinux.org/)

[Arch Wiki](https://wiki.archlinux.org/)

[r/unixporn – a subreddit about customization](https://www.reddit.com/r/unixporn/)
