---
published: true
layout: post
title: "Error in saveGIF() and im.convert() in animation package"
author: Yu
categories: R-Language
tags:
- animation
- R
- saveGIF
- im.convert
---

There is a bug at the latest version of animation package, `im.convert()` can not generate gif under some OS.

The problem is that <q>R cannot find the executable file of convert</q>.

We fix it a few days ago，you can see the [issue report at here](https://github.com/yihui/animation/issues/71).

I test it under Vista, Windows7, Fedora. Removing the extra quotes also help `saveGIF()` generate the gif.

Some isssue reports like [Problem with ``saveGIF`` on Windows](https://github.com/yihui/animation/issues/67) and [Removing excess shQuote in im.convert for Windows](https://github.com/yihui/animation/issues/46) are associated with this problem.

I write the post and want to find some one can help us to test if the `saveGIF()` works well under Windows.

Here is the test code.

```r
library(devtools)

dev_mode(on=T)

install.packages('animation', repos = 'http://yihui.name/xran')
library(animation)
saveGIF({
  for (i in 1:10) plot(runif(10), ylim = 0:1)
})

dev_mode(on=F)
```

If you find any other problem when using the development version, please leave us a message at [github](https://github.com/yihui/animation/issues).

