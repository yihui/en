---
layout: post
title: Enhanced tidy.source() (Preserve Some Comments)
categories:
- R language
tags:
- parse()
- R code
- R Language
- tidy.source()
---

After a few hours' work, I modified the function `tidy.source()` in the [animation](http://cran.r-project.org/web/packages/animation/index.html) package so that it can preserve _complete_ comment lines. See the [`tidy.source()` wiki page](http://animation.yihui.name/animation:misc#tidy_up_r_source) for example.

[notice type=download][Downdload the R code here](http://yihui.name/en/wp-content/uploads/1238508996_0.r)[/notice]

    
    tidy.source <- function(source = "clipboard", keep.comment = TRUE,
      keep.blank.line = FALSE, begin.comment, end.comment, ...) {
      # parse and deparse the code
      tidy.block = function(block.text) {
          exprs = parse(text = block.text)
          n = length(exprs)
          res = character(n)
          for (i in 1:n) {
            dep = paste(deparse(exprs[i]), collapse = "\n")
            res[i] = substring(dep, 12, nchar(dep) - 1)
          }
          return(res)
      }
      text.lines = readLines(source, warn = FALSE)
      if (keep.comment) {
          # identifier for comments
          identifier = function() paste(sample(LETTERS), collapse = "")
          if (missing(begin.comment))
            begin.comment = identifier()
          if (missing(end.comment))
            end.comment = identifier()
          # remove leading and trailing white spaces
          text.lines = gsub("^[[:space:]]+|[[:space:]]+$", "",
            text.lines)
          # make sure the identifiers are not in the code
          # or the original code might be modified
          while (length(grep(sprintf("%s|%s", begin.comment, end.comment),
            text.lines))) {
            begin.comment = identifier()
            end.comment = identifier()
          }
          head.comment = substring(text.lines, 1, 1) == "#"
          # add identifiers to comment lines to cheat R parser
          if (any(head.comment)) {
            text.lines[head.comment] = gsub("\"", "\'", text.lines[head.comment])
            text.lines[head.comment] = sprintf("%s=\"%s%s\"",
              begin.comment, text.lines[head.comment], end.comment)
          }
          # keep blank lines?
          blank.line = text.lines == ""
          if (any(blank.line) & keep.blank.line)
            text.lines[blank.line] = sprintf("%s=\"%s\"", begin.comment,
              end.comment)
          text.tidy = tidy.block(text.lines)
          # remove the identifiers
          text.tidy = gsub(sprintf("%s = \"|%s\"", begin.comment,
            end.comment), "", text.tidy)
      }
      else {
          text.tidy = tidy.block(text.lines)
      }
      cat(paste(text.tidy, collapse = "\n"), "\n", ...)
      invisible(text.tidy)
    }


Note that inline comments will still be removed. I don't want to spend more time on dealing with inline comments any more.
