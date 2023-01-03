---
date: "2022-02-01T21:41:28Z"
guid: https://mischavandenburg.com/?p=79
id: 79
tags:
  - Coding
title: 'Python Project: Mad Libs'
url: /python-project-mad-libs/
---

I am currently working through the book [Automate the Boring Stuff](https://automatetheboringstuff.com) by Al Sweigart . I can already highly recommend it to anybody who is learning Python.

Chapter 9 is about reading and writing files, and there are two assignments at the end of the chapter. Here I’ll discuss my solution of the Mad Libs assignment.

### here is the full assignment text:

```
Mad Libs
Create a Mad Libs program that reads in text files and lets the user add their own text anywhere the word ADJECTIVE, NOUN, ADVERB, or VERB appears in the text file. For example, a text file may look like this:
The ADJECTIVE panda walked to the NOUN and then VERB. A nearby NOUN was
unaffected by these events.

The program would find these occurrences and prompt the user to replace them.
Enter an adjective:
silly
Enter a noun:
chandelier
Enter a verb:
screamed
Enter a noun:
pickup truck

The following text file would then be created:
The silly panda walked to the chandelier and then screamed. A nearby pickup
truck was unaffected by these events.

The results should be printed to the screen and saved to a new text file.
```

Looks pretty simple, right? I went into it with a lot of zeal and started writing a long list of if statements. My first attempts at the solution involved matching the words NOUN and ADJECTIVE directly, like so:

```python
if word == 'ADJECTIVE':
        inv = input('Enter an adjective: ')
```

However, this is problematic because as you can see, the sentence can contain words with a period attached, such as “VERB.” in the above example.

## no, please no regex!

I’ve understood that here is a general anxiety around regex. I have certainly noticed it in myself and some of my junior engineer friends. As soon as I read the word regex, or realise that an assignment is going to involve regex, I get a constricting feeling in my throat and a rise in my heart rate.

I’ve had to struggle with it quite a bit during my freeCodeCamp Front End Development Certificate, and the memories are still fresh in my mind.

So, like any other ‘rational’ human being, I tried to approach this relatively simple assignment with all sorts of ways trying to account for a period ending the word:

```python
def period_check(x):
    for letter in x:
        if letter == ".":
            return True

# loop over the array and prompt user

for word in source_text:

    period = False

    if period_check(word):
        word = word.replace(".", "")
        period = True

    if word == 'ADJECTIVE':
        invoer = input('Enter an adjective: ')
        if period:
            result_text.append(invoer + ".")
            period = True
        else:
            result_text.append(invoer)
    elif word == 'NOUN':
```

It’s quite funny to see the lengths we go through to avoid regular expressions. However, as you maybe deduce from the code above, it didn’t work, and after a couple of hours of fiddling I gave up, and like any other frustrated programmer, I started to google.

I quickly found solutions to this assignment and they all involved regex, and I realised I could not walk away from my fears anymore.

## an elegant solution

Eventually I ended up with the following result for the part of my assignment that needed to recognise and replace the words with the user input. Of course I heavily borrowed from my Google search results.

```python
# set up and match the regex
grammar_regex = re.compile(r'ADJECTIVE|NOUN|VERB|ADVERB')
match_regex = grammar_regex.findall(source_text)

# replace the matches with user input 
for i in match_regex:
    ask_user = input('Please enter ' + i + ': ')
    source_text = source_text.replace(i, ask_user, 1)
```

When I say elegant, I mean elegant in total Python beginner terms. I’m sure there are enough Seniors out there who will burst out laughing when they see this. But to me, it was quite an eye-opening experience to see this little piece of code do exactly what I had intended to achieve with 3 different functions and long blocks of if statements.

Also, I was pleasantly surprised with how simple regex can be in Python. In this case there were no scary \[Az ^\*\*/!!${}aa{}aA{nF}\] statements. We simply defined which words we wanted and called the findall() module to generate a list with all the matches.

Then we iterate over the list of matches and for each match we ask the user for the desired word, and replace it in the source\_text.

## final result

Having sorted out the pattern matching and replacing part, it was only a matter of implementing reading from files and writing to a new file.

```python
# Automate the Boring Stuff chapter 9
# Mad Libs assignment
# Mischa van den Burg

from pathlib import Path
import re

# ask the user which file to open
file_name = input('Enter the filename. For example, grammar.txt: ')

# my script and .txt file are located in ~/python/automatetheboringstuff/ 
text_file = open(Path.home() / 'python' / 'automatetheboringstuff' / file_name )

# read the file and store in variable & close
source_text = text_file.read()
text_file.close()

# set up and match the regex
grammar_regex = re.compile(r'ADJECTIVE|NOUN|VERB|ADVERB')
match_regex = grammar_regex.findall(source_text)

# replace the matches with user input 
for i in match_regex:
    ask_user = input('Enter ' + i + ': ')
    source_text = source_text.replace(i, ask_user, 1)

# write to the new file and print the result
new_file = open('new_' + file_name, 'w')
new_file.write(source_text)
new_file.close()

print(source_text)

```

I was getting into some better functionality, such as accounting for existing filenames, and making the pathing relative so it could be run from anywhere. But I decided to save that for a later assignment.

The assignment was clear and did not require such functionality. I need to learn to keep things simple, and I decided to do just what I was asked and not go into any other rabbit holes.

# Lessons Learned

All in all the assignment is pretty simple, but I learned surprisingly much from it. I decided I’ll need to change and learn to love regex rather than fear it, because it showed me how powerful it can be.

Also, I got some insight into my own mind and how I tend to work. I realised I have a tendency to make things much more complicated than they need to be. I need to learn to keep things simple.
