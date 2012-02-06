---
layout: post
title: Simulation of Burning Fire in R
categories:
- R language
tags:
- Animation
- Capital of Statistics
- Fire
- Flash
- heat.colors()
- image()
- Linlin Yan
- png2swf
- R Language
- saveSWF()
- Simulation
---

Linlin Yan posted [a cool (hot?) simulation of burning fire](http://cos.name/en/topic/the-first-r-code-here) with R in the [COS forum](http://cos.name/en/) yesterday, which was indeed a _warm_ welcome. I'm not sure whether our forum members will be scared by the "fire" under the title "Welcome to COS Forum". :grin: The fire was mainly created by the function `image()` with carefully designed rows and columns in heated colors `heat.colors()`. Here is one of the pictures generated from his code:

[caption id="attachment_273" align="aligncenter" width="480" caption="Simulation of Burning Fire in R"][![Simulation of Burning Fire in R](http://yihui.name/en/wp-content/uploads/2009/06/simulation-of-burning-fire-in-r.png)](http://yihui.name/en/wp-content/uploads/2009/06/simulation-of-burning-fire-in-r.png)[/caption]

And code here:

    
    Fire <- function(row = 100, col = 100, time = 500, fade = 0.03) {
      fire <- matrix(0, col, row);
      fire[,1] <- runif(col);
    
      for (t in 1:time) {
        image(fire, col = rev(heat.colors(row)),
          axes = FALSE, main = "Welcome to COS Forum!");
    
        fire <- (
          fire +
          cbind(fire[,1], fire[c(col,1:(col-1)), 1:(row - 1)]) +
          cbind(fire[,1], fire[                , 1:(row - 1)]) +
          cbind(fire[,1], fire[c(2:col,1)      , 1:(row - 1)])
          ) / 4;
        fire <- cbind(fire[,1], (fire + fade / 5 - runif(1, max = fade))[,-1]);
        fire[fire < 0] <- 0;
    
        r <- runif(1);
        if (r < .1) fire[,1] <- fire[,1][c(2:col, 1)];
        if (r > .9) fire[,1] <- fire[,1][c(col, 1:(col-1))];
      };
      NULL;
    }


The speed of drawing animation frames is rather slow in my computer, but it doesn't matter since we can use the `animation` package (hey, you are advertising!) to save all the image frames and convert them to a single animation file.

    
    library(animation)
    # set row=50 instead of the default 100 to let the fire burn to the ceiling
    # make sure you have installed the SWF Tools and the command 'png2swf' can
    #    be executed (with or w/o it installation path); see ?saveSWF
    saveSWF(Fire(50), interval = 0.05, dev = "png", outdir = getwd(),
        para = list(mar = c(0, 0, 2, 0)))






Now the animation is much more smooth than what we saw in R graphics window.

Thanks, awesome Linlin.

[notice type=attention]For those who are not familiar with the website "[Capital of Statistics](http://cos.name/)" (COS), I'd like to give a brief introduction here: this website was built 3 years ago by me and it was originally a Chinese website for discussion in statistics, but later I thought a place for English-speaking visitors was also necessary, so an English forum was constructed: [http://cos.name/en/](http://cos.name/en/). Please feel free to join us if you are interested.[/notice] 
