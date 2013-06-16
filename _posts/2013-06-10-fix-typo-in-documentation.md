---
layout: post
title: You Do Not Need to Tell Me I Have A Typo in My Documentation
categories: [R language, Computer Science]
tags: [GIT, Documentation, Pull Requests]
---

![help me with Github pull requests](http://i.imgur.com/j9NYXiQ.png)

So I just got yet yet another
[comment](http://yihui.name/knitr/demo/sweave/#comment-924948061) saying
"you have a typo in your documentation". While I do appreciate these kind
reminders, I think it might be a good exercise for those who want to try GIT
and [Github pull
requests](https://help.github.com/articles/using-pull-requests), which make
it possible for you to contribute to open source and fix obvious problems
_with no questions being asked_ -- just do it yourself, and send the changes
to the original author(s) through Github.

The official documentation for Github pull requests is a little bit verbose
for beginners. Basically what you need to do for simple tasks are:

1. click the `Fork` button and clone the repository in your own account;
1. make the changes in your cloned version;
1. push to your repository;
1. click the `Pull Request` button to send a request to the original author;

For trivial changes, sometimes I accept them on my cell phone while I'm
still in bed. No extra communication is needed.

Occasionally I see reports of this kind of trivial documentation changes in
the R-devel mailing list, and I believe that is just horribly inefficient.
You could have done this quietly and quickly, and the developers could have
merged the changes with a single mouse click. (Oh, okay, well, you know,
SVN, mailing lists, ...)

For the [**knitr** repository](https://github.com/yihui/knitr), it has two
branches: `master` and `gh-pages`. The R package lives in the `master`
branch, and the **knitr** website lives in the `gh-pages` branch. If you
want to fix any problems in the website, just check out the `gh-pages`:

    git checkout gh-pages

All pages were written in Markdown, so edit them with your favorite text
editor. For example, as the above comment pointed out, I omitted a right
parenthesis `)` in `_posts/2012-02-24-sweave.md`, and you just add it, save
the file, write a GIT commit message, push to your repository and send the
pull request.

I know I can do this by myself in five seconds, and it takes me way more
time to write this blog post, but I just want everybody to know how people
with different skill levels can play their roles in software development.

Let's see how many minutes it takes for the pull request to come after I
publish this blog post. Hurry!! :)
