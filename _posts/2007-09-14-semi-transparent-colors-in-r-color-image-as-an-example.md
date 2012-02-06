---
layout: post
title: 'Semi-transparent Colors in R: Color Image as an Example'
categories:
- R language
tags:
- Graphics
- image()
- pdf()
---

There are many graphical functions offering the availability of the parameter `alpha` which is usually used to specify semi-transparent colors, however, such kind of colors can only be displayed in certain devices, as stated in the help of `rgb()`:


> Semi-transparent colors (`0 < alpha < 1`) are supported only on  some devices: at the time of writing only on the `pdf` and (on MacOS X) `quartz` devices as  well as several third-party devices such as those in packages **Cairo**, **cairoDevice**, **JavaGD** and **RSvgDevice**.


Here is an example illustrating semi-transparent colors in a `pdf` device:


[![](http://yihui.name/en/wp-content/uploads/1189762567_0.png)](http://yihui.name/en/wp-content/uploads/1189762567_1.png)



[notice type=download][Downdload the file here](http://yihui.name/en/wp-content/uploads/1189762512_0.gz)[/notice]

    
    tmp = file.path(tempdir(), "alpha.pdf")
    pdf(tmp, version = "1.4") # open a pdf device in the temp dir
    plot(rnorm(100), ylim = c(-3, 3), xlab = "x", ylab = "y",
       main = "Add a semi-transparent color image to a graph")
    image(x = seq(1, 100, length = 20), y = seq(-3, 3,
       length = 20), z = matrix(rnorm(400), 20), col = heat.colors(20,
       alpha = 0.5), add = T) # here we set alpha = 0.5
    dev.off()
    shell.exec(tmp) # open the pdf file now


You may try other devices such as `postscript`, `png`, `jpg`, or `bmp`, etc, and you'll find `alpha` is not supported there. Please do note that _the image in this page was converted from the PDF file_.
