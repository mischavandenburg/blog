---
date: "2022-05-26T09:03:44Z"
tags:
- Linux
title: How To Move Steam Game Files To a Separate Hard Drive on Linux
url: /how-to-move-your-steam-game-files-to-a-separate-hard-drive-on-linux/
---

I have installed my OS on a 240GB SSD, and I prefer to keep my data on a different disk to leave enough space to work with. I wanted to move my steam game files to a separate hard drive on Linux. I’ll show you what I did to make this happen in this article. I use Manjaro GNOME.

First, you need a correctly partitioned hard drive.

To wipe your drive clean and have a single partition on it, we’ll use GParted.

![](/gparted1.png)
Select your disk in the upper right corner.

Then go to Device and select Create Partition Table:

![](/parted2.png)

Follow the wizard and use an ext4 filesystem. NTFS can cause problems because Steam cannot read it properly.

# mounting

To use a disk or a partition in Linux, it needs to be mounted.

List your devices and identify the one you wish to mount by using the “lsblk” command.

In my case, I wish to mount the drive sdc1

![](/lsblk.png)

On Linux, all filesystems need to be mounted before they can be used. I wanted my whole disk to be available in the directory /mnt/data

Before mounting, I created the directory.

`cd /mnt`

`sudo mkdir data`

When you make the directory by using sudo, the directory owner will be the root user. This means that you cannot access the directory and write to it from your own user.

Use this command to change the ownership of the directory. Replace “mischa” with your username.

`sudo chmod mischa:mischa data`

Verify that the directory now has the correct ownership:

![](/steamgames1.png)

Now you can mount your directory, so it is available for use.

`mount /dev/sdc1 /mnt/data`

# mounting on boot

For the mount to happen automatically on startup, you’ll need to add it to the /etc/fstab file. We start by finding the UUID of our disk.

Use the following command:

`ls -al /dev/disk/by-uuid/`

![](/steamgames2.png)

In my case the UUID will be 50d608bc-a7ad-4ff6-bf44-bb6f26efa4f6

### /etc/fstab

open the file in your favorite editor. I like to use vim.

`sudo vim /etc/fstab`

Add a new entry to your /etc/fstab file and use the following parameters:

`UUID=50d608bc-a7ad-4ff6-bf44-bb6f26efa4f6 /mnt/data ext4 defaults 0 0`

![](/steamgames3.png)

Before we go further, verify that we did this correctly by using the following command:

`findmnt --`verify

This will verify the /etc/fstab file. Not meaning to scare you, but an incorrectly configured fstab may lead to an unbootable system.

Now reboot your system and check if your disk is mounted automatically.

It is also a good idea to cd to your mounted directory and touch a file to see if you have write permissions.

![](/steamgames4.png)

# Steam

Now it’s time to set things up in Steam. Open Steam and open your settings.

![](/steamgames5.png)

go to Downloads –> Steam Library Folders

Click the + button and navigate to your mounted drive.

![](/steamgames6.png)

To test, install a game and reboot your system.
