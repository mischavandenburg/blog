---
title: How to Run Newsboat in Zenmode
date: 2023-03-26
tags:

- Linux
- Productivity
- Command-Line
- Neovim
- How-To
---

To read the news free from distractions and ads I use Newsboat as a reader for RSS feeds.

However, one thing that annoyed me was that it would span across my entire screen in the terminal. When you read blogs or news pages in the browser, you'll notice that the text is always located in a middle column of the window, so you don't have to move your neck while reading. At least, this is the case with well designed websites that serve text content.

Vim has a plugin that achieves this and I use it extensively. It is called Zenmode. In Neovim I use a similar plugin called No Neckpain. 

To achieve a similar configuration for Newsboat I used tmux. I wrote the following bash script:

```bash

#!/bin/bash
# nb opens a new pane and runs newsboat in it. I want to read from a centered column in my screen.
tmux split-pane -h \; resize-pane -x 130\; send -t 2 "newsboat" Enter\; send -t 1 "clear" Enter
```

Note that this needs to be run from within an existing tmux window with no split panes.

It splits the current window in to two panes, resizes the new pane to a width of 130 pixels and sends the "newsboat" command to the new pane, and the "clear" command to the old (left) pane to keep it nice and clean.

In my `~/.newsboat/config` file I added the following setting:

`text-width 72`

This will limit the text on the right hand side of the screen.

The end result looks like this:

![Newsboat Zenmode](/newsboat-zen.png)

## Links:

202303260903

https://github.com/folke/zen-mode.nvim

https://github.com/shortcuts/no-neck-pain.nvim
