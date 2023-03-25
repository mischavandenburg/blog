---
date: "2022-09-16"
tags:
- DevOps
- Book-Notes
- Article
title: "Book Notes: The Phoenix Project"
---
![Phoenix Project](https://m.media-amazon.com/images/W/IMAGERENDERING_521856-T1/images/I/914-sUgELZL.jpg)

When I started my DevOps traineeship, I borrowed this book from my boss and read it from cover to cover. I loved the story and the characters; it helped me understand "the old way" of doing things and the merits of implementing DevOps principles.

I reread the book ten months later. In the meantime, I've learned many new skills and technologies and started working full-time as a DevOps engineer. 

Not surprisingly, it made a lot more sense to me this time, and I'm sure it will be even better when I reread it a few years later. In this article, I'll share my thoughts and notes on the book. 

## The Story

The main character is Bill Palmer, a mid-level IT manager in a manufacturing company called Parts Unlimited. Within a few pages, he is called into the CEO's office, and he is promoted to the VP of Operations, putting him in charge of IT, much against his own will or desire. 

The situation Bill enters is a humorously chaotic one. We are thrown straight into a Sev 1 incident where managers point fingers and shout at each other. We quickly get the impression that this is a dysfunctional department that only performs tasks for the manager who shouts the loudest while fighting off crippling outages. 

>  It’s like the Wild West out here. We’re mostly shooting from the hip.”  
>  *The Phoenix Project*

The bulk of the story revolves around how Bill, together with his team of managers, Wes, Patty, and John, manage to turn this chaos into a department that does work according to a streamlined plan in a much more predictable manner.

To achieve this goal, Bill is introduced to Erik, a prospective board member of the company. Erik becomes Bill's mentor and guides Bill through the process of creating order in the chaos. Their interaction reminds me of Zen masters training their disciples by asking deep questions which don't have an immediately apparent answer. 

## Master & Disciple

Erik takes Bill to the manufacturing plant of Parts Unlimited and tries to impress upon Bill that manufacturing planning principles from Lean can be applied to IT work. Erik argues that an IT department could be structured like a factory production line, but Bill is not ready to accept this.

A fundamental notion from manufacturing principles is that work should always be moving forwards along the production line, never backward. But unfortunately, this is very often the case in the "old" way of working: the development team works on an application for several months, and when they are finished with it, they throw it over the fence to the Operations people, whose job it is to deploy the application.

>One of the developers had actually walked in a couple of minutes ago and said, “Look, it’s running on my laptop. How hard can it be?”  
>*The Phoenix Project*

However, as we see happening time after time in the book, usually the application is incompatible with the infrastructure it is deployed to. As a result, the application needs to go back to development. According to manufacturing theory, this is a situation where work goes backward through the production line, which we must avoid at all costs. 

## Implementation

Erik challenges Bill to start doing ten deployments a day instead of one deployment every nine months. Understandably, this is a ridiculous notion to Bill. The last few deployments were disastrous events that required his entire department to pull all-nighters through the weekend, and still, the stores were not managing to process all orders and payments. 

However, Bill takes his mentor's advice and figures out a way to do it together with his team. One of the main problems they uncovered was the inconsistent deployment and production environments.

The solution to this problem was to involve the operations people in the development stage right from the beginning, so the development environment matched the production environment exactly. The environments were standardized and put in code with version control, and things started progressing quickly. 

>As Wes talks, I think about Erik challenging me to think like a plant manager as opposed to a work center supervisor. I suddenly realize that he probably meant that I needed to span the departmental boundaries of Development and IT Operations.
>*The Phoenix Project*

This is just one of the problems addressed by melting away the fence between Development and Operations. By the end of the book, the two camps started to work together much better. They come closer to the target of 10 deployments a day, and the DevOps way of working was born. 

## Closing Thoughts

I think this book is a must-read for anyone considering entering the DevOps field or anyone already working with DevOps. 

As a nerd who loves structure and organization, the theme of the story is incredibly entertaining and satisfying to me. The authors excellently capture the transition from an utterly disorganized situation to a predictable environment with happy co-workers. Actually, I'm a little embarrassed by how much joy this transition brings me. 

Especially the second time around, it helped me better understand the underlying principles that enable the DevOps way of working in an organization. Moreover, it paints a great picture of how an organization can change for the better by embracing DevOps principles and how these changes express themselves in the improved quality and speed of software development and deployment. All of these advantages lead to delivering better value to the customer, which is the core focus of any productive and creative endeavor involving customers and end users. 

## The Phoenix Project, written by Gene Kim, Kevin Behr, George Spafford
