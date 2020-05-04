---
published: ture
layout: post
title: "How to convert webm to gif on Linux"
author: Yu
categories: HowTo
tags:
- webm
- gif
- GIMP
---


`webm` is an audiovisual media file format. It is primarily intended to offer a royalty-free alternative to use in the HTML5 video and the HTML5 audio elements. It has a sister project WebP for images. On Fedora system, you could use the [Gnome's embedded screencast tool](https://fedoraproject.org/wiki/ScreenCasting) to create a 30 seconds video of your screen by default. What I want is a `gif` animation that would be easier to be transferred and shown online.

So how to convert `webm` file to `gif` file on Linux?

The best way is using ffmpeg [^1].

```bash
ffmpeg -y -i input.webm -vf palettegen palette.png
ffmpeg -y -i input.webm -i palette.png -filter_complex paletteuse -r 10 output.gif
```
After that I also recommend using GNU Image Manipulation Program (GIMP) to crop off the unwanted part in the animation, it is also a way to reduce the animation size.


[^1]: [How to do I convert an webm (video) to a (animated) gif on the command line?](https://askubuntu.com/questions/506670/how-to-do-i-convert-an-webm-video-to-a-animated-gif-on-the-command-line)

