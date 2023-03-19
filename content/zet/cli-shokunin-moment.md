---
title: Had a CLI Shokunin Moment Today
date: 2023-03-19
tags:
- Zettelkasten
- Linux
---

Today was meal-prepping day and I cut up some vegetables for the coming week. I’m on a strict caloric restriction regimen and need to meticulously track and plan all the food that I consume.


I cut up three kinds of vegetables and wrote down how many grams of each I cut up so I could divide them by three and add them to my calorie tracking application.


As I took out my phone to pick up my calculator to divide each number, my inner engineer started complaining about the fact that I had to do three calculations and that it would be much better to loop over an array of these values.

I had my laptop nearby with a terminal open and wrote this instead:

```bash
for i in 287 252 321; do echo "$i / 3" | bc; done

95
84
107
```

After turning my passion into my career, I love the fact that I’m starting to think like an engineer in all my other areas of life. I’m also happy to see my progress in command line work. I chose to go the hard way, using bash instead of zsh and doing all my editing in vim and using Linux as much as possible, and it is paying off because I don’t have to think much about these little operations anymore.


Then again, I would be much better off training my brain to become better at doing math without the aid of paper or calculators 🤓 

If you're curious about what a shokunin is, check out this article I wrote: https://mischavandenburg.com/zet/articles/jiro-sushi/
