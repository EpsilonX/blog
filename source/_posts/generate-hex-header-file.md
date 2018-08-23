---
title: xxd命令生成十六进制头文件
date: 2018-08-20 16:09:54
tags:
- xxd
- file
- bundle
category: tools
---

# xxd命令是什么？

```bash
xxd [options] [infile] [outfile]
```

xxd命令用于二进制或十六进制显示文件的内容，本文将重点讲下`xxd -i`这个命令的使用。

> -i | -include
>
> output in C include file style. A complete static array  definition  is  written (named after the input file), unless xxd reads from stdin.

`xxd -i` 可以把指定的二进制文件转换成C的头文件，那么这样做有啥用呢？

# 应用场景

在实际开发中，特别是SDK开发者，对于一些图片资源文件，常常需要建立一个bundle来打包这些图片，如果是静态库，提供给第三方时，需要一并带上bundle，有时候一个bundle里可能就几张图，有没有更好的方式不需要用bundle来存这些图呢？

当然是有的，我们就利用`xxd -i abc.jpg abc.h`命令，将二进制的图片文件转成头文件，那么就不需要用bundle来打包图片了，只需要将这些图片生成的头文件放入项目中即可。

生成的头文件是类似这样的结构：

```c
unsigned char abc_png[] = {
  0x89, 0x50, 0x4e, 0x47, 0x0d, 0x0a, 0x1a, 0x0a, 0x00, 0x00, 0x00, 0x0d,
  0x49, 0x48, 0x44, 0x52, 0x00, 0x00, 0x00, 0x3c, 0x00, 0x00, 0x00, 0x3c,
  0x08, 0x06, 0x00, 0x00, 0x00, 0x3a, 0xfc, 0xd9, 0x72, 0x00, 0x00, 0x00,
   .... //省略无数行
  0xf7, 0xf0, 0xd5, 0x19, 0xf4, 0x00, 0x00, 0x00, 0x00, 0x49, 0x45, 0x4e,
  0x44, 0xae, 0x42, 0x60, 0x82
};
unsigned int abc_png_len = 1541;
```

调用方法如下:

```objc
#import "abc.h"
...
NSData *imgData = [NSData dataWithBytesNoCopy:abc_png length:abc_png_len freeWhenDone:NO];
UIImage *image = [UIImage imageWithData:imageData];
...
```

# 优缺点

优点：

- 无需新建bundle来打包图片，使得静态库的使用更为方便

缺点：

- 每个图片都需要生成一个头文件，当然也可以手动把生成的内容汇总在同一个文件里，但这样可读性会变差

- 头文件的大小大约是原始PNG图片的十倍左右，对于一些小的icon比较适用。
