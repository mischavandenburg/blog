---
date: "2022-10-11"
tags:
- Linux
- DevOps
- Book-Notes
title: "Book Notes: The Unicorn Project" 
---
![Unicorn Project](https://m.media-amazon.com/images/W/IMAGERENDERING_521856-T1/images/I/91UM5i4nirL.jpg)

This book is the sequel to [the Phoenix project](/articles/phoenix-project). Both books are set at Parts Unlimited, a fictitious company that supplies car parts to DIY mechanics and repair shops. Phoenix is a new system that Parts Unlimited has worked on for multiple years. It is supposed to handle order processing and communication between manufacturing, stores, and clients. Phoenix will also play a role in sales and marketing. The company has been gathering customer data for years, but it cannot use any of this data yet. Phoenix will enable it to generate targeted marketing campaigns from the data when it's finished. 

But as we saw in the previous book, it is far from finished, and things go wrong all the time. The company is not doing well, the stock prices are falling, and they need an edge over the competition. Phoenix will be their edge, but they've been working on it for years. Eventually, management decided that Phoenix needed to be deployed in two weeks. But it is far from ready. 

The main character in the Unicorn project is Maxine, a senior developer who temporarily transferred to a different department. She had to work on the Phoenix project against her will because of an unfortunate situation that needed a scapegoat. However, Maxine decides to make the best out of it, and she'd like to begin with one thing: to get a Phoenix build going on her laptop.

Very quickly, she finds it impossible to run a full build of the Phoenix project due to missing files and other elements. She is appalled and makes it her mission to get the build going, but she meets another hurdle every step of the way. Missing credentials. Missing binaries and libraries. And for each of these hurdles, she must submit a ticket with a different department. Very soon, she has over 20 tickets running with long waiting times. Just to get a build going on her machine so she can work! Dozens of developers were hired to work on the Phoenix project. But when she asks them if they've managed to get a build going yet, Maxine is horrified to discover that they've tried for several months but haven't made any progress. Maxine has made more progress in a week.

> ---
> "Everyone around here thinks features are important because they can see them in their app, on the web page, or in the API. But no one seems to realize how important the build process is. Developers cannot be productive without a great build, integration, and test process."
>- Unicorn Project
> ---

After a few weeks, Maxine receives an invitation to have a drink with a group of people who are very interested in her. When she arrives at the bar, she meets the Rebellion: a group of developers, managers, and people from Operations, who are tired of the old organizational structure and want to make real changes. They think out of the box and experiment with new technologies, even though they are not authorized to do so. 

With the Rebellion, Maxine significantly improves the build and deployment process. They recognized that Phoenix actually never was being built in its entirety. Developers were always working on parts of the application. However, after a lot of struggle, they create a build process that enables each developer to become operational on his first day. 

This is the first step of a long series of exciting events that lead to Phoenix becoming a success. By the end of the book, they have a completely new development and testing process, and they can deploy changes to production without needing to take the entire application down. This allows them to create targeted marketing campaigns and respond to changes in the market. The first campaign was a huge success and generated the highest sales ever.

Maxine's struggle with the build process was an eye-opening experience for me. It gave me a very practical example of the need for DevOps principles to enable delivering value to customers. It is also something I recognize in my current organization. For example, projects can get stuck on a firewall change that needs to be approved by an external party. By implementing DevOps principles and arranging teams according to the "you build it, you run it" principle, teams can be responsible for the entire process from idea to production and therefore have a very short release cycle for their application. 

I thoroughly enjoyed the first part of the book. However, the second part was less engaging to me. It became long-winded and felt like butter spread over too much bread. The author demonstrates a high level of technical experience and knowledge through his descriptions of processes, deployments, and fictional applications. Although I understand the intention of making Parts Unlimited a believable company, I think it could have been accomplished with much less detail and words.

The second part has more corporate drama, such as temporarily suspended managers without any clear reason. The focus shifts from a development and operations perspective to a managerial perspective. Maybe I will reread the book in a few years and this part will make a lot more sense to me then. The same happened when I reread the Phoenix project. I could not understand some aspects of the book, which became much clearer to me when I revisited it after gaining experience in the field. 

I highly recommend this book to anyone working as a developer, DevOps Engineer, or in operations, especially if you are starting your career. The book gave me a lot of insights into "the old way of working" and a better understanding of the need for DevOps principles in the modern IT landscape. However, make sure to read the Phoenix project first.

## The Unicorn Project: A Novel about Developers, Digital Disruption, and Thriving in the Age of Data by Gene Kim
