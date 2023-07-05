---
published: ture
layout: post
title: "How to update all R packages after installing a new version of R?"
author: Yu
categories: HowTo
tags:
- update R packages
---

Many R users are unfamiliar with the process of updating all their R packages after upgrading to a new version of R.

Would you like to learn how to update all R pacakges in a safe and straightforward way?

*The method I described below should work for all platforms, however, I didn't use R in Windows, so I don't know the specific directory path, I will describe the steps using a Linux system.*

For example, I upgraded R from 4.2.1 to 4.3.1 recently.

As we can see in the R library folder, there are two sub-folders, one is 4.2 (`/home/ylj/R/x86_64-pc-linux-gnu-library/4.2`) and the other is 4.3 (`/home/ylj/R/x86_64-pc-linux-gnu-library/4.3`).

First, we need to copy all the packages from the `4.2` folder to the `4.3` folder. Alternatively, it is also possible to perform the reverse operation, but in that case, the folder needs to be renamed.

Then, in R terminal/Rstudio run each line below.

```{R}
## load all installed packages
pkglist=data.frame(installed.packages(lib.loc="/path/to/lib")) # at here, lib.loc="/home/ylj/R/x86_64-pc-linux-gnu-library/4.3"
## update all packages
BiocManager::install(pkglist$Package,update=TRUE,ask=FALSE,checkBuilt=TRUE)
## reload new Rsession
## check if there are any packages that weren't updated
pkgupdated=data.frame(installed.packages(lib.loc="/path/to/lib"))
pkgfailed=pkgupdated[pkgupdated$Built<"4.3.1",]
```

For all packages in the `pkgfailed`. we have to install them manually.

