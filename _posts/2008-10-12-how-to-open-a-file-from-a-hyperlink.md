---
layout: post
title: How to Open a File from a Hyperlink
categories:
- Computer Science
tags:
- hyperlink
- hyperref
- LaTeX
- pdf()
- run
---

I'd like to thank [Michael Friendly](http://www.math.yorku.ca/SCS/friendly.html) for telling me this: just create a hyperlink to the file with the `run` protocol and then you can open a file directly from the hyperlink. For example, in LaTeX with the **hyperref** package, you may use `\href{run:path/to/some.file}{some link}` to open this `some.file` in your PDF. This is a useful hack that I have been looking for over a long time.

