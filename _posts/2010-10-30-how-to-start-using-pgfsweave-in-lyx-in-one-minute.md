---
layout: post
title: How to Start Using (pgf)Sweave in LyX in One Minute
categories:
- Featured
- R language
tags:
- Literate Programming
- LyX
- pgfSweave
- R Language
- Reproducible Research
- Sweave
---

[notice type=alert]Warning: this post is becoming dated! I'm working with LyX 2.0 now and you are encouraged to try the new version in the future. [Some preliminary work can be found here](http://yihui.name/en/2011/05/sweave-and-pgfsweave-in-lyx-2-0-x-experimental/).[/notice]


# 0. Summary


Take a look at the video in this entry if you don't understand the title. To put it short,



	
  1. install LyX and R as well as a working LaTeX toolkit such as MikTeX or TeXLive or MacTeX;

	
  2. run `source('http://gitorious.org/yihui/lyx-sweave/blobs/raw/master/lyx-sweave-config.R')` in R under Windows or Ubuntu or Mac; I tried my best to automatically configure LaTeX, R and LyX;

	
  3. restart LyX as instructed, and you can enjoy `pgfSweave` in LyX now -- either play with my demo ([demo 1](http://gitorious.org/yihui/lyx-sweave/blobs/raw/master/demo/LyX-pgfSweave-minimal-demo.lyx); [demo 2](http://gitorious.org/yihui/lyx-sweave/blobs/raw/master/demo/LyX-pgfSweave-demo-Yihui-Xie.lyx) with [bibliography](http://gitorious.org/yihui/lyx-sweave/blobs/raw/master/demo/LyX-pgfSweave-demo-Yihui-Xie.bib); [a beamer demo](http://gitorious.org/yihui/lyx-sweave/blobs/raw/master/demo/animation-2011-Yihui-Xie.lyx); [an animation demo](http://gitorious.org/yihui/lyx-sweave/blobs/raw/master/demo/animation-LyX-Sweave-demo.lyx) with [PDF output](http://gitorious.org/yihui/lyx-sweave/blobs/raw/master/demo/animation-LyX-Sweave-demo.pdf)), or DIY: create a new document, change the document class to article (Sweave noweb) from Document --> Settings, switch the environment to Scrap from the top-left drop list, start your Sweave code chunks like
`<<test>>=
rnorm(10)
@`
and click the PDF button to compile this document. Done. [Take a look at this video](http://vimeo.com/16374405) if you feel confused.


This works for MikTeX under Windows (Server 2003 / Win7), and TeXLive 2009 under Ubuntu 10.10, MacTeX 2010 under Mac OS; R 2.12.0 or 2.11.1; LyX 1.6.x.

Gregor Gorjanc published an interesting article ``Using Sweave with LyX'' in [R News in 2008](http://cran.r-project.org/doc/Rnews/Rnews_2008-1.pdf), which (I believe) makes it much easier to use Sweave. I use command-line tools a lot every day, but I am still ``GUI-addicted''. (I don't want to comment more about Microsoft Word here.) [LyX](http://www.lyx.org) is a somewhat WYSIWYG tool based on LaTeX, and on the first time I saw it I decided that Word was completely useless to me from then on. In the past, I did not like writing LaTeX documents just because I hate wasting my time on typing the raw commands. For example, I hate typing `\item` in an `itemize` environment each time I need a new item. There might be some text editors which can automatically do this tedious task, but the more serious problem is I cannot see the whole picture -- in my eyes there are only commands; my imagination is limited -- it is difficult for me to imagine `\section{}` to be a section title. However, LyX has provided a perfect solution to lazy people like me. We don't have to write LaTeX documents from scratch, and everything is intuitive in LyX. You can clearly _**see**_ the structure of your document, as well as figures (instead of `\includegraphics{}`), tables (instead of the gory `\begin{table} numbers 1.4 & 2.2 & 3.8`), headings (instead of `\title{} \section{}`) and math formulae (instead of `$\frac{\gamma}{\alpha_{ij}}$`)... In all, it is a whole lot easier and faster to write LaTeX documents in LyX. This is the main reason for an easier life of Sweave, because a Sweave document is nothing but a mixture of LaTeX and R code.


# 1. Introduction


Although Sweave in LyX is convenient to use, it is not a trivial task for beginners to configure and understand how it works. The video below can give you an idea on what it looks like in LyX (Chinese visitors please go to [Photobucket](http://s288.photobucket.com/albums/ll181/xieyihui/?action=view&current=lyx.mp4) or [56.com](http://www.56.com/u11/v_NTYxNDgxMzY.html) to watch the video):



As we can see, R code can be easily embedded into LyX. If you are familiar with Sweave, you don't even need time to learn anything. For those who do not know Sweave well, a good place to look at is the help page `?Sweave`. A Sweave document is dynamic in the sense that everything in the document can be changed by the R code (nothing is hard-coded), so we don't need to worry too much about the specific numbers and plots in the output. Instead, we focus on the code which produces these output. In the above video, I used a LaTeX macro `\Sexpr{}` to output the value of `pi` and I don't need to write the specific number 3.1415926 there.


# 2. `pgfSweave`


While Sweave is a great invention for reproducible research, there are other packages which can improve R's default Sweave functionality. A brilliant one is the `pgfSweave` package. It was built upon the cacheSweave package to support caching R objects (to avoid unnecessary repeated computations and save time), and it also provided a mechanism to cache graphics! Beside the speed issues, a remarkable feature is the quality of graphics -- it is unbeatable. I'm not exaggerating. This packages uses the `tikzDevice` package to produce pgf/tikz graphics which are essentially LaTeX code, in other words, the R graphics are represented in the LaTeX language so that they are treated (compiled) in the same way as the body of a LaTeX document. This will make the style of graphics completely consistent with the body of a document, e.g. the fonts.

By the way, I also like the `nogin` option for Sweave.sty to be the default in pgfSweave, because I really don't like the idea of setting the size of graphics by a LaTeX macro `\setkeys{Gin}{width=0.8\textwidth}`. In pgfSweave, we just set the width and height naturally in the code chunk options like `<<width = 5, height =4>>=`.

pgfSweave comes with a command line usage like Sweave: `R CMD pgfSweave your-file.Rnw`. I'm not using this approach in LyX, because this requires system admin privilege to install pgfSweave. Instead, I use this way:

    
    R -q -e "library(pgfSweave);pgfSweave('yourfile.Rnw')"


R can accept a string in its `-e` argument, e.g.

    
    yihui@xie:~$ R -q -e "rnorm(5)"
    > rnorm(5)
    [1] -0.2970093 -0.2171444  1.5645127  0.5422097  0.7359204


Later I'll explain how to connect LyX and R/pgfSweave in this way.


# 3. Configuration


To make LyX work with Sweave, we need to take these steps:



	
  1. put the LaTeX style files such as Sweave.sty under the texmf tree

	
  2. define the literate programming environment in LyX (literate-scrap.inc) and corresponding layouts

	
  3. (the critical step) create converters to convert Sweave documents to LaTeX (this is done in the preferences file) and the converter is like
`R -e "library(pgfSweave);pgfSweave($$i,compile.tex=FALSE)"`
where `$$i` is a variable in LyX denoting the input file


Each step involves with several novel concepts for beginners. For example, you may ask ``what's a texmf tree?'' ``What's a layout file? Where is it?'' ``Where is the preference file?'' ``What's a converter?''...

I spent several hours on writing an R script trying to cover all these gory details automatically, so the configuration becomes as easy as

    
    source('http://gitorious.org/yihui/lyx-sweave/blobs/raw/master/lyx-sweave-config.R')


This is convenient, which is good. But I have to confess it is a really nasty script -- (for Windows) it calls several system commands to help the configuration, such as `initexmf` or `mpm`, or `setx` to set the system PATH variable; this is a dangerous practice and some users may feel extremely uncomfortable with it. Under Mac and Linux, it tries to download and copy files to your home directory so that LaTeX and LyX will work properly. It will do no harm to your system, but I need to warn you first in the spirit of ``open source''.


# 4. Gory Details


You are still reading... So first you have to read Gregor Gorjanc's paper in R News and that make most things clear in this blog entry. A few more things I need to add are:



	
  1. I modified the file literate-scrap.inc so that we can hit the `Enter` key to start a new line in R code, which is more natural for the users, I think. Gregor's original definition for breaking a line was `Ctrl+Enter`.

	
  2. I added another environment in LyX named `ScrapCenter`, which is for the code chunks that produce plots, because plots are usually centered in the page.

	
  3. I also modified the LaTeX style definitions based on `Sweave.sty` to add line numbers to the code chunks in LaTeX (these definitions are in the preamble).

	
  4. I did not add R's texmf directory to the root directory of MikTeX; instead I copied all the files to the user directory of MikTeX (this can be a bad practice too) and I did the same thing to Mac and Ubuntu. This will make LaTeX know the style definitions for Sweave.

	
  5. I copied LyX configurations to the user directory of LyX as well. I need to mention here that all the old configurations will be overwritten except the `preferences` file, with which I handled in a special way: if the old `preferences` has defined Sweave converters, I will update these converters, otherwise I will _append_ the Sweave converters to the old preferences file. This makes sense, I believe. Users do not want their `preferences` to be completely overwritten.

	
  6. Under Mac and Windows, the path for LyX will be hard-coded as `LyX16` if I cannot find a LyX configuration directory. Linux usually stores configurations in the home directory as folders with a dot, e.g. `~/.lyx`. This is good because we don't need to worry about the version numbers.

	
  7. `pgfSweave` will try to format your R code if the chunk or global option `tidy = TRUE` (and better if additionally in R: `options(keep.blank.line = FALSE)`), e.g. add spaces and indent where appropriate when your source code is not well-formatted. This is my suggestion (and contribution), and I think this makes sense because many users just do not care about formatting R code (hence torture other people's eyes), so let's do it automatically with `pgfSweave`<del></del>[](https://github.com/cameronbracken/pgfSweave).




# 5. Demo


After you have got everything ready, here are two demos that you can play with:

[notice type=download][demo 1](http://gitorious.org/yihui/lyx-sweave/blobs/raw/master/demo/LyX-pgfSweave-minimal-demo.lyx); [demo 2](http://gitorious.org/yihui/lyx-sweave/blobs/raw/master/demo/LyX-pgfSweave-demo-Yihui-Xie.lyx) with [bibliography](http://gitorious.org/yihui/lyx-sweave/blobs/raw/master/demo/LyX-pgfSweave-demo-Yihui-Xie.bib)[/notice]


# 6. Bad News


The ``No-Free-Lunch'' theorem is always true. With LyX/(pgf)Sweave, there might be some potential headaches:



	
  1. error handling and debugging: thanks to Kai Ying, this problem has been almost solved. The trick is to use command line redirection so that all the error messages (and normal messages) will be redirected to a log file whose name is the same as your Sweave document except that an extension `.log` will be added. For example, suppose your LyX file name is `myfile.lyx`, then the Sweave file name will be `myfile.Rnw`, and the log file will be `myfile.Rnw.log`. This file can be found in the temporary directory of LyX. If you have no idea about where is that directory, please go to `Tools --> Preferences --> Path --> Temporary directory` (Mac has a different menu). Looking at this log file can be extremely useful to know where it goes wrong in R code in case of errors, because LyX will be unable to tell you any information about the R process (it only records the LaTX log file).

The real trick is:

    
    R -e "library(pgfSweave);pgfSweave($$i,compile.tex=FALSE)" > $$i.log 2>&1


This looks weird but is useful in command line environments.
	
  2. special LaTeX characters: since `pgfSweave` uses LaTeX to create graphics output, you need special care for the special characters in LaTeX. For example, `plot(x, y, xlab="x_label")` will result in an error, although it is legitimate R code -- it contains a LaTeX special character `_`, which needs to be escaped as `\_`, so the correct way to write the above expression is `plot(x, y, xlab="x\\_label")` (why do we need two backslashes? because the backslash is a special character in R...). Sometimes the situation is even worse, for instance, there are special characters which are beyond your control (they are introduced by the authors of functions). In this case, you might consider the `sanitize` option, e.g. `<<sanitize=TRUE>>=`. This option can protect some of your special characters. See the package `tikzDevice` for more details. The other (easier) way to overcome this difficulty is to use PDF graphics; see 3 below.

	
  3. large pictures: although tikz pictures are beautiful, they can get stuck when you draw a complicated plot. Here ``complicated'' means there are a lot of graphical elements in a plot, e.g. `plot(rnorm(10000))`. This will make LaTeX complain that it is running out of memory to compile the picture. I feel most frustrated on this issue, but there is a solution too: just turn off the tikz option and open the pdf option, i.e. use PDF plots instead of tikz. In this case, your font style will not be consistent with the whole LaTeX document, but [there are also approaches to alleviate this problem](http://yihui.name/en/2010/03/font-families-for-the-r-pdf-device/) is you are a picky user. In all, use a code chunk like this:


    
    <<fig=TRUE, tikz=FALSE, pdf=TRUE, width=5, height=4>>=
    draw.your.plot()
    @


By the way, even if you can use tikz pictures, you will probably get stuck when you print the PDF document containing a complicated tikz picture. You will have to increase the memory of the printer.
	
  4. `lattice` or `ggplot2` graphics: this problem is not specific to `pgfSweave`, but rather it is a common problem for Sweave users -- they often forget to `print()` the plots. In `lattice` and `ggplot2`, all the plot objects need to be printed, otherwise they will not be actually drawn. Some users may ask, why does `qplot(x, y)` work in my R console/terminal? This is because when you type commands in R and hit `Enter`, the objects will be _implicitly_ printed. So the correct way of using `lattice`/`ggplot2` graphics is:


    
    <<fig=TRUE, width=5, height=4>>=
    library(ggplot2)
    print(qplot(x, y))
    ## or
    p = qplot(x, y)
    print(p)
    @


This is different from base R graphics, which do not need to be explicitly printed, e.g. `hist(x)` is enough (no need to `print(hist(x))`).
I hope this is useful to the community. Please feel free to report problems during your installation and configuration.
