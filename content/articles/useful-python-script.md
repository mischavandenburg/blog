---
date: "2022-02-05T05:49:06Z"
tags:
  - Coding
title: My Mirst Useful Python Script
url: /my-first-useful-python-script/
---

The best part of learning Python is trying to identify things in my life which I can automate by writing a script. Learning a programming language involves doing a lot of exercises that sometimes lack a connection with the real world. But after I decided to go for it, I am always on the lookout for projects. Not only for my job as a DevOps Engineer, but also for my private life. In this case, I needed to write a program that parses log files from a bot so I could get a total number of runs. You can have a look at the final result in my Diablo 2 [GitHub repo. ](https://github.com/mischavandenburg/diablo2)

Like I wrote in my [journey into DevOps article](https://mischavandenburg.com/my-journey-into-devops-so-far/), I love automating games. Diablo 2 is a game that was originally released in 2002 and which recently was remastered. Diablo 2 always had a very strong presence of bots in the online game, and it didn’t take long before I also joined the ride.

A few months after the remaster the first bots have started emerging as well. There is a a particularly good one written in Python which is an [open source project](https://github.com/aeon0/botty), which is a perfect opportunity for me to learn more about Python by trying to understand its code and solving problems. I was very excited to discover it because I was playing quite a few hours a week. Diablo 2 is a very grindy game and it takes a lot of time to find the needed gear. Now I could finally outsource my grinding to the computer again.

## the problem

Although the bot is very functional and does several tasks very well, there are still features missing because it is relatively early in its development. One of these features is keeping track of the total amount of runs that the bot has done. In Diablo, every time you play you start a “game” or an instance. In that game there are certain bosses you can kill, and when you are finished you exit your game. This is called a run. Then you create a new game and everything is reset, and you get another shot at killing the bosses to get the precious gear.

Being the nerd that I am, I like to keep track of the total amount of runs that the bot has done. On these numbers I like to apply some calculations to see how many items I get per xxxx runs and suchlike. The bot keeps track of the amount of runs it does per session and stores them in a log file. But there is no functionality of seeing the total amount of runs you have done, and when I discovered this, I realised I had my first little Python project.

## log files

Every time you close the bot after a session, a log file is created that looks like this:


![](/logfile.png)

It is formatted as a .txt file and shows information about the bosses that were run and the items that were found. Most importantly, it contains the amount of games that were done in the session. Even after only using the bot for a short time I had over 100 log files to go through, and that’s where I needed a script that would go through these files for me and add the numbers in order to get the total amount of runs.

## the script

After completing chapter 9 and 10 in the Automate the Boring Stuff book, I learned about file paths and opening files and reading from them. Now it was time to apply that knowledge. The process went pretty well and soon I had a script that would open the files for me.

Things got a little bit more complicated when I needed to read information from the files. And of course, this operation would almost certainly involve the dreaded topic of regex. In the end it was not as bad, and I ended up with the following regex:

```python

regex_games = re.compile(r"Games:")
regex_dict = {
'nihla': 'Nihl|Nihlatak',
'pindle': 'Pin|Pindle',
'eldritch': 'Eld'
}

```

As you will see afterwards, I needed a way to check every line for a certain statement. However, rather than hardcoding every operation, I wanted it to loop over a list of terms. This meant that I could easily go back to the code and add a few more search statements if I needed them. I ended up storing them in a dictionary as you can see above. I really like the way you can make dictionaries in Python and have every entry on a new line. It makes the code very readable and structured.

And this is the actual looping sequence that I ended up with:

```python
for folder_name, sub_folder, file_names in os.walk(source):
    for filename in file_names:
        p = PurePath(folder_name, filename)
        with open(p, 'rt') as my_file:
            # search for games number line
            for line in my_file:
                # find number of games and add to total games
                if regex_games.search(line):
                    g = line.split()
                    total_games += int(g[1])
                    f = my_file.read()

                    # check which runs were done by using the regex dict
                    for key in regex_dict:
                        location = regex_dict[key]
                        reg = re.compile(location)
                        # if there is a match, add the numbers to the total variable
                        if reg.search(f):
                            var_name = key + '_total'
                            globals()[var_name] += int(g[1])
```

This sequence loops through the folder, the subfolder, and opens each file one by one. When the file is opened it looks for the “Games: 25” line and adds the number to a variable. However, I was not only interested in the total number of games. I also wanted to get more insight in how many Pindle runs or Nihla runs I had done. So I set up another regex search and made sure that the number of games are added to a “pindle\_total” or “nihla\_total” variable.

## result

When running the script in the shell, the result looks like this:

```bash
mischa@MischaMacBook stats_parser % python3 total_runs.py 
Total runs: 7159
Pindle runs: 6926
Eldritch + Shenk runs: 367
Nihla runs: 232
mischa@MischaMacBook stats_parser % 
```

Exactly what I wanted. Now I can just paste my stats files into a folder and see how many runs I’ve done. Maybe I’ll improve it by building a GUI. Another fun idea I have is to create a little pipeline where this script would be run once an hour and the stats would be uploaded to a webpage somewhere, so others could see the amount of runs of my bot. Not that anyone is interested in that, but it is a fun project for me to do. Let’s see what happens!

For now I am very happy with the result. It was a very satisfying experience to identify a problem that I had and to be able to come up with an automated solution. Of course it is still very rudimentary programming, and there is a long long way ahead of me, but it was fun to finally do something practical that solved a particular problem in my life.

The final result is in my [Diablo 2 GitHub repo.](https://github.com/mischavandenburg/diablo2)
