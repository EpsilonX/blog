---
title: 如何改变Xcode的编辑器字体
date: 2016-03-16 15:53:40
tags:
- Xcode
- font
category: Xcode
---

Xcode本身的字体大小默认只有11，如果在iMac下做开发，人眼看得会很累，那么如何更改Xcode编辑器的字体呢？

1. 打开Xcode-Preferences-Fonts&Colors，点下方+号，选择从原本的数十个模板里新建一个自己的新模板，比如我的就叫做Midnight Extended，这样可以避免破坏原始的主题
![选择主题](http://7xrxpb.com1.z0.glb.clouddn.com/xcodeaddNewTheme.jpeg)

2. 在SourceEditor中选中任意文本（如PlainText），然后按⌘(cmd)+A选中所有文本，这样下方的"T"按钮应该会从灰色变为可以点击，点击后就可以弹出字体选择框了，我选的是*OSX 10.11*推出的**苹方-简**字体，字号选14号
![选择文本](http://7xrxpb.com1.z0.glb.clouddn.com/xcodeselectText.jpeg)

3. 最终效果如图所示，是不是比默认的好看多了？当然大家也可以根据自己的喜好自由定制~
![最终效果](http://7xrxpb.com1.z0.glb.clouddn.com/xcodeeditorView.jpeg)
