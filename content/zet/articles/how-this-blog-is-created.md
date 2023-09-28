---
title: "How This Blog is Created, Written and Hosted"
date: 2023-01-03
tags:
- Neovim
- Productivity
- Personal
- Explanations
- Linux
---

{{< youtube bzkrmkGDQJA >}}

As I alluded to in my [article about Obsidian,](/zet/articles/obsidian-introduction) I am very fond of editing my text in neovim. Naturally, if you want to edit in neovim, you need to have your text as local files. I keep all of my personal notes in markdown.

Previously I was using WordPress, but the editing and writing experience became torture which I could not endure any longer. I looked for a different solution that would allow me to edit my files locally instead of in the browser.

I discovered [Hugo](https://gohugo.io/) and I fell in love with it immediately.

Hugo is a static site generator based on markdown files. My entire blog is written in markdown files which are stored [in a GitHub repo.](https://github.com/mischavandenburg/blog). I write my blog posts in vim and when I'm done I use Hugo to generate the updated website.

The result is what you see in the "public" directory in the GitHub repo. This public directory is pushed to a different repo which is hooked up to my hosting provider. My hosting provider uses Plesk, and with Plesk I have the option to connect the GitHub repo to the web server with a webhook. When I push to my hosting repo, the contents are gathered by the webserver and served as public web content. 

My complete writing and publishing workflow looks like this:

1. Create a new markdown file
2. Write the note or article
3. Save the file and run the "hugo" command to regenerate the website
4. Run the "publish" script. This is a custom script I wrote that takes the contents of the "public" directory to my hosting repo
5. Push the newly generated website to the hosting repo
6. And we're live! 🚀 🎉

It is such a smooth and convenient process. I can literally have a new note published to the interet within a few minutes, and it is all done from the command line using my favorite tools. 

[Blog GitHub repo](https://github.com/mischavandenburg/blog)
