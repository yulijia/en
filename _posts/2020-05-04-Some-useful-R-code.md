---
published: ture
layout: post
title: "Some useful R code"
author: Yu
categories: "R-Language" 
tags:
- R
---

This is an Attic to store some useful R code, I don't want to search them every time, so I put these code at here.

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


TBC
