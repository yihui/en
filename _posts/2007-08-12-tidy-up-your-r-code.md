---
layout: post
title: Tidy up your R code
categories:
- R language
tags:
- Highlight
- parse()
- R code
---

This is my function for tidying up R code:

[notice type=download][Downdload the file here](http://yihui.name/en/wp-content/uploads/1186929697_0.gz)[/notice]

Actually it's quite easy, though I didn't know it before. When R was upgraded from 2.4.1 to 2.5.0, the function `source()` was also modified. In the past I used to make use of `source(my_source_file, echo = TRUE, prompt = "")` to "tidy up" my code because it's not convenient for me to type every space between operators, what's more, I have no fixed rules to break a line or make a proper indent. Thus I need a function to automatically "tidy up" my code.

After I've read the source code of the function `source()`, I quickly found that the most critical function is `parse()`, which can turn your code file into neat expressions, and the rest work is just to extract substrings.

    
    tidy.source = function(file = choose.files()) {
       exprs = parse(file)
       for (i in 1:length(exprs)) {
           dep = paste(deparse(exprs[i]), collapse = "\n")
           dep = substring(dep, 12, nchar(dep) - 1)
           cat(dep, "\n")
       }
    }
