---
layout: post
title: Flash Animations by saveSWF() in Package animation
categories:
- Computer Science
tags:
- Animation
- Flash
- R Package
- SWF
---

The [SWF Tools](http://www.swftools.org/) has provided several SWF utilities for the manipulation and creation of Flash files. Today I just wrote a wrapper `saveSWF()` in the package **animation** to convert image frames to Flash animations. I'd like to thank [Hadley](http://had.co.nz/) for telling me this tool set. The function `saveSWF()` is to appear in `animation 1.0-1`.

Up to now, there are four kinds of animations in the animation package:

1. animations inside R windows graphics devices;
2. animations in HTML pages (driven by JavaScript);
3. GIF or AVI animations with the help of "ImageMagic";
4. Flash animations with the help of "SWF Tools".
