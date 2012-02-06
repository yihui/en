---
layout: post
title: Slight Changes to Boadilla Theme in Beamer Class
categories:
- Computer Science
tags:
- beamer
- Boadilla
- LaTeX
- theme
---

"`Boadilla`" is a quite plain theme in beamer class; I like plain styles, but I also want a headline in the top indicating the sections so that I can know where I am when I'm giving a speech. Thus I gave some slight changes to this plain theme: the `default` headline has been replaced with "`infolines`", while the definition of the _outer theme_ "`infolines`" has also been modified as I don't have any _subsections_.

The original headline:

    
    \ifbeamer@secheader\else\setbeamertemplate{headline}[default]\fi


The modified version:

    
    \ifbeamer@secheader\else\setbeamertemplate{headline}[infolines]\fi




[![](http://yihui.name/en/wp-content/uploads/1192781106_0.png)](http://yihui.name/en/wp-content/uploads/1192781106_1.png) [![](http://yihui.name/en/wp-content/uploads/1192781122_0.png)](http://yihui.name/en/wp-content/uploads/1192781122_1.png)


[notice type=download][Downdload the file here](http://yihui.name/en/wp-content/uploads//1192775346_0.zip)[/notice]

You may visit [http://yihui.name/en/2007/10/jokes-in-statistics-a-talk-to-be-given-in-cueb/](http://yihui.name/en/2007/10/jokes-in-statistics-a-talk-to-be-given-in-cueb/) to see the effects of my modifications.
