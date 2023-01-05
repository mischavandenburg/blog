---
title: Fall in love with sed. 
date: 2023-01-04
tags:
- Linux
- Productivity
- Zettelkasten
---

Sed, it's so powerful. I remember I struggled with finding practical uses for it when I did my LPIC-1 certification. But now I find myself using it several times a week. It is so powerful to edit multiple files at a time. I use it for work, but also for making to my entire second brain in Obsidian with one command. 

Today I needed to update my /articles/ links to /zet/articles/ links because I'm restructuring my website. Here is the sed expression that is executed for every markdown file that is found by fd:

```bash
sed -i 's/\/articles\//\/zet\/articles\//g' $(fd .md)
```

The result:
```bash
diff --git a/content/zet/move-to-zet.md b/content/zet/move-to-zet.md
index 1e37283..3b817e3 100644
--- a/content/zet/move-to-zet.md
+++ b/content/zet/move-to-zet.md
@@ -6,7 +6,7 @@ tags:
 - Zettelkasten
 ---

-I've transitioned my note taking system towards a Zettelkasten system. I still use directories for folders and make copious links, but more
often than not I put them in the larger generic 00-zettelkasten directory in my [Obsidian](/articles/obsidian-introduction/) vault.
+I've transitioned my note taking system towards a Zettelkasten system. I still use directories for folders and make copious links, but more
often than not I put them in the larger generic 00-zettelkasten directory in my [Obsidian](/zet/articles/obsidian-introduction/) vault.


```

These sites are super useful to help you formulate your expressions:

https://sed.js.org/

https://regex101.com/
