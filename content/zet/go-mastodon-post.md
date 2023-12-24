---
title: Wrote A Go Program That Posts To Mastodon
date: 2023-12-24
tags:
- Go
- Coding
- DevOps
---

This morning I had some fun writing a Go program which takes the latest blog post from my RSS feed and posts it to Mastodon. It is based on my [twitter cli](https://github.com/mischavandenburg/twitter-cli) I wrote a while ago. 

It was fun to write some Go again, it has been far too long. I've been writing mostly Terraform and yaml recently and working a lot with infrastructure. I haven't been doing any projects at home that required programming. But recently I started up my personal blog and completely re-architected my social media setup.

I'm going to post all of my personal stuff on Mastodon from here on out, not on X. However, I still wanted to post my tech content on Mastodon as well and I needed a tool to help me with that.

I have this program installed as a binary on my computer and I call it from my [blog script](https://github.com/mischavandenburg/dotfiles/blob/main/scripts/blog) which I use to generate my website with Hugo and publish it to my web hosting solution. Now it can post to Twitter / X and Mastodon as well.

The Go code I wrote is available here:

https://github.com/mischavandenburg/mastodon

## Links:

202312241112
