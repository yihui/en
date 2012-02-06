---
layout: post
title: Sweave and pgfSweave in LyX 2.0.x (experimental)
categories:
- Computer Science
- R language
tags:
- Literate Programming
- LyX
- pgfSweave
- R Language
- Sweave
---

[notice type=alert]Please ignore this post completely, because Sweave support has become mature in LyX since 2.0.2, and I'm not going to add the pgfSweave module in LyX. For pgfSweave users, you may consider the new knitr module (available since 2.0.3) which uses the R package [knitr](http://cran.r-project.org/package=knitr).[/notice]

About half a year ago, I wrote [a post on the configuration of (pgf)Sweave and LyX](http://yihui.name/en/2010/10/how-to-start-using-pgfsweave-in-lyx-in-one-minute/), which was intended to save us some efforts in going through all the details during the configuration. Now many things have changed: LyX 2.0 has internal support for Sweave, and fortunately I have been in touch with the developers on this feature (thanks to [Gregor](http://ggorjan.blogspot.com/)); meanwhile, there have also been many changes in the [pgfSweave](http://cran.r-project.org/package=pgfSweave) package. In all, we have a number of new features which we should definitely make use of.


## New Features


A list of new features as far as I can remember:



	
  1. support for Sweave in LyX 2.0 is internal, so there is no need to modify the preferences file manually (the converters have been defined internally)

	
  2. most importantly, Sweave becomes an independent module now in LyX, which means you can use it with arbitrary layouts!

	
  3. we can see the messages during compilation in LyX 2.0 (View-->View Messages), which is really really helpful and I would strongly recommend you to turn on this option when compiling Sweave documents, because you will know which code chunk goes wrong in case of any errors (in the past, you only got an annoying error dialog box which told you almost nothing about the error)

	
  4. pgfSweave is faster: it uses the GNU make utility to compile graphics, and you can use multi cores if you like; the compilation becomes 3 steps (pdflatex, make, then pdflatex); other nice features include: the R code is put in an environment Hinput now so you can customize it in LaTeX preamble; there will be no longer a huge gap between the R code and the output ([fixed by Liang Qi](https://github.com/cameronbracken/pgfSweave/pull/26))...

	
  5. tikzDevice has better support for multi-byte characters (using UTF8)


I have been working on improving the Sweave module and adding a new pgfSweave module to LyX, and now I have basically finished what I planned to do. See [the ticket #7555 for details](http://www.lyx.org/trac/ticket/7555). To sum up,



	
  1. LaTeX will not complain about not being able to find Sweave.sty; I used several tricks to guarantee this -- even in the worst case, LaTeX can still use the hard-coded Sweave style;

	
  2. Spaces and dots in path names or filenames will no longer be a problem;

	
  3. the pgfSweave module is also working now;

	
  4. you can export the reformatted R code in a LyX document with the pgfSweave module;


I have also tried to document all the cool bells and whistles in two examples, [sweave.lyx](http://www.lyx.org/trac/raw-attachment/ticket/7555/sweave.lyx) and [pgfsweave.lyx](http://www.lyx.org/trac/raw-attachment/ticket/7555/pgfsweave.lyx). Or you can directly read the PDF documents, [sweave.pdf.tar.xz](http://www.lyx.org/trac/raw-attachment/ticket/7555/sweave.pdf.tar.xz) and [pgfsweave.pdf](http://www.lyx.org/trac/raw-attachment/ticket/7555/pgfsweave.pdf).


## Try the (pgf)Sweave Module


It seems several people are interested in testing the two modules, and it is actually very easy under Linux. So far I have had no luck with Windows to build LyX from source (I tried once; it took me days to compile and ended up with errors).



	
  1. check out the source code of LyX: `svn co svn://svn.lyx.org/lyx/lyx-devel/trunk lyx-devel`

	
  2. (`cd lyx-devel`) apply my patch [sweave-patch.diff](http://www.lyx.org/trac/raw-attachment/ticket/7555/sweave-patch.diff) to the svn source you checked out just now: `patch -p0 -i /path/to/sweave-patch.diff`

	
  3. build LyX

    
    ./autogen.sh
    ./configure
    make
    sudo make install





Done.

Currently the patch is still waiting on LyX Trac. If you run into any problems before the developers begin to look at the patch, please let me know and we will try to make the two modules more stable and useful. But first of all, please remember to keep all your software packages up-to-date: R 2.13.0, pgfSweave 1.2.1 (run `update.packages()` in R as frequently as you can) and LaTeX package pgf 2.10 (very important for externalization). 


## For LyX 1.6.x Users


If you have been using Sweave since LyX 1.6.x and followed the approach in my [previous blog post](http://yihui.name/en/2010/10/how-to-start-using-pgfsweave-in-lyx-in-one-minute/), and you happen to be a Linux user as well, I apologize for the inconvenience: you probably need to remove all the old configurations. One way is to rename `~/.lyx/`:

    
    mv ~/.lyx ~/.lyx16


Then reconfigure LyX. If you have important configurations in the `preferences` file and the `layouts` directory under `~/.lyx/`, you need to remove all the Sweave-related configurations (including the converters and format definitions), and also delete all the `literate-*.layout` files under the `layouts` directory. Then reconfigure LyX.


## For Windows Users


Compiling LyX from source under Windows was pain for me, so the above approach might not work well for Windows users. Anyway, manual configuration is not terribly hard, as long as you understand how a LyX module works.

First we need to define a new format for pgfSweave (Tools --> Preferences; do exactly as the below screenshot shows, and note the upper or lower cases):

[caption id="attachment_751" align="aligncenter" width="587" caption="Define the pgfSweave format"]![Define the pgfSweave format](http://yihui.name/en/wp-content/uploads/2011/05/lyx-pgfsweave-format.png)[/caption]

Then we tell LyX how it should deal with such a format by defining a converter (e.g. select pgfSweave from the left drop-down list and LaTeX (plain) from the right list, then click the Add button; depending on your output type, you may need several similar converters):

[caption id="attachment_752" align="aligncenter" width="606" caption="pgfSweave converters"]![pgfSweave converters](http://yihui.name/en/wp-content/uploads/2011/05/lyx-pgfsweave-converter.png)[/caption]

Next we define a module to use the above format and converter, and an R script to do the real job of Sweaving. Download [pgfsweave.module](http://gitorious.org/yihui/lyx2-sweave/blobs/raw/master/pgfsweave.module) and [lyx-pgfsweave.R](http://gitorious.org/yihui/lyx2-sweave/blobs/raw/master/lyx-pgfsweave.R). Go to your installation directory of LyX, put `pgfsweave.module` under `Resources/layouts` and `lyx-pgfsweave.R` under `Resources/scripts`. Reconfigure LyX (Tools --> Reconfigure) and you are done.

You can test with my example [pgfsweave.lyx](http://www.lyx.org/trac/raw-attachment/ticket/7555/pgfsweave.lyx).
