---
title: "Learning Go Day 1: Notes" 
date: 2023-02-28
tags:

- Go
- Study
- Coding
---

The CTO of my new company recommended the Udemy course "Go: The Complete Developer's Guide (Golang)". I started today and here are some notes I made.

# Hello World in Go

We start by writing a Hello World and studying all the elements.

```go
package main

import "fmt"

func main() {

fmt.Println("Hello World!")

}
```

### How do we run code?

`go run main.go` runs the program
`go build main.go` compiles it to an executable

### What does package main mean?

`package main`

A package is a collection of common source code files.

One app is a package. If you have multiple files in a folder, such as helper.go or support.go, they should have `package main` to indicate that they belong to package main. 

*Why do we call it main?

There are two types of packages. 

- executable
	- generates file that can be run
- reusable
	- used as "helpers"
	- reusable logic

When you call the package `main`, you are telling the compiler it needs to be compiled as an executable. If it  has a different name, it won't generate an executable. Main is sacred. 

Any other name is a reusable or dependency type package (helper code). 

Another important point is that whenever you create an executable package, it must always have a func called 'main'. 

### What does import fmt mean?

The import statement is used to give our package access to code written in another package.  You are saying "give access to all code in fmt". Fmt is a standard library package included in Go. Short for format. Used to print out information to the terminal. 

Other packages included in the standard library of go are debug, math, encoding, crypto, io. 

golang.org/pkg for documentation on standard library packages for Go.

A lot of learning go is learning the standard packages and how they work. 

### Organizing the main.go file

It is the same for every go file, just like the code example at the top of the page. Package main, import fmt, and func main.

# variable declarations

`var card string = "Ace of Spades"`

var: we are about to create a new variable

card: name

string = telling the go compiler that only strings will be assigned to this variable

### Alternatively:

`card := "Ace of Spades"`

Here you are relying on the compiler to figure out what type it is. 

Compiler will infer the type.

We only use this := assignment for **new variables**

If you want to assign a value to a variable after it is declared, you just do `card = "Five of Diamonds"`

### Go types

Go is  a statically typed language. 

Javascript, python are dynamically typed language. We don't care what value is assigned to a variable.

You always a assign a type to a variable in Go. 

Basic go types:
- bool
- string
- into
- float64 : a number with a decimal after it.
