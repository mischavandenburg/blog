---
title: Adding Articles to Wallabag From Qutebrowser
date: 2024-09-02
tags:
- Command-line
- Qutebrowser
---

I've been daily driving Qutebrowser for over a month now, and I'm starting to feel at home in it.

Since it doesn't have any extensions, you have to hack things together yourself, which is why I like it so much.

Today I wanted to add an article to my Wallabag instance, which I usually did by using the browser extension.

I installed this Wallabag CLI:

<https://github.com/artur-shaik/wallabag-client>

`pipx install wallabag-client`

After setting up the config I can now add entries from the command line. By running `wallabag add [url]`. Neat!

Next, we can use the `spawn` command from within Qutebrowser to call this CLI command:

`spawn -v -m wallabag add {url}`

## Alias

And to make it easier, I added a `:wallabag` alias by adding this to my `config.py`:

```python
c.aliases = {
    'w': 'session-save', 
    'q': 'close', 
    'qa': 'quit', 
    'wq': 'quit --save', 
    'wqa': 'quit --save',
    'wallabag': 'spawn -v -m wallabag add {url}'
}
```

Now I can just run `:wallabag` from within Qutebrowser and it will add the article.

## Build It Yourself

I'm really starting to love Qutebrowser for much of the same reasons why I love (neo)vim so much. I can call CLI commands from within my editor (or browser) and interact with my system that way.

Since you can do almost anything from the CLI, you can build some very powerful workflows this way.

Next, I'm going to do the same for my Linkding instance so I can add bookmarks from within Qutebrowser.

Feels nice that I don't need any extensions for this!

## Links

<https://www.reddit.com/r/qutebrowser/comments/fgc0ol/is_there_a_way_to_make_this_wallabag_bookmarklet/>

202409020809
