---
Title: Structs In Go - Similar To Classes In Python?
date: 2023-04-10
tags:
- Go
- Coding
- Python
- Study
---

The Tour of Go is very clear: 

> Go does not have classes.

One benefit of learning multiple programming languages is that each language provides you with a set of “pegs” that you can use to refer to other languages. As I learned about structs in Go, I hung them to the “Python classes” peg and used that as a reference point. Using these reference points can help you to understand the object of study by looking at their differences and similarities.

Even though Go does not have classes, my understanding of Python classes did help me to understand structs much more quickly.

# Go Structs

A struct is a collection of fields, and they are accessed using a dot.

Each data field in a struct has its own type, either user defined or built in.

```go
type Person struct {
 name    string
 address string
}

person1 := Person{"Mischa", "Amsterdam"}

fmt.Println(person1.name)
fmt.Println(person1.address)
```

When creating a struct, you can use the `Name:` syntax to set the values. Otherwise, you need to populate the fields in order.

```go
type Computer struct {
os string
ram int
}

func main() {
macBook := Computer{os: "MacOs"}
fmt.Println(macBook)

thinkPad := Computer{"Arch Linux", 16}
fmt.Println(thinkPad)
}
```

This produces the output:

```go
{MacOs 0}
{Arch Linux 16}
```

In the first line, the ram is 0 because I only set the `os` field, and unset fields get a `0` value by default.

# Struct Methods

Classes in Python can contain methods. What about Go structs?

Structs can have methods, but they are not contained in the struct definition, like you would see in Python. Methods are defined on types, and the type does not need to be a struct. Therefore, methods are defined after you create the struct.

```go
type Computer struct {
os string
ram int
}

func (c Computer) doubleRam() string {
return fmt.Sprintf("If you would double your ram, you would have %v GB of ram.", c.ram  * 2)
}

func main() {
macBook := Computer{"MacOs", 32}
fmt.Println(macBook.doubleRam())

}
```

Output:

`If you would double your ram, you would have 64 GB of ram.`

In this example, `doubleRam()` is my method of the `Computer` type. Methods take a special receiver argument, written between the `func` keyword and the method name. The `doubleRam` method has a receiver of type `Computer`, named `c`.

# Struct Literal

A struct literal is a struct which has its contents defined in the source code itself. The opposite would be to generate the contents of the struct through computation or reading memory during the execution of the program.

# Conclusion

In conclusion, here are the similarities and differences that stood out to me in this morning's study of Go structs.

## Similarities

* Both can have fields to hold values of different types
* Both can have methods 

## Differences

* Go structs are used for data structures, classes in Python are used for OOP
* Go struct fields have static types
* Methods are defined separately from the struct definition


# Links:

202304100704

[[go]]

[[data-types]]

https://go.dev/tour/moretypes/2

https://go.dev/tour/methods/1

https://articles.wesionary.team/map-vs-struct-in-golang-when-to-use-b0b66446627a
