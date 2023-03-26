---
title: "Go - Skillstak Beginner Boost Week 17 and 18 Notes"
date: 2023-03-26
tags:
- Zettelkasten
- Study
- Go
- Coding
---

# Beginner Boost Week 17 and 18 Notes

[Link to video](https://www.youtube.com/watch?v=WMH5ENF_Xvo)

* Don't forget to set `GOBIN=~/.local/bin`, `GOPRIVATE`, `CGO_ENABLED=0`

# Go Testing - Example Tests

```go
func ExampleFoo() {

	foo()

	// Output:
	// Foo

}
```

The ExampleFoo indicates the test here. It needs to match the name exactly after Example. But it is capitalized.

It runs that function and will compare the output to what is specified.

It says "see if the program generates this output in std out".

These are called example tests.

To export a function the first letter should be a capital. Everything that has a capital as first letter is exported.

When you are writing example tests you are providing readable automatic documentation to your end users. 

You can use `// Unordered Output` if you don't care about the order.

## Printf

use `printf %q` to see all of the characters. `%q` escapes all of the invisible characters. So you can see \r or \n for example. This is the only way to check for empty values in Example tests.

# How to Learn Go

Don't go to the books. Stay with the spec, write your own projects, and find one or two or 10 Go projects and study the crap out of them. 

Study the code bases. Kind is a good codebase.

Read the Go codebase!

https://youtu.be/9hEnzD-bNy4?t=8467

Read other people's code and see if your code looks like that.

"Good artists copy, great artists steal." - quote accredited to Steve Jobs but it has its origin in T.S. Eliot and even further back.

**Start with the tests! Read the tests first.**

# Searching Go Documentation

Always search for golang when searching on the net. Preferably text based searching. 

Then you use `go doc os.Stderr` - for example.

# Variable Names

Long variable names are frowned upon in the Go community. They should be as short as they need to be. 

If using in a tight scope, a single letter is fine. In a block two or three characters is more than enough. It distracts from reading the code if they have very long names. 

The more remote the variable is, the more descriptive it should be. 

# The Art of Coding

* breaking everything down into small 1 task functions
* don't repeat yourself

See [stuttering](#stuttering)  

# Top Level Libraries

It's not common to make a single utility at a top-level GitHub repo. 

It's very likely that you are going to want to reuse code somewhere. 

You should start thinking of things as composition. "How is this code going to be used by other people". 

Go is different than bash: you will use this code elsewhere.

* What is the function of what I'm creating?
* Create these as small composable blocks
* So you can use them later

# Example Based Testing

https://pkg.go.dev/testing

> The package also runs and verifies example code. Example functions may include a
concluding line comment that begins with "Output:" and is compared with the
standard output of the function when the tests are run. (The comparison ignores
leading and trailing space.) These are examples of an example:

```go
func ExampleHello() {
    fmt.Println("hello")
    // Output: hello
}

func ExampleSalutations() {
    fmt.Println("hello, and")
    fmt.Println("goodbye")
    // Output:
    // hello, and
    // goodbye
}
```

# Searching Information

Being able to look up information quickly and taking notes about them is just as important as the coding itself. 

# Packages or Libraries

You can either have a command, which will be a main package, or you can have an importable library named <lib>.go. Should have any other name besides main. 

You rarely want your top level of your module to be a package. 

Convention: cmd directory.

This is specifically for separate commands.

## Stuttering

`greet.Greet()` is called stuttering in go. Don't do this.

# Modules

A module can be defined as "a GitHub repo with a go.mod" in it.

"A collection of Go packages stored in a file tree with a go.mod file at its root"

The go.mod contains the import path of the module.

## What Module Does Greet Belong To?

In the case of our greet command, it belongs to the greet module located at:

`github.com/mischavandenburg/go/rwxrob/boost2022/challenges/greet`

Greet has a main package which is part of the greet module.

Now we will add another command with another main package to this module.

# CLI

Stuff that has to do with interacting with the user on the command line should never be in the package library. The package library should be written to be solid no matter what. 

The "name" for our greet program should be able to come to anywhere. 

# Avoid Interactive Input

You generally want to avoid interactive input. Prefer arguments, env variables or file inputs.

If you call your program from another script it will stall if it waits for interactive input. 

A good example is forgetting "-y" when running apt-get. 

An interactive story game is a different use case than a CLI tool.

UNIX filters are specifically designed to read input and generate an output based on the input. Here it is expected behaviour to stall if no input is given. 

# Runes

A rune is a single Unicode point. 

The same people who created Go, created the Unicode standard.

Go has the best Unicode support.

# Writing Documentation

Convention is to always start with the name of the thing you are documenting. When documenting a function, always start with the function name. For example `// ReadLine reads from standard input`

Use `go doc --all` to see a print of all the documentation in your package. 

The same goes for packages. Always start with "Package internal does bla bla"

# Links

202303261403

https://www.youtube.com/watch?v=WMH5ENF_Xvo

[[go]]

[[coding]]

[[rwxrob]]
