---
date: "2022-02-18T16:21:12Z"
tags:
  - Coding
title: My First Contribution to Open Source
url: /my-first-contribution-to-open-source/
---

Two months ago I knew nothing about GitHub. This week my first pull request got merged into master!

Programming tutorials and books very often suggest that you should try to contribute to open source in order to practice your skills. Even though I am still on the beginner level in Python, I managed to find something I could contribute with. But there were a few things I needed to learn in order to be able to do so.

GitHub is a place where many open source projects are hosted. Projects are hosted in “repositories” available to the public. Everyone can go in and take a look at the code. And the great thing about it is that everyone can contribute to the code as well.

Two months ago I knew almost nothing about GitHub. Surely, I had often downloaded software from GitHub, and I knew it had to do with version control. But I had no idea that it was such a powerful system of enabling collaboration for software projects.

During an assignment in my DevOps Traineeship I spent some time learning about Github and the Git language. I learned about repositories, branches, commits and pull requests. Now I wanted to take it to the next level and make a contribution of my own somewhere.

### the project

As I have mentioned in other posts, I love game automation, and recently I discovered the Botty project, which is a bot written for the game Diablo 2: Resurrected. The bot is written in Python, which means that it is a great way of applying my Python learning to something I am passionate about.

The bot uses computer vision in order to recognise what is on the screen and run scripts accordingly. The monsters in the game drop items, and if you want the bot to pick up items, it will need to be taught which items it needs to pick up.

This is done by adding some images to its image database and adding the filenames to a list of items. When the bot scans the screen for items, it will look for a match in its image database, and when it matches, it will click the corresponding pixels on the screen to pick up the item.

Here’s what an image in the database looks like:

![](/axe.png)

I am doing a Holy Grail project in this game, which means that I am collecting every item in the game. It is quite an undertaking as there are 506 items in the game, and some items have a drop chance of 1 : 1.000.000. Luckily I have a bot to help me with this project.

Not surprisingly, many items were still missing from the bot because it is a fairly new project that is still in development. And as I needed my bot to pick up the items I needed, I decided to add these 46 missing items to the database.

### forks, commits and pull requests

After doing the work I still needed to figure out how I should offer these items to the project. Luckily someone shared a few very helpful tutorials in the project’s discord. This is the tutorial I used for my first contribution:

[Step-by-step Guide to Contributing on Github](https://www.dataschool.io/how-to-contribute-on-github/)

You begin with “forking” the project repository, which basically means making your own copy of all the code in the project. Then you add your contribution to the fork by cloning it to your local machine and making your changes to a new branch.

When you have committed your changes and pushed your new branch to your fork, you are ready to make your first pull request. A pull request is a way of telling the project that you have something to contribute. You are sharing your version of the project repository including your proposed changes, and someone from the project will take a look at your suggestions and see if they are useful and compatible.

### merged into master

After a few days someone had a look at my contribution and requested me to make a few small adjustments. When I managed to incorporate those my contribution was accepted, and my changes were “merged into master”, which means that my contribution was brought into the main version of the project’s code.

![](/merged.png)

### I learned a lot!

As I am typing out this article, I am very satisfied with how much I have learned in the past few months already. I remember being very confused about all the GitHub terminology when I attended my first meetings during my DevOps traineeship.

Going through the process of making a contribution to open source on GitHub has been an enriching experience. It seemed quite intimidating at the start, but by following a good tutorial I managed to successfully submit my first pull request. I feel I have a much better understanding of Git, GitHub and the workflow.

Another valuable lesson I learned is that you don’t need to be a Senior Engineer in order to be able to contribute to open source. Although this project is written in Python, my contribution had very little to do with code, but I provided assets which were required by the code. So if you are a beginner at programming, you can look for other ways to contribute, such as fixing spelling mistakes in the documentation, providing images or writing wiki pages.
