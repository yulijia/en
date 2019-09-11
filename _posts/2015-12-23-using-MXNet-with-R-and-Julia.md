---
published: true
layout: post
title: "using MXNet with R and Julia"
author: Yu
categories: HowTo
tags:
- R
- Julia
- MXNet
---

## MXNet with R

I couldn't install pre-built binary package, the pre-built binary package is avaliable for Windows and Mac users.

So we need  build the R package first when we try to install it. 
[Just follow the R package install guide for linux user](http://mxnet.readthedocs.org/en/latest/build.html#r-package-installation). 

I successful install the package.


If you find this error message when you try to install dependencies of R package.

> ../inst/include/CImg.h:368:19: fatal error: fftw3.h: No such file or directory

Try install fftw-devel

~~~
dnf install fftw-devel
~~~

## MXNet with Julia

To install mxnet.jl, you may face the same problem as [install MXNet on Fedora](http://yulijia.net/en/howto/2015/12/22/install-MXNet-on-Fedora.html).

> LoadError: Provider BinDeps.PackageManager failed to satisty dependency cblas

![Imgur](https://i.imgur.com/m5dIXwG.png)

If you already copy libsatlas or libtatlas to /usr/lib64/.

I suggest you make a soft link as below:

~~~
ln -s /usr/lib64/libsatlas.so.3 /usr/lib64/libcblas.so
~~~

Then try `Pkg.rm` and `Pkg.add` again.

If you install MXNet in local environment, and julia can not find header files, to installing mxnet.jl would re-install the mxnet in the .julia folder.
