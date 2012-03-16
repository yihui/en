---
layout: post
title: Identify Data Points in Off-Screen R Graphics Devices
categories:
- R language
tags:
- Graphics
- Graphics Device
- identify()
---

Today Ruya Gokhan Kocer asked me how to use the R function `identify()` in off-screen graphics devices. Actually it's pretty easy as long as we obtain the list returned by `identify(pos = TRUE)`. For example,

{% highlight r %}
# open a windows device
x11()
x = rnorm(20)
y = rnorm(20)
plot(x, y)
# identify 5 points
id = identify(x, y, n = 5, pos = TRUE)

# $ind
# [1]  2  6 10 14 16
#
# $pos
# [1] 1 1 4 4 1

# then open a bitmap device
png("identify.png")
plot(x, y)
# use the information from above mouse click
text(x[id$ind], y[id$ind], id$ind, pos = id$pos)
dev.off()
{% endhighlight %}

