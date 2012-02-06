---
layout: post
title: Customizing the Theme of Your R HTML Help
categories:
- Computer Science
- R language
tags:
- CSS
- R HTML Help
- R.css
---

R's default theme of the HTML help pages is too plain for me to read, but we can easily modify the theme, which is essentially a CSS file. You can find the file under:

    
    file.path(R.home('doc'), 'html', 'R.css')
    


Simply replace this file with my version:

[notice type=download]Download [R.css](https://github.com/yihui/configuration/raw/master/R.css) (1K)[/notice]

which looks like:

[caption id="attachment_689" align="aligncenter" width="587" caption="R HTML Help Theme"][![R HTML Help Theme](http://yihui.name/en/wp-content/uploads/2011/01/r-html-help-theme.png)](http://yihui.name/en/wp-content/uploads/2011/01/r-html-help-theme.png)[/caption]

Of course you can design your own `R.css` if you know CSS.
