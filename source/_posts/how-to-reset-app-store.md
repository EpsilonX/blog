---
title: 如何重置App Store 
date: 2016-02-23 12:52:55
tags: mac
category: bugfix
---

Mac的App Store有时会出现无法下载软件或是已购项目中显示该下载项目错误之类的提示，解决方法如下：

1. 在终端内输入：
```
defaults write com.apple.appstore.ShowDebugMenu -bool true
```

2. 重新启动AppStore，菜单栏最右会出现Debug栏，选择Reset Application以及Clear Cookies

3. 重新开始下载项目
