---
title: I Wrote My First Go Program Today
date: 2023-03-28
tags:
- Go
- Coding
- Neovim
---

I'm still at the beginning of my Go learning journey, but I worked through a few tutorials and guides by now. I've gathered lots of ideas for programs that I want to write, big and small, but I have to start somewhere.

The best thing to do is to write little programs that solve a problem that you have.

One problem I needed to solve was converting sentences to title case in vim. There are plugins for this, or elaborate macros, but I thought this was a nice opportunity to write my first program from scratch. You can view the program here: [my go repo](https://github.com/mischavandenburg/go/tree/main/projects/title).

# Title

Converting a string to title case is fairly easy:

```go

// Package title converts a string to title case.

package title

import (
	"fmt"
	"strings"
)

func Make(s string) {
	t := strings.Title(s)
	fmt.Printf("%v\n", t)
}
```

This function takes a string as an argument and uses the Title function from the strings package to convert it. I use printf to format the output and to add a new line character.

# Challenge

The challenge lies in taking input from Stdin. I solved this by reusing the things I learned from the greet challenge. I described how to read from standard input in go in [this article](/zet/go-reading-stdinput-cmdline).

Here is the code for the title command:

```go
package main

import (
	"log"
	"os"
	"strings"

	"github.com/mischavandenburg/go/projects/title"
	"github.com/mischavandenburg/go/projects/title/internal"
)

func main() {
	var x string
	var err error
	if len(os.Args) > 1 {
		x = strings.Join(os.Args[1:], " ")
	}
	if x == "" {
		x, err = internal.ReadLine(os.Stdin)
	}
	if err != nil {
		log.Print(err)
		return
	}
	title.Make(x)
}
```

This main function checks if arguments are given to the program from the command line, and uses these arguments if they are given. If there are no arguments, it will expect input to be piped to it. It takes either the input from the arguments or from standard input and calls the `Make()` function that I described above.

This main function uses a Readline function to extract the string from standard input:

```go
package internal

import (
	"bufio"
	"io"
	"strings"
)

// ReadLine takes anything of type io.Reader and returns a trimmed string (initial
// and trailing white space) or an empty string and error if any error
// is encountered.
func ReadLine(in io.Reader) (string, error) {
	out, err := bufio.NewReader(in).ReadString('\n')
	out = strings.TrimSpace(out)
	return out, err
}
```

For a more detailed explanation of this function read [this article](/zet/go-reading-stdinput-cmdline) I wrote.


# Usage

I wrote the program so I can call it with a sentence as an argument:

```bash
mischa@mac-beast:~
(ins)$ title hello world, this is my sentence in title case
Hello World, This Is My Sentence In Title Case
```

But the main use case is to use it as a [UNIX filter](https://www.geeksforgeeks.org/pipes-and-filters-in-linux-unix/) inside of vim. A UNIX filter is a program that takes standard input, performs some operation on the input, and prints it to standard output. 

Now, when I'm working inside of vim, I can convert the current line to title case by typing the following command:

`!!title`

The `!!` command sends the specified amount of lines to the command you specify. Exclamation marks truly are magic wands! I highly recommend reading [this article](https://rwx.gg/tools/editors/vi/how/magic/) by rwxrob to learn more about them.

## Links:

202303280803

https://github.com/mischavandenburg/go/tree/main/projects/title

https://rwx.gg/tools/editors/vi/how/magic/

[Reading from standard input in Go](/zet/go-reading-stdinput-cmdline)
