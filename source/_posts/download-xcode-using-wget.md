---
title: 使用wget下载Xcode
date: 2016-03-29 14:53:26
tags:
- Xcode
- wget
- download
category: Xcode
---

自从Xcode出现了那次XcodeGhost事件后，便只能通过AppStore上进行更新了，然而由于种种原因，AppStore经常上不去，即使连上了也经常会有坑爹的网络问题，所以如果能下载dmg当然是更好的。

方法也很简单，就是通过wget等命令行下载工具直接下载，支持断点续传，稳定方便，比AppStore好多了。

首先需要做的就是找到下载链接:

[https://developer.apple.com/services-account/download?path=/Developer_Tools/Xcode_7.3/Xcode_7.3.dmg](https://developer.apple.com/services-account/download?path=/Developer_Tools/Xcode_7.3/Xcode_7.3.dmg)

用Chrome打开这个链接会弹出Apple Developer账户登录的信息，然后会重定向回开发者主页，第二次打开这个页面，便会弹出下载对话框，但是用Chrome下载可能会面临网络不稳定的情况，我们右键页面选择检查，打开Chrome调试界面，选择Network， 选择download?path=...开头的连接，如图所示：

![调试界面](http://7xrxpb.com1.z0.glb.clouddn.com/xcode_getcookie.jpeg)

在Response Headers中，我们可以获得location，即实际的下载链接。在Request Header中，我们找到cookie中的ADCDownloadAuth字段。

只需要有location和ADCDownloadAuth我们就可以用wget进行下载了。

命令如下：

```
wget --header "Cookie:ADCDownloadAuth=ZbmIKnE9W%2FhUDmqp9hYPVdeKuzB3VwGEUrEhZn%2BDQtF4Wg9DJiVonlD2Qi7gb7m3ZRLlBQdx02Oq%0D%0AV9qpH2t4WHYq20FxlxKMm7xFJZwEJpg1zXHh08rCVh6bZFBu7J9nfOmkwt21m4esehS0jzrwu%2Fgf%0D%0As5S2EUVdfyfStzkCL3QnTRmo%0D%0A" http://adcdownload.apple.com/Developer_Tools/Xcode_7.3/Xcode_7.3.dmg
```

附上下载截图：

![下载截图](http://7xrxpb.com1.z0.glb.clouddn.com/xcode_wget.png)

喝杯咖啡，静静等候就好啦:-)

(这也方法也适用于下载其他Developer工具，只需要知道Request URL。如下载Xcode7.2.1.dmg只需要把Xcode7.3替换成7.2.1即可。)
