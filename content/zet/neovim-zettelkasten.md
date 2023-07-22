---
title: "My Neovim Zettelkasten: How I Take Notes in Markdown Using Vim and Bash"
date: 2023-03-27
tags:
- Zettelkasten
- Productivity
- Second-Brain
- Notetaking
- Article
---

I used to make all my notes on paper, but I decided to switch to a digital note-taking system about two years ago.

Digital note-taking provides the following advantages:

* Searchability
* Collected in one place
* Can be converted to different output formats
* Easier to share with others
* Does not take up physical space
* No risk of losing your notes in case of fire or other disaster

I've gone through several iterations of my note-taking systems. I started on Google Docs, moved on to Notion and eventually landed on Obsidian. However, as I became more proficient with UNIX systems and vim, I realized I did not need all of that distracting functionality, and I switched over to using Neovim and a few bash scripts. I occasionally open up Obsidian to look at my graph view or to make use of the Anki plugin, but I enjoy the trimmed-down version that I built myself. I don't need to leave the command line to read my notes or to create a new one.

In this article I'll show you the system I'm currently using for note-taking. My note-taking system is part of a larger system which I call my "Second Brain", [a term coined by Tiago Forte.](https://fortelabs.com/blog/basboverview/) This is an overarching system I've build to manage my time, projects, tasks and bookmarks. Explaining this entire system is beyond the scope of this article, but I plan to write about my Second Brain in the future as well.

My system is based on the Zettelkasten method, but I've adjusted it to my own needs. After a few years of experimentation I feel that my system has reached a "mature" state, but it will always remain continuosly under development, also because my needs may change.

# What is a Zettelkasten?

Zettelkasten is a system of note taking and personal knowledge management. [Zettelkasten](https://en.wikipedia.org/wiki/Zettelkasten) means "slip box" in German. Traditionally it is a physical box of slips of paper or index cards containing smaller notes. Each of these cards have a unique identifier, and these identifiers are used to create links between the cards.

[Niklas Luhmann](https://en.wikipedia.org/wiki/Niklas_Luhmann), who was one of the most prolific scholars in history, used it extensively. He wrote 70 books and nearly 400 academic articles, and he credited the Zettelkasten with making his productivity possible. It contained around 90,000 index cards.

I first came across this concept by reading the book [How to take Smart Notes](https://www.soenkeahrens.de/en/takesmartnotes) by Sönke Ahrens. 

# Why Take Notes?

>"Your professional success and quality of life depend directly on your ability to manage information effectively."
>
>Tiago Forte, Building A Second Brain

In the modern age we consume very large amounts of information, much more than we can remember. It is therefore very important to manage your intake of information, but also the retention of that information. I find that taking notes on the topics that may be of interest to me at any given time helps me understand the subject much better, because it forces me to clearly formulate my thoughts and convert them to written form. 

Over time a large collection is built up which can be used to generate new ideas.

# What Do I Take Notes On?

Firstly, I keep a daily journal where I write down my thoughts, my feelings and the activities that I undertake. Some days may be several pages worth, and other days will be only "Had a nice day today". But I make a point to write something down every day.

Secondly, I write about the topics I study. I'm always learning something new. I'm a DevOps Cloud Engineer, and new developments are made constantly in the Cloud Native landscape. There is always more to learn.

My notes are not only work related. I also keep notes of the research I do for all of my interests, such as health, longevity, exercise, diet, literature, yoga and meditation. I made a habit out of always writing a note when I read a book, watch a documentary or listen to a podcast. These notes can be a short summary, a few thoughts that come to mind, or a few bullet points that capture the main takeaways.

These activities are usually accompanied by taking notes:
* Reading books
* Watching video courses
* Watching educational YouTube videos
* Researching hobbies and interests
* Coding
* Reflection
* Interesting conversations
* Listening to podcasts

# An Overview of My System

My system is a modified Zettelkasten system that incorporates daily notes for journaling. The Zettelkasten method emphasizes small atomic notes containing only your own thoughts and links to other notes. However, I choose to also make longer notes. For example, when I did the [Fundamentals of Bicep](https://mischavandenburg.com/zet/articles/fundamentals-of-bicep) course, I made one large note that contains all of the information I wished to remember or refer to later. In this particular use case, I think it's much better to keep everything collected in one note, rather than breaking this note up in smaller notes on parameters and variables, for example. 

Another area where I diverge from the traditional Zettelkasten is categorization of notes. Traditionally the Zettelkasten is one big repository of notes, but I like to keep them loosely collected. When I create a new note, my system places that note in an inbox directory. Once a week I go through my inbox and revisit all of my notes. Sometimes I will throw some notes away because I don't think they're necessary after all. When I decide to keep it, I put it in a directory that I find suitable. I have a directory structure based on the [PARA method](https://fortelabs.com/blog/para/), but I also have a large Zettelkasten directory where I put anything that does not necessary belong to a category. I mostly keep some notes collected in a directory because I expect to use them as a collection at some point, or because I want to run scripts on the files.

Every day a daily note is created that contains a few checkboxes for habit tracking and sometimes I'll include an inspirational quote in my template for a while. 

This is what a daily note looks like in my editor:

![](/dailynote.png)

# Local Text Files

I think it is important to use locally stored markdown text files. This is mainly why I moved away from Notion. My notes are stored on my local machine rather than with a third party. This gives me complete control over my data.

Because my notes are stored as markdown files, I can use various tools to write or edit my  notes. Moreover, storing my notes as local files allows me to run scripts on my notes and customize my workflow to my heart's content. This can be especially useful for automating tasks, streamlining my work, or making bulk updates.

I have my collection backed up in several places and on GitHub.

I recommend [this article](https://rwx.gg/lang/md/) about the benefits and merits of using markdown for note-taking.

# Vim, My Preferred Text Editor

I use Vim, or technically, Neovim for all of my text editing. It took a while to get used to, but my productivity related to text editing has increased significantly. I also find it very enjoyable to work on the command line, not having to leave my tmux window to manage my entire Zettelkasten or keep my journal.

My [vim configuration](https://github.com/mischavandenburg/dotfiles) is always subject to constant change, but I only use three plugins that are related to my note-taking and Zettelkasten.

## 1. Telescope

I cannot live without [this plugin](https://github.com/nvim-telescope/telescope.nvim) any longer. It is a fuzzy searcher integrated into my editor. I can search for files but also search within the files using ripgrep, and preview the files as I'm searching. 

Heres what a search for Neovim looks like:

![](/telescope.png)

I can navigate through all the search results blazingly fast using only the keyboard, and I can preview the files by scrolling up and down in the window on the right hand side.

## 2. Marksman LSP

Marksman is a markdown language server which helps me format my documents and to create links between notes. 

![](/marksman.png)

When I start to make a link it will search through my entire Zettelkasten and look for matches based on the filename or markdown headers. Here it shows me suggestions related to markdown.

## 3. Pandoc

According to Wikipedia, "Pandoc is a free-software document converter, widely used as a writing tool (especially by scholars) and as a basis for publishing workflows."

I use the Pandoc plugin for vim to export my notes to different formats when I need to.

However, the thing I like most about it is that it renders my notes very nicely as I'm writing them. I really like how headings look, and that it will **make text bold** while editing, which did not work when I used nvim-markdown.

# Bash

To facilitate my workflow I wrote a few bash scripts. 

When I want to take a note, I run the `zet` command from the command line. This command is a bash script I wrote which creates a file in my inbox, adds a template to it, and opens the file in Neovim. I can either provide a filename to it as an argument or it will ask me to provide one.

I have another script named `blog` which I use to create notes that I intend on publishing on my website. These files are stored in a different location and use a different template. The blog script also has a `pub` function that will publish the script directly to my website in a few seconds after I'm done writing.

These scripts can be found in my [dotfiles repo](https://github.com/mischavandenburg/dotfiles/tree/main/scripts).

# Identifiers and Filenames

The Zettelkasten method assigns unique identifiers to notes. I generate a unique identifier from the current date and time, but I never really use these identifiers. However, since I've been doing this consistently from the beginning, I'll keep generating them to make my system future proof. Who knows, maybe they will become useful someday when I suddenly find a use case for it.

I've seen other people use unique identifiers in filenames as well, but this is a big no for me. I want to be able to discern what the contents might be from the filename whenever I'm navigating around my directory structure. Using the filename as the note identifier is a much better solution in my opinion. There is a small chance that you will choose a filename that already exists, but you can easily add a number to it or change a word and you have solved that problem.

Moreover, I use the marksman LSP server to create links between my notes in Neovim. The LSP server is based on headings and filenames, so I'm really glad that I chose to use filenames such as "fundamentals-of-bicep.md" right from the beginning.

# Tags and Links

If you are as deep into note-taking systems as I am, you might have come across the problem of using tags versus using links for your note-taking system. 

I primarily use `[[markdown links]]` to create links in my notes. This is also why you will sometimes see these things in my public zettelkasten. The links between notes are rendered beatifully by Obsidian in the graph view. This is also why I'll always try to keep my system compatible with Obsidian, even though I've moved away from the application almost entirely.

Here is a graph view of my Zettelkasten:

![](/graph-view.png)

For the notes in my Zettelkasten which are intended to be published on my website, I use tags in the YAML frontmatter. These are used by my static website generator, Hugo, to create tag pages on my website which I think will be useful to my readers.

Here is an example of the YAML front matter of one of these notes:

```yaml
---
title: "I'm In Love with my Work: Lessons from a Japanese Sushi Master"
date: 2022-10-08
tags:
- Career
- Personal
- Article
---
```

# Conclusion

That's about it. My system is a minimalistic set of tools utilized for maximum productivity. It's Neovim with a few plugins and a couple of helper scripts I wrote in bash that store markdown files in a directory structure that I find meaningful. After using many different applications and solutions, I'm extremely satisfied with the system I built for  myself which is very basic, free from distractions and tailored to my own needs.

When I want to reflect on a certain topic or to write an article, I go through my collection of notes and link them together. Very often I find that the creation of these links will stimulate even more new connections and associations, and I end up with new ideas and more topics of study I want to explore and write about. I use my Zettelkasten as a vehicle for reflection, learning and creativity.

I love the simplicity of my system. It enables me to capture notes very quickly while I'm working or studying with very little effort. By storing my note collection on my iCloud drive and in GitHub, they can be accessed from all of my devices across all different operating systems. 

I hope that this article may give you some inspiration to start building your own note collection. You don't need to build your own system like I have done, any app that satisfies your needs can be used. Keeping notes on the things I encounter in life is one of the most enriching habits I've acquired. It will be interesting to see how large my collection grows over the years!

# Links:

202303270703

https://fortelabs.com/blog/basboverview/

https://fortelabs.com/blog/para/

https://rwx.gg/lang/md/

https://github.com/mischavandenburg/dotfiles/tree/main/scripts


