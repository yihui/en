---
layout: post
title: Auto-completion in Notepad++ for R Script
categories:
- Computer Science
- R language
tags:
- Auto-completion
- Notepad++
- R Language
---

Auto-completion is fancy in a text editor. Notepad++ does not support auto-completion for the R language, so I spent a couple of hours on creating such an XML file to support R:

[notice type=download]Download: [R.xml (938Kb)](http://yihui.name/en/wp-content/uploads/2010/08/R.xml)[/notice]

Put it under '`plugins/APIs`' in the installation directory of Notepad++ (you can see several other XML files there supporting different languages such as C), and make sure you have enabled auto-completion in Notepad++ (`Settings --> Preferences --> Backup/Auto-completion`). Open an R script and start typing a familiar function (e.g. `paste()`), you will see some candidates in a drop-down list like this:

[caption id="attachment_545" align="aligncenter" width="366" caption="Show parameters of R functions in Notepad++"][![Show parameters of R functions in Notepad++](http://yihui.name/en/wp-content/uploads/2010/08/paste-parameters-notepad++.png)](http://yihui.name/en/wp-content/uploads/2010/08/paste-parameters-notepad++.png)[/caption]

Hit the Enter key if the function name selected in the list is correct for you, then type '`(`' and you will see hints for parameters:

[caption id="attachment_544" align="aligncenter" width="486" caption="Auto-completion in Notepad++ for R script"][![Auto-completion in Notepad++ for R script](http://yihui.name/en/wp-content/uploads/2010/08/paste-auto-completion-notepad++.png)](http://yihui.name/en/wp-content/uploads/2010/08/paste-auto-completion-notepad++.png)[/caption]

The file R.xml was actually generated from R; it contains almost all visible R objects in base R packages as well as recommended packages like MASS. You may create an extended XML file (containing keywords from other packages) by yourself after loading the packages you need into your current workspace, and run:

    
    source('http://yihui.name/en/wp-content/uploads/2010/08/Npp_R_Auto_Completion.r')
    # R.xml will be generated under your current work directory: getwd()
