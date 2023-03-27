---
title: "What is the difference between a Go module and a package?"
date: 2023-03-20
tags:

- Go
- Coding
---

A module is generally associated with a single git repo.

You can have a module with multiple packages, and each package would get its own subdirectory.

You should always name your main file main.go

## creating a module

Use the `go mod init {{your path here}}` command to initiate a module. 

## multiple modules

I was running into some trouble with this because I want to have one big repo where I will store all my go projects.

My gopls LSP in Neovim would start throwing errors when I added multiple projects in my repo.

The fix is to create a separate directory for each project. For example:

/go/hello/main.go

/go/hi/main.go

Hello and Hi are each separate projects.

Now I enter each of these directories and run `go mod init hello`

I'm sure this isn't good practice for production code, but it serves its purpose to collect all my learning code in one place.

https://www.youtube.com/watch?v=9hEnzD-bNy4
