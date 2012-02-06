---
layout: post
title: Availability of the PNG Device in R
categories:
- R language
tags:
- Graphics
- Operating System
- png()
- R Package
---

In some operating systems, a few R graphical devices might not be available, so we have to check the capabilities of devices before writing code for creating image files in case that there should be errors. The function is just [`capabilities()`](http://finzi.psych.upenn.edu/R/library/base/html/capabilities.html).

I didn't notice this and was wondering why there were errors in the [check summary](http://cran.r-project.org/src/contrib/checkSummary.html) of my R package "[animation](http://cran.r-project.org/src/contrib/Descriptions/animation.html)". Now I understand the reason. Thus I'll modify the function [`savePNG()`](http://finzi.psych.upenn.edu/R/library/animation/html/savePNG.html) a little.
