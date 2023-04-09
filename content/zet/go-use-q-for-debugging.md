---
title: Use The %q Verb When Debugging In Go
date: 2023-04-09
tags:
- Go
- Coding
---

I'm working on my twitter CLI and as I was writing a function to format the tweet I remembered something I picked up last week. After a quick search in my notes I remembered to use the `%q` with printf. 

```go
slice := []string{feed.Items[0].Title, feed.Items[0].Link}
result := strings.Join(slice, "\n")
fmt.Printf("Testing printf %q", result)
```

This is very useful when formatting output. Now I can actually see whether it is inserting the new line characters correctly:

`Testing printf "I Made My First Tweet Using My Go Program\nhttps://mischavandenburg.com/zet/go-first-tweet/"I Made My First Tweet Using My Go Program`

This can be harder to see when printing the variable using `fmt.Println` or using the `%v` verb with `Printf`.

## Links:

202304091304

[[go]]

[[coding]]
