---
title: The Return of Arch Linux
date: 2024-07-26
tags:
  - Linux
  - Command-line
---

I'm planning to do a series on Linux on my YouTube channel. The idea is to cover the fundamentals of Linux, either by teaching them myself or to link to existing resources. When the basics are done, I will take over and we'll build an Arch Linux desktop together.

I use my MacBook for normal productive work and coding, and I have a Thinkpad T480 which I have for experimentation, and mostly for writing notes and browsing while I'm in the living room. It was running Fedora using the Sway spin, and it worked fine.

But Arch Linux has been gnawing at me ever since I started my community. We've had some lovely discussions about distros and window managers, and I almost always commented that I enjoyed building my Arch Linux system.

Well, I went back to it!

This time I'm going even more minimal. I took the challenge of not using NetworkManager, and I figured I would go with systemd-boot right away too.

Some of you might remember the problems that happened with GRUB about 2 years ago. Systemd-boot feels so much more intuitive and straightforward to configure.

So now I have a super clean and minimal system using system-boot and systemd-networkd with iwd.

Getting internet to work was a bit of a challenge but figured it out eventually.

Using systemd-resolved, I currently don't even have a /etc/hosts file on my Arch Linux system. That's a new one for me.

This is the current pstree while ssh'd into my Thinkpad from the MacBook.

I'm so happy with the current minimal state of it!

```bash
[mischa@thinkpad ~]$ pstree
systemd─┬─dbus-broker-lau───dbus-broker
        ├─iwd
        ├─sshd───sshd-session───sshd-session───bash───pstree
        ├─systemd───(sd-pam)
        ├─systemd-journal
        ├─systemd-logind
        ├─systemd-network
        ├─systemd-resolve
        ├─systemd-udevd
        └─systemd-userdbd───3*[systemd-userwor]
```
