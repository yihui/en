---
layout: post
title: Animations for Spatial Patterns (simulation only!)
categories:
- R language
tags:
- Animation
- GIF
- Map
- Spatial Statistics
---

Sorry, it has been a long long time since I wrote the last blog entry... In the past few months, I was extremely busy with a lot of time-consuming affairs, and it seems that such a trend is not to change in near future. ![](http://yihui.name/en/wp-content/uploads/bo/emot/stupid.gif)

A week ago, [Ye Li](http://individual.utoronto.ca/ye_li/) wrote me an email, telling me his interests in my package [animation](http://cran.r-project.org/web/packages/animation/index.html). And his idea was to implement animations in spatio-temporal models to illustrate certain changes of patterns over time / space (say, clustering). Well, this sounds easy for me to create animations, however, I should know theories behind spatial statistics first of all -- that's much more difficult than the animation techniques and requires more efforts in study.

Here is a simple demonstration using the map of USA:

![](http://yihui.name/en/wp-content/uploads/1209141267_0.gif)

I just used the function `saveMovie()` in the package [animation](http://cran.r-project.org/web/packages/animation/index.html):

[notice type=download][Downdload the file here](http://yihui.name/en/wp-content/uploads/1209147563_0.r)[/notice]

    
    library(animation)
    library(maps)
    saveMovie(for (i in 1:20) map("state", col = sample(terrain.colors(30)),
        fill = TRUE), height = 300, width = 500, interval = 1, outdir = getwd())


Easy enough, huh?
