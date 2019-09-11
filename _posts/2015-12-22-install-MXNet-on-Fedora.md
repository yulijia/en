---
published: true
layout: post
title: "How to install MXNet on Fedora"
author: Yu
categories: HowTo
tags:
- MXNet
- lcblas
---

The [Offical Installation Guide](http://mxnet.readthedocs.org/en/latest/build.html) has not mentioned any instruction about how to install MXNet on Fedora/RedHat/CentOS.

This is because some dependencies library names are different compared to Ubuntu/Debian.

I installed MXNet successfully on Fedora 23 and wrote the guide to help me remember the installation steps.


### 1.Install dependencies

~~~
dnf install opencv-devel atlas-devel 
# make sure /usr/bin/ld  can find these library
cp /usr/lib64/atlas/* /lib64/ 
~~~

You may also need to install gcc, git and other [build tools](http://unix.stackexchange.com/questions/1338/what-is-the-fedora-equivalent-of-the-debian-build-essential-package "build essential package")

### 2.change the compilation command

If you follow the Official Guide, you may get an error when doing 'make' files.

> /usr/bin/ld: cannot find -lcblas

This is because [names and content of atlas libraries have changed](https://www.centos.org/forums/viewtopic.php?f=47&t=48543).

> chemal:
>
> Names and content of atlas libraries have changed recently. Try
>
> -L/usr/lib64/atlas -lsatlas or -L/usr/lib64/atlas -ltatlas
>
> instead of -lcblas.

So, We need change the complication command from `-lcblas` to `-lsatlas` or `-ltatlas` at `mshadow/make/mshadow.mk, line 57` 

For me, I use `-lsatlas`:

~~~
MSHADOW_LDFLAGS += -lsatlas 
~~~

### 3.make -j4

Then you can make files successfully.

### 4.Try Python version with CPU

I try using example/neural-style/run.py to draw a picture.

First, run download.sh to download the model and input files.

The script need import kitimage package. 

~~~
pip install scikit-image
~~~

Install Python package as the [guide mentioned](http://mxnet.readthedocs.org/en/latest/build.html#python-package-installation).

I always install package system-widely, which requires root permission.

~~~
cd python; sudo python setup.py install
export PYTHONPATH=~/mxnet/python
~~~

Finally, run command 

~~~
python run.py --content-image input/1.jpg --style-image input/starry_night.jpg --gpu -1 ##using CPU
~~~

You can get a neural style image. I post the [SDTOUT INFO at gist](https://gist.github.com/yulijia/032afa9a9123b7643522).

![Imgur](https://i.imgur.com/P86I0F0.jpg)
