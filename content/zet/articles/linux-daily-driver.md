---
date: "2022-04-26T06:38:57Z"
image: /wp-content/uploads/2022/04/Tux.svg.png
tags:
- Linux
title: Goodbye Windows, Hello Linux! Switching to Linux as my Daily Driver
url: /goodbye-windows-hello-linux-switching-to-linux-as-my-daily-driver/
---

After getting my LPIC-1 certification, my interest in Linux continued to grow. When I started my new job, I performed more sysadmin tasks, such as increasing the size of filesystems or removing backups, and it felt good to put the theory into practice.

I was still using Windows in my personal setup, and I started running into limitations. Finally, I realized how much I appreciated the freedom and control over my system that Linux gave me. So I decided I wanted to switch to Linux for my daily operating system. But a few things were holding me back. Will I be able to play my favorite games? Will I run into a lot of problems with my sound and microphone? Are all the apps I need for working from home available on Linux? Can I even edit videos on Linux?

## preparing to switch

I made a dual boot install of Ubuntu to try things out to answer these questions. I started things off by setting small goals for myself. For example, I need to be able to work from home. Which programs do I need? And I started from there.

I had no problems installing Slack, Zoom, Teams, and all the other programs I needed for my work. I was very surprised by how well all of the external hardware worked. My Bluetooth keyboard and mouse worked immediately without needing to install any drivers. Even my USB webcam worked instantly without any problems.

To put this into perspective, I spent an entire afternoon getting my keyboard to work correctly on my MacBook. It needed a lot of extra drivers. Still, there is a 4-second delay when I press my volume up/down keys when working on my MacBook. All of this works perfectly on my Linux OS without any delays and without any drivers or extra configuration.

I kept my dual boot setup for a while as I was trying out different distros, and I eventually ended up with Manjaro as my distro of choice. I like it because it is based on Arch Linux, and it gives you access to the Arch User Repository (AUR). I want to use Arch eventually, and I figured this might be a good stepping stone.

## fate decided for me

This dual boot setup continued for a while as I was warming up to the idea of completely abandoning Windows. I set Linux as my default boot option, and after a few weeks, I realized I hadn’t needed to boot into Windows for anything at all. However, I still didn’t feel quite ready to switch completely to Linux.

One evening I wanted to make another fresh install to check out the GNOME version of Manjaro. I was doing a lot of chores at the same time, and it was getting quite late, but I wanted to have the installer running as I was doing other tasks. Probably not my most brilliant move.

You probably know what is coming: in between my chores, I started the installation. In a moment of carelessness, I managed to point the installation to my Windows partition, and it was completely wiped and replaced with a sparkling fresh Linux install.

“Well, I guess I am moving to Linux today!” I thought while I suppressed a hint of panic as I racked my brain to see if I had lost any important files. I knew that most of my important stuff was safely backed up in the cloud. But if I had formatted my Windows drive by choice, rather than by accident, I would have backed up a lot more files.

## first week without windows

A week ago, I lost my complete Windows install, but there hasn’t been a single moment where I regretted making the switch. Fortunately, it also seems that I did not lose anything important.

I am learning so much by forcing myself to use Linux as a daily driver. Most things are correctly configured out of the box. But sometimes, you have to do some work to get the configuration you like.

For instance, after installing Steam, I wanted to have the game files located on a different hard disk because my OS SSD is only 256GB. This required me to format my data SSD to an ext4 filesystem and mount it in a folder. I also needed to add it to my /etc/fstab file to make sure that it mounts automatically when I boot my PC.

These tasks have been great practice for the things I need to do on my servers at work, and they will make me approach these tasks with a little more confidence because I have done them before on my personal setup. This is the great advantage of having Linux as a daily driver if you are becoming a DevOps Engineer or Linux System Administrator.
