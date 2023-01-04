---
title: "You Don't Need to pay for Readwise! I wrote my own Readwise clone in Python."
date: 2023-01-01T16:48:00+01:00
draft: True
---

# Introduction

I've always been an avid reader. Even though there is nothing like the smell and feel of a real book, I transitioned to reading mostly on a Kindle 8 years ago. Two reasons: I can look up words in the dictionary just by tapping them, and I can make highlights which I can export and use elsewhere. 

However, even though I've accumulated a massive collection of highlights and interesting quotes from my reading, I rarely interact with them. This is where a service like Readwise comes in. This syncs with your Amazon account and sends you an email with a quote from your highlights every day. 

You might be surprised at how easy it is to create your own solution using Python. In this article, I'll explain how I created a Python script that does just that, and show you how you can use it to get daily inspiration from your own Kindle highlights for free. Scroll to the bottom to see the full script.

## How it works

The script that I created is fairly simple and has the following key components:

1. A library called frontmatter that is used to parse the metadata (e.g., author and title) of my Kindle highlights files.
2. A function called get_author that takes an input file and returns the author of that file.
3. A function called get_title that takes an input file and returns the title of that file.
4. A list of files in my Kindle highlights directory, which is generated using the Path library.
5. A for loop that iterates over the files in the list and selects a random file.
6. A for loop that iterates over the lines in the chosen file and adds any lines that begin with a quotation mark to a list of quotes.
7. A randomly chosen quote from the list of quotes, which is then modified to remove the quotation marks.
8. The author and title of the chosen file, which are generated using the get_author and get_title functions.
9. An email server that is used to send the quote and its corresponding author and title to my email address.

## Obsidian Setup

I use the [Obsidian Kindle Highlights plugin](https://github.com/hadynz/obsidian-kindle-plugin) to import my kindle highlights from the My Clippings.txt file.

This is how I have my highlights pages set up:

![](/kindle-highlights.png)


And I adjusted my kindle highlights template, so it always surrounds the quote with quotation marks ". This makes it easier to filter out the quotes themselves:

![](/kindle-template.png)

Now that we have an understanding of the components of the script, let's walk through each step in more detail.

# Step 1: Parsing metadata with frontmatter

The frontmatter library is used to parse the metadata (e.g., author and title) of my Kindle highlights files. To use it, I first need to import it at the top of my script:

```python
import frontmatter
```

Then, I create two functions called get_author and get_title that take an input file and return the author and title of that file, respectively. These functions use frontmatter.load to parse the metadata of the input file, and then return the values associated with the kindle-sync keys author and title. If either of these keys is not found, the functions return "Unknown author" and "Unknown title", respectively.

```python
def get_author(input_file):
    post = frontmatter.load(input_file)
    try:
        return post['kindle-sync']['author']
    except Exception:
        return "Unknown author"

def get_title(input_file):
    post = frontmatter.load(input_file)
    try:
        return post['kindle-sync']['title']
    except Exception:
        return "Unknown title"
```
# Step 2: Generating a list of files

Next, I generate a list of all the files in my Kindle highlights directory using the Path library. To do this, I first import the Path library at the top of my script:

```python
from pathlib import Path
```

Then, I use the Path class to specify the path to my Kindle highlights directory and assign the resulting Path object to a variable called path:

```python
path = Path('/home/mischa/obsidian/second-brain/kindle-highlights')
```

Next, I create an empty list called file_list that will eventually contain all the files in my Kindle highlights directory. Then, I use a for loop to iterate over the files in path and append them to file_list if they are files (i.e., not directories):

```python
file_list = []

for file in path.iterdir():
 if file.is_file():
        file_list.append(file)
```

Next, I need to select a random file from file_list. This is done using the random.choice function from the random library:

```python
chosen_file = random.choice(file_list)
```

# Step 4: Extracting quotes from the chosen file

Now that we have a random file, we need to extract all the quotes from it. To do this, I create an empty list called quote_list and use a with statement to open the chosen file for reading. Then, I iterate over the lines in the file and use an if statement to check if each line starts with a quotation mark ("). If it does, I append the line to quote_list.

```python
quote_list = []
with open(chosen_file, "r") as f:
    for line in f:
        if line.startswith("\""):
            quote_list.append(line)
```

# Step 5: Choosing a random quote

Now that we have a list of quotes, we need to choose a random one. This is done using the random.choice function again:

```python
chosen_quote = random.choice(quote_list).replace("\"", '')
```

Note that I use the replace method to remove the quotation marks from the beginning and end of the quote.

# Step 6: Preparing the email message

Next, I use the get_author and get_title functions to get the author and title of the chosen file and use string formatting to create a string called author_title that combines them.

Finally, I create the message that will be sent via email. This consists of a subject line, the chosen quote, the author and title of the book from which the quote was taken, and a closing line.

```python
message = f"""\
From: {from_email}
Subject: Your Daily Quote
{chosen_quote}
{author_title}

With love from Python.
"""
```

# Step 7: Sending the email

To send the email, I set up a mail server using the smtplib library. I use the SMTP_SSL method to connect to the server over a secure connection and the login method to log in with my email and password. Finally, I use the sendmail method to send the email.

```python
port = 465
from_email = "random_email@random.com"
to_email = "your_destination_email@gmail.com"
context = ssl.create_default_context()

with smtplib.SMTP_SSL("enter_email_domain.com", port, context=context) as server:
    server.login(from_email, mail_password)
    server.sendmail(from_email, to_email, message.encode('utf-8'))
```

# Conclusion

That's it! This script runs every day in a cronjob on my Linux machine, and sends me a random quote from my Kindle highlights. If you have your own Kindle highlights stored in an Obsidian vault, you can use this script as a starting point to create your own daily quote email. There is no need to pay for services like Readwise. You can use this script to get similar functionality for free.

# Full Script

Note: I use a secrets.py file that contains a variable with mail_password.

```python
from pathlib import Path
import frontmatter
import random
import smtplib
import ssl
from secrets import mail_password
path = Path('/home/mischa/obsidian/second-brain/kindle-highlights')


def get_author(input_file):
    post = frontmatter.load(input_file)
    try:
        return post['kindle-sync']['author']
    except Exception:
        return "Unknown author"


def get_title(input_file):
    post = frontmatter.load(input_file)
    try:
        return post['kindle-sync']['title']
    except Exception:
        return "Unknown title"


# get all the files and make a list
file_list = []

for file in path.iterdir():
    if file.is_file():
        file_list.append(file)

# choose a random file
chosen_file = random.choice(file_list)

# get all quotes from file, quotes start with "
quote_list = []
with open(chosen_file, "r") as f:
    for line in f:
        if line.startswith("\""):
            quote_list.append(line)

# prepare data to be sent
chosen_quote = random.choice(quote_list).replace("\"", '')
author_title = f"{get_author(chosen_file)}, {get_title(chosen_file)}"

# set up mail server
port = 465
from_email = "your_from_email@email.com"
to_email = "your_chosen_email@gmail.com"
context = ssl.create_default_context()
message = f"""\
From: {from_email}
Subject: Your Daily Quote
{chosen_quote}
{author_title}

With love from Python.
"""

with smtplib.SMTP_SSL("your_email_host.com", port, context=context) as server:
    server.login(from_email, mail_password)
    server.sendmail(from_email, to_email, message.encode('utf-8'))
```
