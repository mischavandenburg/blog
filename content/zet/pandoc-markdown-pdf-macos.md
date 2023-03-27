---
title: Converting markdown to PDF on MacOS
date: 2023-03-25
tags:

- MacOS
- Notetaking
- Writing
- Neovim
---

It happens that I want to share my notes with friends that just simply want a pdf instead of a markdown file. 

This morning I figured out a quick way to convert markdown to pdf on a M2 MacBook running MacOS Ventura.

You need the pandoc and wkhtmltopdf packages.

```bash
brew install pandoc wkhtmltopdf 
```


To convert:

```bash
pandoc 00-zettelkasten/Fundamentals\ of\ Bicep.md --pdf-engine=wkhtmltopdf  -o /tmp/test.pdf
```

This will output a pdf to my /tmp/ directory and it looks pretty good.

To convert all markdown files in a directory you can use a wildcard:

```bash
pandoc 00-zettelkasten/*.md --pdf-engine=wkhtmltopdf  -o /tmp/test.pdf
```

Fun fact: converting my entire zettelkasten took a few seconds and generated a document of 80 pages with a font size of 10. Pretty fun to see it in a more regular format instead of the terminal.
