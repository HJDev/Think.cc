---
title: "Ubuntu 16.0.4 安装 Swift 后提示 error while loading shared libraries: libpython2.7.so.1.0"
date: 2017-09-28T16:58:49+08:00
draft: false
slug: "ubuntu-16-0-4-an-zhuang-swift-hou-ti-shi-error-while-loading-shared-libraries-libpython2-7-so-1-0"
---

Ubuntu 16.0.4 安装 Swift 后提示: error while loading shared libraries: libpython2.7.so.1.0:

```
swift/usr/bin/lldb: error while loading shared libraries: libpython2.7.so.1.0: cannot open shared object file: No such file or directory
```

这个问题会在 Ubuntu 14.04 和 Ubuntu 16.04 上出现，是 swift 的一个依赖问题，只需要安装 `libpython2.7-dev` 就可以解决问题。代码如下

```
sudo apt-get install libpython2.7-dev
```

### Done !

Link : [Incomplete install instructions for Ubuntu](https://bugs.swift.org/browse/SR-2743)
原文链接: [Ubuntu 16.0.4 安装 Swift 后提示 error while loading shared libraries: libpython2.7.so.1.0](https://www.teamleader.cn/ubuntu-16-0-4-an-zhuang-swift-hou-ti-shi-error-while-loading-shared-libraries-libpython2-7-so-1-0/)
