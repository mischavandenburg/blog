---
title: How to continuously run a Go file while coding in the terminal
date: 2023-03-19
tags:

- Go
- Coding
- Linux
- Neovim
---

I do all my coding and note taking in the terminal using tmux and neovim. I picked up a nice trick from Rob Muhlenstein today.

You can use this command in a split window to keep running a Go file. It will update when you save the file.

`entr -c bash -c "go run main.go" <<< main.go`

Entr runs commands when files change. Here we are feeding it only one file, but you can also feed it a directory like so: 

`find src/ | entr -s 'make'`

Super handy to see the outcome of your code changes in real time.

To run all the files in the directory, use the following:

`entr -c bash -c "go run . " < <(find .)`

I picked this up while going through Rob's Beginner Boost of 2022:

https://youtu.be/kwrN3jbv4sE


