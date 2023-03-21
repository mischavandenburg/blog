---
title: How to Surround a Word in Quotes in Vim 
date: 2023-03-21
tags:
- Zettelkasten
- Neovim
- Coding
---

I find myself quoting words very often in vim when I'm writing bash code. I used to do this by simply navigating around the word and typing them, but I knew there had to be a better way.

I found this vim command:

`ciw""<Esc>P`

"c" deletes into register and enters insert mode. "iw" stands for "inner word" and selects the word. 

So we delete the entire word and enter insert mode. Then we type two quotes, and we press "P" to paste the register (containing the word) before the cursor.

Voila, the word is surrounded by quotes.

To make it even easier, I added this to my keymaps, and I'll add a few more for parentheses and brackets.

`vim.keymap.set("n", "<leader>wsq", 'ciw""<Esc>P', { desc = "Word Surround Quotes" })`
