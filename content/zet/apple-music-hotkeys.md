---
title: Controlling Apple Music with hotkeys from anywhere on MacOS
date: 2023-02-24
tags:
- Zettelkasten
- MacOS
- Productivity
- Coding
---

I'm a little obsessed with controlling everything with my keyboard. That's why I loved AwesomeWM so much on my Arch Linux setup, I hardly used my mouse anymore.

One thing I loved about my setup was the ability to control my music from the keyboard from anywhere. This is a feature I picked up from the awesome-copycats theme for AwesomeWM. This was one of the first things I missed when I made my switch to MacOS. 

I started using Apple Music as my music app but it does not have any global hotkeys, and it makes you use a widget with the mouse.

# skhd

I solved the problem using [skhd](https://github.com/koekeishiya/skhd). This is a free hotkey daemon for MacOS. To install:

```bash
brew install koekeishiya/formulae/skhd
brew services start skhd
```
Then I added the following to my skhd config file:

```bash
# Control apple music globally
# Based on Aesome-copycats theme for AesomeWM

ctrl + cmd + fn - up : osascript -e 'tell app "Music" to playpause'
ctrl + cmd + fn - left : osascript -e 'tell app "Music" to back track'
ctrl + cmd + fn - right : osascript -e 'tell app "Music" to play next track'
```

# links

https://github.com/koekeishiya/skhd
