---
title: How to follow symbolic links while searching with Telescope in neovim
date: 2023-01-06
tags:
- Neovim
- Linux
- How-To
---
I use the [Obsidian](/zet/articles/obsidian-introduction/) app, but I mostly write and search my notes with neovim. I added my zet directory from this blog repo into the Obsidian vault as a symbolic link, but I soon discovered that these files were not being searched. 

Telescope.nvim uses ripgrep (rg) to do the live grepping in its search, and ripgrep does not follow symbolic links by default. You need to pass the -L flag to it.

To pass the -L flag, and some other flags, I added the following to my telescope config file:

```lua
-- Custom ripgrep configuration:

local telescope = require("telescope")
local telescopeConfig = require("telescope.config")

-- Clone the default Telescope configuration
local vimgrep_arguments = { unpack(telescopeConfig.values.vimgrep_arguments) }

-- I want to search in hidden/dot files.
table.insert(vimgrep_arguments, "--hidden")
-- I don't want to search in the `.git` directory.
table.insert(vimgrep_arguments, "--glob")
table.insert(vimgrep_arguments, "!**/.git/*")

-- I want to follow symbolic links
table.insert(vimgrep_arguments, "-L")

telescope.setup({
	defaults = {
		-- `hidden = true` is not supported in text grep commands.
		vimgrep_arguments = vimgrep_arguments,
	},
	pickers = {
		find_files = {
			-- `hidden = true` will still show the inside of `.git/` as it's not `.gitignore`d.
			find_command = { "rg", "--files", "--hidden", "--glob", "!**/.git/*", "-L" },
		},
	},
})
```

Based on the configuration examples found on the project's GitHub page.

https://github.com/nvim-telescope/telescope.nvim
