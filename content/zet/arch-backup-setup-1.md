---
title: Setting up automated backups on my Arch Linux system with rsync and bash
date: 2023-01-21
tags:
- Zettelkasten
- Linux
---
For the past few months I've been stuyding every hour of free time that I had. Now that I reached my certification goals for now, I finally had some time to do a chore I had been meaning to do for a long time.

My Arch Linux system is fully encrypted, and I make backups. But I was still doing it a bit haphazardly, usually every Friday. I fired the commmand of by hand. 

I wanted to automate this for a long time now, but I never got round to it. Today I made the first steps, but it is still in progress.

Naturally, I could use a tool like Timeshift or something similar to schedule my backups. However, I want to do it myself using rsync because I want to fully understand what I am backing up, when, and where. Rsync is also used in our environment at work, so I assume it is more common in enterprise and production environments.

## full system backup

Before I was making a full system backup every Friday using this command:

```bash
sudo rsync -aAXH --info=stats1,progress2 --exclude={"/dev/*","/proc/*","/sys/*","/tmp/*","/run/*","/mnt/*","/media/*","/lost+found","/home/*/.cache/*","/data-hdd/","/games/","/var/lib/docker/*","/home/mischa/music/*","/swapfile", "/data-hdd2/", "/data-hdd3/"} / /data-hdd/backups/arch-beast/01-01-23
```

This command creates a full backup of my entire root filesystem, and it should be possible to restore my entire system by just reversing the target and destination in the end. 

However, as I was coming up with my new strategy, I thought this was overkill.

## slimming down

All I really need to back up is my home directory and it would be nice to have my /etc directory backed up as well. 

So I wrote a simple shell script to do this:

```bash
#!/bin/bash

BACKUPS_DESTINATION="/data-hdd/backups/arch-beast"

# format:
# rsync -a --delete --quiet /path/to/backup /location/of/backup

# stop the script if an error occurs
set -e

rsync -a --delete --quiet --exclude="{"/home/*/.cache/*"}" /home/mischa $BACKUPS_DESTINATION/home
rsync -a --delete --quiet /etc $BACKUPS_DESTINATION

echo "Made backups on: $(date)" >> /var/log/backup.log
```

-a flag from man page:

*"This  is  equivalent to -rlptgoD.  It is a quick way of saying you want recursion and want to preserve almost everything."*

--delete: means files deleted on the source are to be deleted on the backup as well

## automation

I have a few scripts running in cronjobs on my system. I have a goal of putting them all in systemd timers, but I haven't gotten round to it yet. For now, I will just add my backup scripts to my existing cronjobs setup.

To make my backups every day, I added this to my crontab:

`0 12 * * * /bin/bash /home/mischa/git/lab/bash/backup`

Every day it will make a backup to the same directory and update the changed files, or delete the files I deleted from my system.

I also wanted to have a weekly backup happening on Monday.

I will make a more elaborate script to make a weekly directory, and rotate it with a new directory every week. But for now, I just chose a quick solution by creating a weekly version of my script and running it every Monday.

The only difference is the path:

`BACKUPS_DESTINATION="/data-hdd/backups/arch-beast/weekly"`

In the crontab:

`0 10 * * 1 /bin/bash /home/mischa/git/lab/bash/backup-weekly`

## to do

- set up weekly backup in the same script
- create error handling and improve logging
- set up in systemd timers instead of crontab
