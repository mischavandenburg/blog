---
title: I Made My First Tweet Using My Go Program
date: 2023-04-08
tags:
- Go
- Coding
---

I spent the evening learning about the Twitter API. It was not as straight forward as I thought. My goal was to do this project using only the standard library, and I hoped to get away with a few simple curls, but since the API requires OAuth 1 to create tweets, I had to revise my strategy.

After struggling with Postman for a few hours to get the correct environment variables set up I managed to make my first tweet through Postman. Turns out that Twitter made some big changes in the free tier of their API, and it took me quite a while to figure out that the functions that are used as examples in the API documentation are not accessible to free accounts anymore. 

When I realized I would not get away with a simple curl I looked into libraries, but many were deprecated. Eventually I discovered [gotwi](https://github.com/michimani/gotwi) and it didn't take long to make my first tweet using Go. 

![](/tweet.png)

It feels like cheating, but looking at the complexity of the library, it looks like I would be stuck in the weeds for quite a while if I wanted to figure all of this out by myself. 

There is still enough to figure out in this project, such as curling my RSS feed and creating a tweet out of that, and wrapping it in a CLI, so in the end I'm glad I found a working library.

Here's the program I used to make my first tweet. It is a modified example from the gotwi GitHub readme.

```go
package main

import (
	"context"
	"fmt"
	"os"

	"github.com/michimani/gotwi"
	"github.com/michimani/gotwi/tweet/managetweet"
	"github.com/michimani/gotwi/tweet/managetweet/types"
)

func main() {
	in := &gotwi.NewClientInput{
		AuthenticationMethod: gotwi.AuthenMethodOAuth1UserContext,
		OAuthToken:           os.Getenv("TWITTER_ACCESS_TOKEN"),
		OAuthTokenSecret:     os.Getenv("TWITTER_ACCESS_TOKEN_SECRET"),
	}

	c, err := gotwi.NewClient(in)
	if err != nil {
		fmt.Println(err)
		return
	}

	p := &types.CreateInput{
		Text: gotwi.String("Hello World! This time I'm using os.Getenv in my Go program to load the credentials.\n#go #coding #study #learning"),
	}

	res, err := managetweet.Create(context.Background(), c, p)
	if err != nil {
		fmt.Println(err.Error())
		return
	}

	fmt.Printf("[%s] %s\n", gotwi.StringValue(res.Data.ID), gotwi.StringValue(res.Data.Text))
}
```

## Links:

202304082104

https://github.com/michimani/gotwi

https://mischavandenburg.com/zet/go-twitter-cli-project/

[[go]]

[[go-twitter-cli-project]]

