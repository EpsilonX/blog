---
title: iOS开发中macro的使用
date: 2016-03-11 19:52:37
tags:
- iOS
- Objective-C
- Macros
category:
- iOS开发
---

在iOS开发中，常常会用到宏(Macro)来灵活定义一些常量或是函数，下面就罗列一些宏的常用方法。

1. \#define
define是最常用的宏命令，用以定义常量或函数，在实际使用中，编译器用宏中`#define A B`的B替换A

2. \#ifdef/\#ifndef
`#ifdef A`表示如果A已被定义，而`#ifndef A`表示如果A未定义。后者常用来解决重复定义的问题，这两个宏都需要在下一行用`#endif`来结束

3. \#if/\#elif/\#else/\#endif
`#if A`或者`#if A 1`是宏中的条件判断语句，也需要用`#endif`结束

4. do {...} while (0)
使用do {...} while (0)构造的宏不会受到大括号、分号的影响，总是会按照期望的方式运行，而`#define A(...) do {} while (0)`可以使A(...)失效，即什么也不做

5. 多行
宏支持多行，只需要在行末尾用`\`即可
