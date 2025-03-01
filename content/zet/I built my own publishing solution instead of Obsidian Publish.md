---
publish: true
hugo_publish: true
---

# I built my own publishing solution instead of Obsidian Publish

Instead of relying on Obsidian publish, I wrote some Python code to parse my Zettelkasten for the "published: true" property.

It keeps track of files and hashes in a Postgres database, and when it finds a new file, it will push it to a webhook in make, which will publish it to X through Buffer.

Pretty neat setup, I'm now going to add a module so it will also publish it to my old blog.

I haven't been posting on my blog for ages, but I think that it will help if I can just easily publish from my Zettelkasten by simply adding some YAML frontmatter. It won't matter if I'm in Obsidian or (neo)vim.



