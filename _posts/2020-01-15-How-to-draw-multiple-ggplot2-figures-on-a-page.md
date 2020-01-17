---
published: ture
layout: post
title: "How to draw multiple ggplot2 figures on a page"
author: Yu
categories: HowTo
tags:
- R
- ggplot2
---

There are multiple ways to layout graphs on one page with R[^1][^2]. I prefer using `grid.arrage` function in `gridExtra` package.
 
The `widths` and `heights` options in the `grid.arrage` function will help you to arrange graphs with different size. Using `do.call` function may help you to pass all figures (in the list) to `grid.arrange` function.


Here is a short example of it.

### 1.With same figure size

```r
library(ggplot2)
library(gridExtra)
p <- list()
p[[1]]<- ggplot(mtcars, aes(wt, mpg))+geom_point()
p[[2]] <-  ggplot(ChickWeight, aes(x=Time, y=weight, colour=Diet, group=Chick)) +
  geom_line() +
  ggtitle("Growth curve for individual chicks")

p[[3]] <- ggplot(mtcars, aes(x = factor(cyl), fill = factor(am)))+geom_bar()
p[[4]] <- ggplot(subset(ChickWeight, Time==21), aes(x=weight, fill=Diet)) +
  geom_histogram(colour="black", binwidth=50) +
  facet_grid(Diet ~ .) +
  ggtitle("Final weight, by diet") +
  theme(legend.position="none")

do.call("grid.arrange", c(p,ncol=2)) 
```

![multiple-plots-on-one-page](https://i.imgur.com/EkZR5ns.png)

### 2.With different figure size

```r
grid.arrange(p[[1]],p[[2]],p[[3]],p[[4]],
             ncol=2, nrow=2, widths=c(4, 2), heights=c(3, 4))
```
![multiple-plots-on-one-page-with-different-size](https://i.imgur.com/wpiriD1.png)



[^1]: [Laying out multiple plots on a page](https://cran.r-project.org/web/packages/egg/vignettes/Ecosystem.html)
[^2]: [ggplot2 - Easy Way to Mix Multiple Graphs on The Same Page](http://www.sthda.com/english/articles/24-ggpubr-publication-ready-plots/81-ggplot2-easy-way-to-mix-multiple-graphs-on-the-same-page/)
