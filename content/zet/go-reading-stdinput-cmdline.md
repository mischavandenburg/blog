---
title: "Go - Reading from Standard Input Provided by User"
date: 2023-03-26
tags:

- Go
- Coding
- Explanations
---

I'm working through the "greet" [challenge](https://rwx.gg/lang/cha/) by rwxrob. It is amazing how such a relatively simple and small challenge can lead down to so many rabbit holes.

The program should take input from the user and print it out. I worked through the challenge together with Rob [in his video](https://www.youtube.com/watch?v=WMH5ENF_Xvo) but I'm going to talk (write) myself through these functions to fully understand what's going on.

We have the following function in main.go:

```go
func main() {
	var name string
	var err error
	if len(os.Args) > 1 {
		name = strings.Join(os.Args[1:], " ")
	}
	if name == "" {
		fmt.Println("Hello there, what's your name?")
		name, err = internal.ReadLine(os.Stdin)
		if err != nil {
			log.Print(err)
			return
		}
	}
	greet.Hi(name)
}
```

If the number of arguments passed to the program is greater than 1, we set `name` to a joined string created from the provided arguments. Args[0] would be the path to the program, so we don't want that to be included. As a result, `greet mischa` will pass `mischa` to the greet.Hi() function, as defined in greet.go, and print out `Hello, mischa`.

However, if no arguments are passed to the greet program, discovered in case `name` is empty, we ask the user for input. We capture the input by `os.Stdin` and pass it to the `ReadLine()` function, which is located at internal/readline.go. 

```go
// ReadLine takes any io.Reader and returns a trimmed string (initial
// and trailing white space) or an empty string and error if any error
// is encountered.
func ReadLine(in io.Reader) (string, error) {
	out, err := bufio.NewReader(in).ReadString('\n')
	out = strings.TrimSpace(out)
	return out, err
}
}
```

ReadLine has a parameter `in` of type io.Reader, which is an interface. Next, we determine that ReadLine should return two values of type string and error. I'm saving to learn about interfaces for another day, I'm just going to work through this function now.

We assign the output of `bufio.NewReader(in).ReadString('\n')` to two new variables named `out` and `err` using the "walrus operator" `:=` which detects the types automatically. We can do it like this because ReadString returns (string, error). 

We take the `in` argument of type `io.Reader` which was passed to the ReadLine function, which in this case is the `io.Stdin` that came from our main function, and pass it on to `bufio.NewReader(in)`. Then we are able to read the string until the newline character `\n` in the string, and trim off the whitespace from the beginning and the end of the string by calling TrimSpace on the `out` variable. 

Then we return the trimmed string back to our original main function, which will pass it on to the `Hi()` function.

However, if the `bufio.NewReader(in).ReadString('\n')` should return an error, it is caught by this code in the main function:

```go
		if err != nil {
			log.Print(err)
			return
		}
```

This is a standard way of handling errors in Go. If the error is anything else than nil, we will print the error and end the function with the return keyword.

# Thoughts

I'm really glad I took the time to talk / write myself through this program. I think I'm going to make a habit of this as I'm learning Go. It made everything much clearer when I sat down and traced the arguments from function to function and describing every step in my own words.

I'll post this note in the YouTube comments, maybe somebody will find it useful as well.

# Links:

This page goes a lot deeper in what stdin and stdout actually do in this context. Very interesting reading:

https://stackoverflow.com/questions/12363030/read-from-initial-stdin-in-go

The code in my repo:

https://github.com/mischavandenburg/go/tree/main/rwxrob/boost2022/challenges/greet

[[go]]

[[go-rwxrob]]

[[coding]]

[[functions]]



