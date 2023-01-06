---
title: Using "//" syntax as search and replace
date: 2023-01-06
tags:
- Linux
- Zettelkasten
---

In this evening's studies I came across this bash script in a tutorial by rwx.gg:

```bash
!#/bin/bash
echo -e ${PATH//:/\\n}
```

I could not make heads or tails of all these slashes, since the output clearly indicated that search and replacement was being performed. I'm used to the sed / vim syntax: `s/foo/bar`

After some research I learned that '//' is a global search and replace syntax of several text processing programs. 

Example:

```bash
foo="1234567890"
echo "${foo//[0-9]/x}"
```

This replaces all the digits in the $foo variable with 'x', so the output would be xxxxxxxxxx

To do this with sed, you would do:

```bash
echo "$foo" | sed 's/[0-9]/x/g'
```

