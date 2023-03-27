---
title: Using parameter expansion as search and replace
date: 2023-01-10
tags:
- Linux

- Linux
- Bash
- Coding
---
*Last modified: 2023-01-10*

In this evening's studies I came across this bash script in a tutorial by Rob Muhlenstein:

```bash
!#/bin/bash
echo -e ${PATH//:/\\n}
```

I could not make heads or tails of all these slashes and curly braces, since the output clearly indicated that search and replacement was being performed. I'm used to the sed / vim syntax: `s/foo/bar`

After some research I learned that '//' is a global search and replace syntax of several text processing programs. It is known as parameter expansion in bash.

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

For more info:

`man bash`

`/parameter expansion`
