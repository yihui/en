---
layout: post
title: Customizing the Theme of Your R HTML Help
categories:
- Computer Science
- R language
tags:
- CSS
- R HTML Help
---

R's default theme of the HTML help pages is too plain for me to read, but we can easily modify the theme, which is essentially a CSS file. You can find the file under:

{% highlight r %}
file.path(R.home('doc'), 'html', 'R.css')
{% endhighlight %}

Simply replace this file with my version of [R.css](https://github.com/yihui/configuration/raw/master/R.css) (1K) which looks like:

![R HTML Help Theme](http://i.imgur.com/9DReh.png)

Of course you can design your own `R.css` if you know CSS.

