---
title: "Generating Markdown Headings with YYYY-MM-DD Date Format Using Bash"
date: 2023-04-01
tags:
- Linux
- Coding
- Bash
---

A couple of weeks ago I created a simple bash script to generate a date in format YYY-MM-DD to use with the magic !! wands in vim. 

Just now I wanted to expand this with being able to create a markdown header with this date. 

```bash
#!/bin/bash
# Gendate generates the date in YYY-MM-DD format
# Can be called with arguments h and number to generate a markdown heading
# For example: 'gendate h 3' will generate '### 2023-03-29'

header=""
date=$(date +"%Y-%m-%d")

if [[ $# -gt 0 ]]; then

    # handling wrong arguments
    if [[ $# -eq 1 || "$1" != "h" ]]; then
        echo "Usage: gendate h 2 to generate with markdown heading ##"
        exit 1
    fi

    # format markdown heading if arguments h, n are given
    if [[ $# -eq 2 ]]; then
        for i in $(seq 1 $2); do
            header+="#"
        done
        header+=" "$date
        echo "$header"
        exit 1
    fi
fi

# if no arguments given, generate the date
echo "$date"
```

## Links:

202304011104

https://github.com/mischavandenburg/dotfiles/tree/main/scripts

