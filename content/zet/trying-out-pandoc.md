---
title: Trying out pandoc for vim
date: 2023-03-23
tags:
- Zettelkasten
- Neovim
- Productivity
- Writing
- Second-brain
- Notetaking
---

I do all of my writing in markdown. I keep my diaries in my Second Brain, I constantly write notes on the topics I'm studying, and my entire blog is written in markdown.

Some of the writing is done in Obsidian but I'm moving away from Obsidian step by step. I'll still keep my second brain compatible with it, but I want to be able to do all of my writing in (neo)vim.

I've been using nvim-markdown for a while now and I was quite happy with it, but it was bothering me that **bold text** was not rendered in my editor. 

Pandoc had been on my radar for a while because it uses the Commonmark [markdown spec](https://spec.commonmark.org/) and it can be used to convert to many different documentation types. It's an interesting thought to keep my entire collection of notes and writings compatible with something that can be converted into anything.

I noticed rwxrob uses pandoc as one of his few vim plugins.

It took me an hour to make it compatible with neovim and to disable the spelling and folding. I couldn't just set the provided variables because they are still in vimscript and I haven't figured out how to set them yet because they use a hashtag in their name #

So far I like it, but there are a few things that I miss from my nvim-markdown
* automatic bulleted list
* nested bulleted lists
  * with nvim-markdown these lists would continue
  * Here I have to indent each line myself and add a bullet
* navigation between headers
  * I didn't use this much to be honest, but I haven't figured it out in pandoc yet. Maybe it is possible?

I find myself using bullete lists and nested lists a lot in my notes, but I wonder if it is better to step away from those and make more use of markdown headings.
