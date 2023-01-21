---
title: Setting up a new LUKS encrypted disk with dm-crypt in Arch Linux
date: 2023-01-21
tags:
- Article
- Linux
- How-To
---
Today I added a harddisk I had lying around because I needed some more space. On my Arch Linux system I have all my drives encrypted like a good boy. It can be a bit tricky when you are adding them because you need to configure a few different files and add different UUID's in each of them.

Here are the steps I follow to add a new disk. Note that this how to assumes that you already have set up your system with dm-crypt.

List out the disks with lsblk:

```bash
(ins)[mischa@arch-beast ~]$ lsblk
NAME          MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINTS
sda             8:0    0 223.6G  0 disk
└─sda1          8:1    0 223.6G  0 part
  └─games     254:0    0 223.6G  0 crypt /games
sdb             8:16   0 931.5G  0 disk
└─sdb1          8:17   0 931.5G  0 part
  └─data-hdd  254:2    0 931.5G  0 crypt /data-hdd
sdc             8:32   0 931.5G  0 disk
└─sdc1          8:33   0 931.5G  0 part
  └─data-hdd2 254:3    0 931.5G  0 crypt /data-hdd2
sdd             8:48   0 465.8G  0 disk
├─sdd1          8:49   0   300M  0 part  /boot
└─sdd2          8:50   0 465.5G  0 part
  └─root      254:1    0 465.5G  0 crypt /
sde             8:64   0 931.5G  0 disk
└─sde1          8:65   0 931.5G  0 part
```

I will be adding /dev/sde to my system. As you see, I already created a partition on it, named `sde1`. The mountpoint for the disk will be `/data-hdd3`.

If you still need to add your partition, use `sudo gdisk /dev/sde` to write a new table and partition. 

## encryption

First I create the mount point I'll use and set the appropriate permisssions:

```bash
sudo mkdir /data-hdd3
sudo chown mischa:mischa /data-hdd3
```

Now we create a LUKS header and an encrypted filesystem on the disk.
Note that I'm using the notation convention from the Arch Wiki where the "#" indicates that the command should be run as root.

```bash
# cryptsetup -y -v luksFormat /dev/sde1
# cryptsetup open /dev/sde1 data-hdd3 
# mkfs.ext4 /dev/mapper/data-hdd3
# mount /dev/mapper/data-hdd3 /data-hdd3
```

Verify that it worked and the new encrypted partition is mounted:

```bash
arch-beast# lsblk
NAME          MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINTS
sda             8:0    0 223.6G  0 disk
└─sda1          8:1    0 223.6G  0 part
  └─games     254:0    0 223.6G  0 crypt /games
sdb             8:16   0 931.5G  0 disk
└─sdb1          8:17   0 931.5G  0 part
  └─data-hdd  254:2    0 931.5G  0 crypt /data-hdd
sdc             8:32   0 931.5G  0 disk
└─sdc1          8:33   0 931.5G  0 part
  └─data-hdd2 254:3    0 931.5G  0 crypt /data-hdd2
sdd             8:48   0 465.8G  0 disk
├─sdd1          8:49   0   300M  0 part  /boot
└─sdd2          8:50   0 465.5G  0 part
  └─root      254:1    0 465.5G  0 crypt /
sde             8:64   0 931.5G  0 disk
└─sde1          8:65   0 931.5G  0 part
  └─data-hdd3 254:4    0 931.5G  0 crypt /data-hdd3
```

## Auto mounting at boot

We'll need to add this disk to the grub parameters, /etc/crypttab and /etc/fstab. I haven't gotten round to switching to systemd boot yet, but I will do so very soon.

Open tmux and split the pane. In the bottom pane, run `lsblk -f` to have all the UUIDs listed. Then open the grub configuration file with `sudoedit /etc/default/grub`

![](/luks1.png)

You can discern which uuid to add from the listed examples. For my new disk, I needed to add the following:

`rd.luks.name=3169af6c-a129-448e-b451-d7767866f607 data-hdd3=/dev/mapper/data-hdd3`

Then run `sudo grub-mkconfig -o /boot/grub/grub.cfg` to update grub with the new settings. Adjust the path if you use a different path for your boot partition!

Next, we add it to /etc/crypttab

![](/luks2.png)

To mount the new encrypted partition at boot, we add it to /etc/fstab.

**Note that this time we need to use the UUID of the partition located at /dev/mapper/data-hdd3**

![](/luks3.png)

Use `sudo findmnt --verify` to check if there is antyhing wrong with the file.

Now you should be able to reboot and your new encrypted disk should be mounted automatically.

```bash
(ins)[mischa@arch-beast ~]$ lsblk
NAME          MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINTS
sda             8:0    0 223.6G  0 disk
└─sda1          8:1    0 223.6G  0 part
  └─games     254:1    0 223.6G  0 crypt /games
sdb             8:16   0 931.5G  0 disk
└─sdb1          8:17   0 931.5G  0 part
  └─data-hdd  254:0    0 931.5G  0 crypt /data-hdd
sdc             8:32   0 931.5G  0 disk
└─sdc1          8:33   0 931.5G  0 part
  └─data-hdd2 254:4    0 931.5G  0 crypt /data-hdd2
sdd             8:48   0 465.8G  0 disk
├─sdd1          8:49   0   300M  0 part  /boot
└─sdd2          8:50   0 465.5G  0 part
  └─root      254:2    0 465.5G  0 crypt /
sde             8:64   0 931.5G  0 disk
└─sde1          8:65   0 931.5G  0 part
  └─data-hdd3 254:3    0 931.5G  0 crypt /data-hdd3
```

# links

https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system#LUKS_on_a_partition

https://wiki.archlinux.org/title/GRUB
