---
title: "Call Stacks"
date: 2023-01-02T21:11:26+01:00
tags:

- Coding
---

When you call a function, the system sets aside space in memory  for the function to do its work. Those chunks are called "stack frames" or "function frames."

These frames are arranged in a stack. The frame for the most recently called function is always at the top of the stack. When a new function is called, it becomes the active frame, and it is on top of the stack.

The function that is actually doing something at the moment is on top of the stack and is known as the "active frame."

When the function finishes its work, the frame is popped off of the stack. The frame in second place becomes the active frame. It had been paused in the meantime, and now it is active again, because it is on top.

Functions that are not on top, are not running.

[This video explains it well.](https://www.youtube.com/watch?v=aCPkszeKRa4)
