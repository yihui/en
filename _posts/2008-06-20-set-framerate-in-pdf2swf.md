---
layout: post
title: Set framerate in pdf2swf
categories:
- Computer Science
tags:
- Flash
- framerate
- pdf2swf
- SWF Tools
---

To convert PDF to SWF, we may use the utility `pdf2swf` in [SWF Tools](http://www.swftools.org). However, the feature of specifying the _frame rate_ has not been documented yet. The other day Matthias Kramm told me this option just hided behind `-s`.

{% highlight text %}
Usage: pdf2swf [-options] file.pdf -o file.swf

-h , --help               Print short help message and exit
-V , --version            Print version info and exit
....
-p , --pages range        Convert only pages in range with range e.g. 1-20 or 1,4,6,9-11 or
....
-s , --set param=value    Set a SWF encoder specific parameter.
                         ?See pdf2swf -s help for more information.
....
{% endhighlight %}

To specify the frame rate, we just need to use `pdf2swf -s framerate=?`.

