---
title: adding encrypted drive luks
date: 2023-01-15
draft: true
tags:

---
```bash

NAME         MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINTS
sda            8:0    0 223.6G  0 disk  
└─sda1         8:1    0 223.6G  0 part  
  └─games    254:2    0 223.6G  0 crypt /games
sdb            8:16   0   2.7T  0 disk  
└─sdb1         8:17   0   2.7T  0 part  
sdc            8:32   0 931.5G  0 disk  
└─sdc1         8:33   0 931.5G  0 part  
  └─data-hdd 254:1    0 931.5G  0 crypt /data-hdd
sdd            8:48   0 465.8G  0 disk  
├─sdd1         8:49   0   300M  0 part  /boot
└─sdd2         8:50   0 465.5G  0 part  
  └─root     254:0    0 465.5G  0 crypt /


```
