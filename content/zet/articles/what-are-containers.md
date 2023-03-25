---
title: "What Are Containers?"
date: 2023-01-01T16:17:58+01:00
tags:
- Explanations
- Linux
- DevOps
- Article
---

When you learn about DevOps, you will come across the concept of a container early on. This is a "Mischa Explains" article where I attempt to explain a concept in my own words as simply as possible. I use the Feynman technique and pretend to explain it to a 12-year-old. 

# Virtualization

To understand containers, we need to understand virtualization. Virtualization is the process of creating "fake computers" or "virtual computers" on a physical computer.

On your desk, you have a laptop or a desktop PC. This machine has hardware such as a motherboard, a hard disk, and a video card. To run programs on your computer, it needs an Operating System. Usually, Windows, macOS, or Linux are used.

Let's say you have a computer running Windows, but you want to run a program that can only run on Linux. One solution is to buy another laptop and put it beside your Windows laptop on your desk. So now you have two computers with two different operating systems.

Fortunately, there are other solutions. We can use virtualization to make a Virtual Machine. A virtual machine is created by software to imitate a fully functional running computer inside your current operating system. You can create a virtual machine that runs Linux on your Windows computer. Your Windows computer running the Linux virtual machine is known as the **host.

Now you don't need to buy another computer to run your Linux program. Instead, you can boot up your Linux virtual machine and run your program when needed. If you have a powerful computer, you could run ten or more virtual machines, each of which has its own operating system and custom environment. 

# Containers

Every time you create a virtual machine, the virtual machine needs a complete operating system to work. So, first, the software creates a virtual processor, virtual video card, and a virtual network interface. Then, it runs a fully functional operating system on that virtual hardware. This takes up a lot of resources. 

Containers are lightweight packages of software. They are designed to do a very specific task, and therefore they only contain the resources they need to do that task. Nothing more. 

Containers use the operating system of the physical computer to run. They have a very minimal, lightweight operating system inside them, but it only contains the elements they need to do their specific task. Therefore, containers are very easy to distribute, and you can run them very quickly. 

# Containers are like newspapers

Containers are like newspapers. Newspapers have a particular task: providing you with the day's news. You cannot use newspapers to study for your mathematics exam. You use your math book to study for your math exam. If you want to be informed of the day's news, you use a newspaper. This is what I mean by containers having a specific task. 

Next, newspapers are printed on a specific kind of paper. When you buy an expensive book, it will have a sturdy and durable cover, and the pages are made of nice thick paper that will last a long time. The pages don't tear very quickly, and when the book gets wet, it can withstand it. This thick cover and high-quality papers are like the operating system of a virtual machine.

Newspapers, on the other hand, are printed on very thin paper. Because they are designed to distribute the news to you effectively, newspapers do not need to be stored forever or do any other tasks. If you used thick, expensive paper for newspapers, they would become costly, and no one would buy them anymore. The paper is optimized to bring the news to you.

In the same way, the container only comes with the components it needs to do its specific task. Therefore, the container is optimized for its purpose. As a result, they can be distributed more quickly and do not take up a lot of resources when running.

There are other benefits to containers, such as improving the ability to autoscale your application, but I will expand on those in a future blog post. 

# Further study

To learn more about containers, you can use the following resources:

[Containers & Friends from John Savill's DevOps Masterclass](https://youtu.be/r6YIlPEC4y4)

[Docker Documentation](https://docs.docker.com/get-started/overview/)

[Docker Tutorial for Beginners](https://youtu.be/3c-iBn73dDE)
