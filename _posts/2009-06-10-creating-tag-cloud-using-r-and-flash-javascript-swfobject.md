---
layout: post
title: Creating Tag Cloud Using R and Flash / JavaScript (SWFObject)
categories:
- Computer Science
- Featured
- R language
tags:
- Flash
- Gorjanc Gregor
- JavaScript
- maptools
- pointLabel()
- R Language
- R4X
- Romain Francois
- Simon Urbanek
- snippets
- SWFObject
- Tag Cloud
- Wordpress
- wp-cumulus
---

Tag cloud is a bunch of words drawn in a graph with their sizes proportional to their frequency; it's widely used in blogs to visualize tags. We can observe important words quickly from a tag cloud, as they often appear in large fontsize. Tony N. Brown asked how to "[graphically represent frequency of words in a speech](https://stat.ethz.ch/pipermail/r-help/2009-June/200645.html)" the other day in R-help list, which is actually a problem about the tag cloud:


> I recently saw a graph on television that displayed selected words/phrases in a speech scaled in size according to their frequency. So words/phrases that were often used appeared large and words that were rarely used appeared small. [...]


Marc Schwartz mentioned that [Gorjanc Gregor](http://ggorjan.blogspot.com/) has done [some work](http://www.bfro.uni-lj.si/MR/ggorjan/software/R/index.html#tagCloud) years ago using R (in grid graphics). The obstacle of creating tag cloud in R, as Gorjanc wrote, lies in deciding the placement of words, and it would be much easier for other applications such as browsers to arrange the texts. That's true -- there have already been a lot of mature programs to deal with tag cloud. One of them is the `wp-cumulus` plugin for WordPress, which makes use of a Flash object to generate the tag cloud, and it has fantastic 3D rotation effect of the cloud.


# 1. Arranging text labels with `pointLabel()`


Before introducing how to port the plugin into R, I'd like to introduce an R function `pointLabel()` in `maptools` package and it can partially solve the problem of arranging text labels in a plot (using simulated annealing or genetic algorithm). Here is a simulated example:

[caption id="attachment_226" align="aligncenter" width="550" caption="Simulated Tag Cloud with R function pointLabel() in maptools"][![Simulated Tag Cloud with R function pointLabel() in maptools](http://yihui.name/en/wp-content/uploads/2009/06/tagcloud-with-pointlabel.png)](http://yihui.name/en/2009/06/creating-tag-cloud-using-r-and-flash-javascript-swfobject/tagcloud-with-pointlabel/)[/caption]



    
    library(maptools)
    set.seed(123)
    x = runif(19)
    y = runif(19)
    w = c("R", "is", "free", "software", "and", "comes",
        "with", "ABSOLUTELY", "NO", "WARRANTY", "You", "are", "welcome",
        "to", "redistribute", "it", "under", "certain", "conditions")
    par(ann = FALSE, xpd = NA, mar = rep(2, 4))
    plot(x, y, type = "n", axes = FALSE)
    pointLabel(x, y, w, cex = runif(19, 1, 5))


I was fortunate to get a very neat graph with no labels overlapping, but I don't think this is a good solution, as it doesn't take care of the initial locations of the words. My rough idea about deciding the initial locations is to sample on circles with radii proportional to the frequency, i.e. let $latex x=\textrm{freq}*\sin(\theta)$ and $latex y=\textrm{freq}*\cos(\theta)$ where $latex \theta\sim U(0,2\pi)$. In this case, important words will be placed near the center of the plot.


# 2. Creating tag cloud in a Flash movie using R


The problem becomes quite easy with a Flash movie [`tagcloud.swf`](http://www.roytanck.com/2008/05/19/how-to-repurpose-my-tag-cloud-flash-movie/) and a JavaScript program [`swfobject.js`](http://blog.deconcept.com/swfobject/). The mechanism, briefly speaking, is that the tag information is passed to the Flash object by JavaScript, and the Flash object will read the variable `tagcloud` where the sizes, colors and hyperlinks of tags are stored. Finally the tags are visualized like rotating cloud.

It's not difficult to pass the tag information to JavaScript in pure text. Below is the function which will create an HTML page by default with a tag cloud Flash movie inside it:

[notice type=download]Download the source code: [tagCloud.r.gz](http://yihui.name/en/wp-content/uploads/2009/06/tagcloudr.gz) (1.18Kb)[/notice]

    
    #------------------------------------------------------------------------------#
    # generating tag cloud in R using Flash and SWFObject                          #
    # tagData: a data.frame containing columns 'tag', 'link', 'count' and optional #
    #     columns 'color' and 'hicolor'                                            #
    # other parameters are self-explaining if you are familiar with                #
    #     the WP plugin 'wp-cumulus'                                               #
    #------------------------------------------------------------------------------#
    tagCloud = function(tagData, htmlOutput = "tagCloud.html",
        SWFPath, JSPath, divId = "tagCloudId", width = 600, height = 400,
        transparent = FALSE, tcolor = "333333", tcolor2 = "009900",
        hicolor = "ff0000", distr = "true", tspeed = 100, version = 9,
        bgcolor = "ffffff", useXML = FALSE, htmlTitle = "Tag Cloud",
        noFlashJS, target = NULL, scriptOnly = FALSE) {
        if (missing(SWFPath))
            SWFPath = "http://www.roytanck.com/wp-content/plugins/wp-cumulus/tagcloud.swf"
        if (missing(JSPath))
            JSPath = "http://www.roytanck.com/wp-content/plugins/wp-cumulus/swfobject.js"
        if (missing(noFlashJS))
            noFlashJS = "This will be shown to users with no Flash or Javascript."
        tagXML = sprintf("<tags>%s</tags>", paste(sprintf("<a href='%s' style='%s'%s%s%s>%s</a>",
            tagData$link, tagData$count, if (is.null(target))
                ""
            else sprintf(" target='%s'", target), if (is.null(tagData$color))
                ""
            else ifelse(is.na(tagData$color), sprintf(" color='0x%s'",
                tagData$color, ""), ""), if (is.null(tagData$hicolor))
                ""
            else ifelse(is.na(tagData$hicolor), sprintf(" hicolor='0x%s'",
                tagData$hicolor, ""), ""), tagData$tag), collapse = ""))
        if (useXML)
            cat(tagXML, file = file.path(dirname(htmlOutput), "tagCloud.xml"))
        cat(ifelse(scriptOnly, "",
        sprintf("<!DOCTYPE html PUBLIC \"-//W3C//DTD XHTML 1.0 Transitional//EN\"
        ?\"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd\">
        <html xmlns=\"http://www.w3.org/1999/xhtml\">
        <head>
        <title>%s</title>
        <meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\" />
        </head>
        <body>",
            htmlTitle)), sprintf("\t<script type=\"text/javascript\" src=\"%s\"></script>",
            JSPath), sprintf("\t<div id=\"%s\">%s</div>", divId,
            noFlashJS), sprintf("\t<script type=\"text/javascript\">
            \t\tvar so = new SWFObject(\"%s\", \"tagcloud\", \"%d\", \"%d\", \"%d\", \"#%s\");
            %s\t\tso.addVariable(\"mode\", \"tags\");\n\t\tso.addVariable(\"tcolor\", \"0x%s\");
            \t\tso.addVariable(\"tcolor2\", \"0x%s\");\n\t\tso.addVariable(\"hicolor\", \"0x%s\");
            \t\tso.addVariable(\"tspeed\", \"%d\");\n\t\tso.addVariable(\"distr\", \"%s\");
            %s\n\t\tso.write(\"%s\");\n\t\t</script>\n",
            SWFPath, width, height, version, bgcolor, ifelse(transparent,
                "\t\tso.addParam(\"wmode\", \"transparent\");\n",
                ""), tcolor, tcolor2, hicolor, tspeed, distr, ifelse(useXML,
                "\t\tso.addVariable(\"xmlpath\", \"tagcloud.xml\");",
                sprintf("\t\tso.addVariable(\"tagcloud\", \"%s\");",
                    tagXML)), divId), ifelse(scriptOnly, "", "</body>\n\n</html>"),
            file = ifelse(scriptOnly, stdout(), htmlOutput), sep = "\n")
    }


The main argument is `tagData` which is a data.frame containing at least three columns (`tag`, `link` and `count`) and looks like:

    
    > head(tagData)
                    tag                                        link count
    1 2D Kernel Density http://yihui.name/en/tag/2d-kernel-density/     1
    2         algorithm         http://yihui.name/en/tag/algorithm/     1
    3         Animation         http://yihui.name/en/tag/animation/    11
    4           AniWiki           http://yihui.name/en/tag/aniwiki/     2
    5            Arcing            http://yihui.name/en/tag/arcing/     1
    6          arrows()            http://yihui.name/en/tag/arrows/     1


Additional columns `color` and `hicolor` will be used if they exist (hexadecimal numbers specifying RGB), e.g.

    
    > head(tagData)
                    tag                                        link count  color hicolor
    1 2D Kernel Density http://yihui.name/en/tag/2d-kernel-density/     1 2163bb  f0763d
    2         algorithm         http://yihui.name/en/tag/algorithm/     1 9f0f38  d825b1
    3         Animation         http://yihui.name/en/tag/animation/    11 800130  5b8d6a
    4           AniWiki           http://yihui.name/en/tag/aniwiki/     2 7ce1df  6607b0
    5            Arcing            http://yihui.name/en/tag/arcing/     1 df4e4a  f5cdf2
    6          arrows()            http://yihui.name/en/tag/arrows/     1 31f5fb  19d50d




# 3. Example


Here is an example on visualizing my blog tags. You may need the following `swf` and `js` files first if you wish the loading would be faster (by default your browser needs to download these two files from _roytanck.com_ first).

[notice type=download]Download the tag cloud Flash file [tagcloud.swf](http://yihui.name/en/wp-content/uploads/2009/06/tagcloud.swf) (33.7Kb) and JavaScript [swfoject.js](http://yihui.name/en/wp-content/uploads/2009/06/swfobject.js) (5.94Kb) as well as the data [tagData.gz](http://yihui.name/en/wp-content/uploads/2009/06/tagdata.gz) (1.43Kb).[/notice]

    
    tagCloud(tagData)
    # use tagCloud(tagData, SWFPath = "tagcloud.swf", JSPath = "swfobject.js")
    #    if you have downloaded these files to your work directory, i.e. getwd(),
    #    this will save you a few seconds loading the flash


The above code will generate an HTML page like this:

{...omited...}

You can adjust the parameters as you wish.


# 4. Other issues


There is still one more step to answer Tony's original question, namely splitting the speech into single words and computing the frequency. This can be (roughly) done by `strsplit(..., split = " ")` and `table()`.

Encoding problems may exist in the above code, but `URLencode(tagXML)` could be of help.

Only Latin characters are supported, but there's possibility to modify the Flash source file to support other languages. See [Roy Tanck's post](http://www.roytanck.com/2008/03/15/wp-cumulus-released/) for more information.

Other R resources I know so far:



	
  * The R package [`R4X`](http://r-forge.r-project.org/projects/r4x/) by [Romain Fran?ois](http://romainfrancois.blog.free.fr): you can generate an HTML page containing the tags with _dynamic_ classes attached to the `<span>` tags (install the package and read its vignette: `install.packages('R4X', repos='http://r-forge.r-project.org'); vignette('r4xslides', package='R4X')`)

	
  * The R package [`snippets`](http://www.rforge.net/snippets/) by [Simon Urbanek](http://simon.urbanek.info/): there is a function `cloud()` to create word cloud; words are arranged from top to bottom and left to right. See the 23rd reply below for an example (thanks, Emilio).


