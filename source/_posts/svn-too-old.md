---
title: Mac SVN版本太旧解决办法
date: 2016-02-24 13:12:21
tags: mac
category: bugfix
---

Mac(El Captain)自带的SVN版本是1.7.20，而最新(2016.2.24)的SVN是1.9.3，用`brew install svn`装了最新的SVN，然而用`svn --version`出来的还是1.7.20。

解决办法就是将homebrew安装的svn路径加入到PATH中，具体方法如下：

1. 找到brew安装svn的bin目录：`/usr/local/Cellar/subversion/1.9.3/bin`

2. 在~/.bash_profile中将其加入到PATH中（若新机器没有bash_profile则新建一个）:
```
export PATH="/usr/local/Cellar/subversion/1.9.3/bin:$PATH"
```

3. 编译bash_profile: `source ~/.bash_profile`
4. 查看svn位置: `which -a svn`(-a可以看到PATH中所有svn的位置)、
5. 检查版本号: `svn --version`

通过其他方式安装的svn也可以通过类似方法解决。
