---
layout: post
title: SyntaxHighlighter Brush for the R Language
categories:
- Computer Science
- R language
tags:
- Code Syntax Highlighter
- JavaScript
- R Language
- Regular Expression
- SyntaxHighlighter
---

Tal Galili requested in the R-help mailing list for a [SyntaxHighlighter](http://alexgorbatchev.com/SyntaxHighlighter/) brush for the R language, so that Wordpress users can highlight their R code easily. I promised to contribute a few minutes on this task, and here is the result:

[notice type=download][shBrushR.js](http://yihui.name/syntaxhighlighter/scripts/shBrushR.js) (1Kb)[/notice]

    
    /**
     *  Author: Yihui Xie
     *  URL: http://yihui.name/en/2010/09/syntaxhighlighter-brush-for-the-r-language
     *  License: GPL-2 | GPL-3
     */
    SyntaxHighlighter.brushes.R = function()
    {
        var keywords = 'if else repeat while function for in next break TRUE FALSE NULL Inf NaN NA NA_integer_ NA_real_ NA_complex_ NA_character_';
        var constants = 'LETTERS letters month.abb month.name pi';
        this.regexList = [
    	{ regex: SyntaxHighlighter.regexLib.singleLinePerlComments,	css: 'comments' },
    	{ regex: SyntaxHighlighter.regexLib.singleQuotedString,		css: 'string' },
    	{ regex: SyntaxHighlighter.regexLib.doubleQuotedString,		css: 'string' },
    	{ regex: new RegExp(this.getKeywords(keywords), 'gm'),		css: 'keyword' },
    	{ regex: new RegExp(this.getKeywords(constants), 'gm'),		css: 'constants' },
    	{ regex: /[\w._]+[ \t]*(?=\()/gm,				css: 'functions' },
        ];
    };
    SyntaxHighlighter.brushes.R.prototype	= new SyntaxHighlighter.Highlighter();
    SyntaxHighlighter.brushes.R.aliases	= ['r', 's', 'splus'];
    




Hopefully Tal can persuade the Wordpress.com manager to add support for R syntax highlighting, so these users do not need to worry much about the installation and configuration. For Wordpress users, there are a couple of choices to make use of SyntaxHighlighter, e.g. the SyntaxHighlighter Evolved plugin, but I find this plugin somehow out-dated because it is still using an old version of SyntaxHighlighter, and it looks not easy to add support for new languages. Therefore I decided not to use any Wordpress plugins, but to manually add the necessary HTML code in my header and footer; this approach works for any HTML pages.

You need to upload the latest version (3.0.83) of SyntaxHighlighter somewhere first (of course, with the R brush `shBrushR.js` added in the `scripts` directory), change the following paths accordingly and insert them before the `<body>` tag in your HTML page:

    
    <link rel="stylesheet" href="path/to/your/syntaxhighlighter/styles/shCore.css" type="text/css" />
    <link rel="stylesheet" href="path/to/your/syntaxhighlighter/styles/shThemeDefault.css" type="text/css" />
    <script type='text/javascript' src='path/to/your/syntaxhighlighter/scripts/shCore.js'></script>
    <script type='text/javascript' src='path/to/your/syntaxhighlighter/scripts/shAutoloader.js'></script>
    


Then add these lines in the footer area (right before the `</body>` tag):

    
    <script type="text/javascript">
    SyntaxHighlighter.autoloader(
     "r  path/to/your/syntaxhighlighter/scripts/shBrushR.js",
     "plain  path/to/your/syntaxhighlighter/scripts/shBrushPlain.js",
     "sql  path/to/your/syntaxhighlighter/scripts/shBrushSql.js",
     "js  path/to/your/syntaxhighlighter/scripts/shBrushJScript.js",
     "html xml  path/to/your/syntaxhighlighter/scripts/shBrushXml.js"
    );
    SyntaxHighlighter.defaults["toolbar"] = false;
    SyntaxHighlighter.all();
    </script>
    


Above is only my configuration; you may refer to the manual of SyntaxHighlighter [for more information](http://alexgorbatchev.com/SyntaxHighlighter/manual/api/autoloader.html). To add those HTML code in your pages, you may modify the theme files (typically the `header.php` and `footer.php`). To make use of highlighting, you need to assign a special CSS class to your `<pre>` tag, e.g.

    
    <pre class="brush: r">
    test.function = function(r) {
        return(pi * r^2)
    }
    test.function(1)
    </pre>
    


The result:

    
    test.function = function(r) {
        return(pi * r^2)
    }
    test.function(1)
    


You may double-click on the code to select all of them, and copy them with normally (e.g. with Ctrl + C). The old version of SyntaxHighlighter uses a Flash file to copy the code to the clipboard and you have to select the line numbers as well when you only want to select the code; I do not like these features.

Finally, the JS brush file can be improved in a few aspects but in fact I'm not quite good at JS RegExp, so I leave these possible improvements to the readers:



	
  1. highlight the function arguments -- loosely speaking, the pattern is: they are in parentheses "()" followed by "="; the first argument follows "(" and others follow ",".

	
  2. highlight the variables -- those strings followed by "<-" or "=" when "=" is not in paretheses.


P.S. today I converted all my `<pre>` tags in this whole site to `<pre class="brush: r">` using a SQL command:

    
    UPDATE `wp_posts`
    SET `post_content` = REPLACE(`post_content`, '<pre>', '<pre class="brush: r">')
    WHERE `post_content` LIKE '%<pre>%';
    


You may also consider this batch processing instead of manually adding `class="brush: r"` to your <pre> tags.
