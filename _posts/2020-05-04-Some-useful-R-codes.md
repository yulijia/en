---
published: ture
layout: post
title: "Some useful R codes"
author: Yu
categories: "R-Language" 
tags:
- R
---

This is an Attic to store some useful R codes, I don't want to search them every time, so I put these codes at here.

### Check packages if they are installed

```r
check.package <- function(pkg){
  new.pkg <- pkg[!(pkg %in% installed.packages()[, "Package"])]
  if (length(new.pkg)) 
    install.packages(new.pkg, dependencies = TRUE)
  sapply(pkg, require, character.only = TRUE)
}


packages.name <- c("reshape","ggplot2","gridExtra")
check.package (packages.name)

```

### Draw beautiful arrowhead


```r
require(ggplot2)
require(grid)

d = seals[sample(1:nrow(seals), 100),]

ggplot(d, aes(x = long, y = lat)) +
geom_segment(aes(xend = long + delta_long/100, 
                 yend = lat + delta_lat/100),
             arrow = arrow(angle = 10,type="closed"),
             colour=c("black"),
             arrow.fill=c("red"),
             size=0.5) 
```

TBC
