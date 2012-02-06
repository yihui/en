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

Amber Watkins gave me a suggestion on the animation for the ratio estimation, and I think this is a good topic for my [`animation`](http://cran.r-project.org/package=animation) package. I've finished writing the initial version of the function `sample.ratio()` for this package, which will appear in the version 1.1-2 a couple of days later.

As we know, the benefit of ratio estimation is that sampling skewness may be  adjusted for, because the estimation of $latex \bar{Y}$ will make use of the information in the relationship of _X_ and _Y_: $latex \bar{X} \cdot (\bar{y}/\bar{x})$. Here is a demo (we can see the ratio estimate, denoted by the red line, generally performs better than $latex \bar{y}$):

[caption id="attachment_480" align="aligncenter" width="480" caption="An animation demo for the ratio estimation"][![An animation demo for the ratio estimation](http://yihui.name/en/wp-content/uploads/2010/03/ratio-estimation.gif)](http://yihui.name/en/wp-content/uploads/2010/03/ratio-estimation.gif)[/caption]



And here is the code:

    
    ## To appear in animation 1.1-2: see documentation there
    sample.ratio = function(x = runif(50, 0, 5), R = 1,
        y = R * x + rnorm(x), size = length(x)/2, p.col = c("blue",
            "red"), p.cex = c(1, 3), p.pch = c(20, 21), m.col = c("black",
            "gray"), legend.loc = "topleft", ...) {
        nmax = ani.options("nmax")
        interval = ani.options("interval")
        N = length(x)
        for (i in 1:nmax) {
            idx = sample(N, size)
            plot(x, y, col = p.col[1], pch = p.pch[1], cex = p.cex[1],
                ...)
    
            points(x[idx], y[idx], col = p.col[2], pch = p.pch[2],
                cex = p.cex[2])
            abline(v = c(mean(x), mean(x[idx])), h = c(mean(y), mean(y[idx])),
                col = m.col, lty = c(2, 1))
            abline(h = mean(x) * mean(y[idx])/mean(x[idx]), col = p.col[2])
            legend(legend.loc, expression(bar(X), bar(x), bar(X) %.%
                (bar(y)/bar(x)), bar(Y), bar(y)), lty = c(2, 1, 1,
                2, 1), col = c(m.col[c(1, 2)], p.col[2], m.col[c(1,
                2)]), bty = "n", ncol = 2)
            Sys.sleep(interval)
        }
    }
    
    library(animation)
    
    sample.ratio()
    
    ## Save as an HTML page
    ani.start()
    sample.ratio(c(runif(50, 0, 2), runif(50, 4, 6)), size = 20)
    ani.stop()
    
    ## Reproduce the above GIF animation
    saveMovie({
        set.seed(123)
        sample.ratio(c(runif(50, 0, 2), runif(50, 4, 6)), size = 20)
    }, moviename = "ratio-estimation", para = list(mar = c(4, 4,
        1, 0.5), mgp = c(2, 1, 0)))
    
