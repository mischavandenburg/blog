---
date: "2022-03-16T21:20:19Z"
image: /wp-content/uploads/2022/03/fallback-image-for-social-media_bw.jpg
tags:
  - Linux
  - Study
  - Article
title: LPIC-1 Study Guide
url: /lpic-1-study-guide/
---

I recently obtained my LPIC-1 certification, and in this blog post, I’ll share the strategy and techniques I used to pass this exam and share my thoughts on the certification. Because I am a Linux novice, the exam was a pretty tough grind for me. This article offers a beginner’s perspective on the LPIC-1 certification. Is the LPIC-1 hard to pass? Keep reading to find out.

Before this certification, I had only a little bit of experience. I deployed LAMP stacks using Ansible and configured VMs to be able to communicate with each other using only the command line. I also did a “Linux Fundamentals” video training. I could navigate the filesystem, edit text files and work in the terminal, but that was about it.

![](/hard.jpg)

### Is it hard?

For a beginner: yes, it was hard! But if you are a Linux administrator with a few years of experience, these exams probably are not very difficult to pass. However, even if you are experienced, be prepared to do a lot of memorization. Even though the requirements on the website seem very basic and straightforward, when you dig into the study resources, you will soon discover that you need to learn a large host of commands and many of their accompanying parameters. For example, you will need to know what grep -H does precisely, the difference between passwd -l, chage -l, and chage -L, the location of the directory that contains all the timezones, and the directory that contains the printer configurations for CUPS.

### The certification

The LPIC-1 certificate requires candidates to pass the LPI 101-500 and 102-500 exams. These exams test the candidates on various subjects, such as file management, boot loaders, networking fundamentals, user and group management, file systems and partitioning, and much more.

Each exam has a 600-page syllabus, so to get your LPIC-1 certification, you need to work through 1200 pages and memorize a few hundred commands and parameters. However, if you work as a Linux sysadmin, you’ll probably know many of these commands and concepts.

### Study Materials

I attended a 4-day course that covered both exams. However, because of the large amount of information that needs to be covered, the teacher could only address the subjects on a superficial level. Therefore, I would advise you to be suspicious of any courses that promise to prep you for the exams in 4 days if you are a beginner. I estimate that you need at least double that amount to get some proper explanation of the material.

#### LPI Syllabus

After finishing the course, it became clear that I needed a lot of studying to pass the exams. Fortunately, LPI has created a syllabus for each exam. These are available for free on the [LPI.org website.](https://www.lpi.org/our-certifications/lpic-1-overview)

#### LPI Practice Exams

It is crucial to test your knowledge. This is the resource I used:

[LPIC-1 Linux Professional Institute Certification Practice Tests: Exam 101-500 and Exam 102-500 ](https://amzn.to/3KHAkCZ)

If you are a member of O’Reilly’s, you can read the book there. It contains around 90 practice questions for every chapter in the LPI syllabus. The questions test your knowledge in detail and are a great way to determine whether you have fully grasped the material.

However, the book was written in 2019 and contains questions about certain subjects that have since been removed from the exams. So if you suddenly encounter questions that do not seem familiar at all, make sure to double-check that it is actually an exam objective.

Lastly, I used [these practice exams on Udemy](https://www.udemy.com/share/1029gO3@XonS1Jh2QkFmIV_mN-r8Rbx04vCYiyykPhTpewu5iLZQVNIYMVe4z53YFSxp2tly/).

### Memorization

As I have stated before, the exams require you to do a lot of memorization. Fortunately, we have some tools and techniques available to help us with this task.

The primary tool is Anki. If you are not familiar with it, Anki is a very simple and free program that allows you to create flashcards that you can use to study and test your knowledge. The best thing about Anki is that it implements spaced repetition. You can download and learn more about Anki here: [https://apps.ankiweb.net](https://apps.ankiweb.net/)

Secondly, I am fond of memory techniques. You can remember things much more quickly by visualizing them in your mind or utilizing techniques such as Memory Palaces or the Method of Loci. If you are interested in learning more about memory techniques, I highly recommend Dr. Anthony Metivier’s [YouTube channel.](https://www.youtube.com/c/AnthonyMetivierMMM/featured)

![](/palace.jpg)

### Strategy

Here is the strategy that I used to pass the exams:

- Read through a chapter and take notes.
- Make Anki flashcards for all the commands and flags that you do not know yet
- Do the exercises at the end of the chapter
- Do the practice exam for your chapter from the exam book, which should give you a good indication of how well you have grasped the material.
- Make flashcards of all the questions that you answered wrong (trust me, there will be quite a few)
- Use Anki to test yourself and memorize all of the commands and exam questions

### Tips:

- Do your Anki reviews every day. On some days I was adding more than 100 new cards, which will lead to a lot of reviews in the coming days
- Although the syllabus for exam 101 explained things very well, the 102 syllabus sometimes is very meager in its explanations and you might need to supplement with reading man pages, YouTube videos, and other tutorials. For example, I needed to find quite a bit of supplementary material for chapter 109 Networking Fundamentals.
- Ask for help if you don’t understand a certain topic
- Don’t think you can get away with skipping a topic. You will be tested on absolutely everything that’s in the syllabus, trust me.
- Try doing it together with someone else. I was doing it together with my friend and colleague, and it was extremely useful to be able to share things I struggled with and to discuss things with him to understand them better. Thank you for the good times, Gino!

### My thoughts on the certification

The subject matter is extensive, and I know my way around Linux much better now. Therefore, if I encounter a problem, I am better positioned to assess where the cause might be and then solve the problem from there. I also feel I have a much better grasp of basic networking concepts, which will prove to be very useful in many situations in my work as a DevOps Engineer.

However, there are also a few drawbacks to this certification. I think there is too much emphasis on memorizing commands and their flags. I think it is not necessary to memorize all of the possible parameters of the chage command because, in the real world, I would take a quick look at the man page to find the parameter that I need. The exams force you to memorize many parameters in a short time, and to be honest, you will probably forget about them very quickly anyway.

But overall, I am pleased and grateful to my employer that I was able to obtain this certification, and it has made me hungry for more, and I am very eager to continue my learning in this domain.
