---
published: true
layout: post
title: "run sciClone without rgl package"
author: Yu
categories: R-Language
tags:
- R
- sciClone
- rgl
---

![rgl_package_problem](https://i.imgur.com/tFHoydW.png)

Maybe you have met the problem above when apply `sciClone` package with a server without libGLU (X server).

For me, our lab use a cluster can not connect to outside network and without X-server. When apply `sciClone` package, it would got a error message like,

~~~
.onLoad filed in loadNamespace() for 'rgl'  
libGLU.so.1 can not open shared object file 
~~~

The fast way to fix this problem is, if you do not need `sc.plot3d()` function, just remove it, then you can run other subfunction in this package successfully.

If you do not know how to remove the `sc.plot3d()` function, you can [download](https://github.com/yulijia/sciclone/archive/master.zip "download sciClone package without rgl dependency") a modified sciClone package from my repository or install the modified package using `install_git()`

~~~
install_github("sciclone",username="yulijia",ref = "master")
~~~
