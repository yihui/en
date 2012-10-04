---
layout: post
title: A Demo for the Ratio Estimation in Sampling Survey (Animation)
categories:
- Featured
- R language
- Statistics
tags:
- Animation
- R Language
- Ratio Estimation
- sampling survey
---

Amber Watkins gave me a suggestion on the animation for the ratio estimation, and I think this is a good topic for my [**animation**](http://cran.r-project.org/package=animation) package. I've finished writing the initial version of the function `sample.ratio()` for this package, which will appear in the version 1.1-2 a couple of days later.

As we know, the benefit of ratio estimation is that sampling skewness may be  adjusted for, because the estimation of $latex \bar{Y}$ will make use of the information in the relationship of _X_ and _Y_: $latex \bar{X} \cdot (\bar{y}/\bar{x})$. Here is a demo (we can see the ratio estimate, denoted by the red line, generally performs better than $latex \bar{y}$):

![An animation demo for the ratio estimation](http://i.imgur.com/ob7Fy.gif)
