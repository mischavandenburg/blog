---
title: No More GitHub - My Updated Backup Setup For My Obsidian Zettelkasten
date: 2024-07-28
tags:
- Zettelkasten
- Note-taking
- Productivity
- Command-line
---

I relied on iCloud sync for my Obsidian backup for many years. However, my vault has grown significantly and has reached nearly 3500 notes. This leads to problems when opening the vault on other iOs devices, because iCloud sync removes files from devices when they are not used. When the vault is reopened, it takes over 5 minutes to sync everything. Having this 5 minute waiting time every time I open up my vault on my iPad was a problem which I needed to address.

These small frictions can become large irritations, even though I only use Obsidian on my iPad very occasionally.

To alleviate this problem I switched to Obsidian Sync. It's 4 dollars a month, and a great way to support the project. I've had Sync before, but switched to iCloud when I entered the Apple ecosystem.

## Git

In addition to iCloud and Obsidian Sync, I've also been backing up my vault to GitHub. I used this to sync my vault to Linux devices before I purchased Obsidian Sync again.

Syncing with Git has often led to problems. I've spent many hours reverting faulty commits where I was about to lose a bunch of data.

Another glaring problem is that I'm committing my entire web of knowledge, my deepest thoughts and reflections and personal information to GitHub. Even though it is in a private repository, it's likely that Microsoft can access it some way or other if they want to. I haven't researched it thoroughly, but using a free service like GitHub usually comes with privacy sacrifices.

I didn't mind it that much, but it was something that was gnawing on the back of my mind for years.

Today I pulled the plug on it. It felt strange to delete the repo, but it's gone now.

## Rsync + iCloud

My notes are safely backed up using Obsidian Sync. It's all encrypted at rest and in transfer. But I also want to have my own backups.

To be absolutely safe I'm also backing up the vault to my iCloud drive.

Yeah, now I'm giving Apple potential access to my files. But I think my data is safer with Apple than with GitHub where it can probably be used for training AI models.

The next step will be to back it up to my Synology NAS, but I'm running into some problems with mounting the SMB share from the command line, so I'll save that for later.

I'm using the following script to back up my Zettelkasten to my iCloud drive. It runs twice a day using a cron job on my MacBook Pro.

```bash
#!/bin/bash

SOURCE_DIR="$HOME/Zettelkasten"
DEST_DIR="$HOME/Library/Mobile Documents/com~apple~CloudDocs/backup/Zettelkasten"

mkdir -p "$DEST_DIR"

rsync -av --delete "$SOURCE_DIR/" "$DEST_DIR"

cd "$DEST_DIR" || {
  echo "Error: Unable to change to destination directory"
  exit 1
}

git add -A
git commit -m "Automatic backup commit $(date '+%Y-%m-%d %H:%M:%S')"

echo "Backup and commit completed successfully!"
```

## Keeping Git

As you can see, I'm still committing changes every time I back up. I'm keeping it as a local Git repo so I can revert commits when something unexpected happens.

If push comes to shove, I can always push (😏) it to a Git instance again, or even to GitHub.

I considered running my private Git instance in my homelab cluster, but I also don't want my Zettelkasten to be available there in case someone manages to break into my cluster. Even though I'm running enterprise grade security with Talos, there is always a risk because I'm exposing some services to the internet.




