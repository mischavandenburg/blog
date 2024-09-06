---
title: Running iCloud Web as a Desktop Application on Arch Linux with Hyprland
date: 2024-09-06
tags:
- Productivity
- Linux
---

Now that I'm daily driving Arch Linux again, I've been thinking about how to handle my tasks. I use Apple Reminders because the iWatch functionality with Siri is so good. I've been thinking of setting up a self-hosted solution, but then I would lose the Siri integration.

In the meantime, I figured I would just try to access Apple Reminders from Linux. Using it from the browser worked surprisingly well.

Created a desktop file here with the name `icloud.desktop`:

`/home/mischa/.local/share/applications`

With these contents:

```bash
[Desktop Entry]
Name=iCloud
Exec=firefox --new-instance --profile /home/mischa/.mozilla/firefox/icloud-profile --kiosk https://icloud.com
Icon=firefox
Type=Application
Categories=Network;WebBrowser;
```

Now I can run `wofi` and search for iCloud, and I have my iCloud applications running in a clean separate window within a workspace.

Also, I can configure Hyprland to run this application in a specific workspace on startup.

## Links

202409060909
