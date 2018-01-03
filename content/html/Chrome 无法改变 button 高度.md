---
title: "Chrome 无法改变 button 高度"
date: 2018-01-03T11:10:49+08:00
draft: false
slug: "Chrome-Wu-Fa-Gai-Bian-button-Gao-Du"
tags: ["html"]
category: ["html"]
---

今天查看[中文转拼音页面](http://xday.cn/convert.html)时,发下转换按钮的样式变成了默认样式了，重新再 Safari 打开，显示样式正常，怀疑是不是 Chrome 最近有改变对 css 的样式支持，Google 到高手秘籍：

<!-- more -->

源代码：

```
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8"/>
    <title></title>
    <style type="text/css">
    .set{
        height: 40px;
        width: 90px;
        }
    </style>
</head>
<body>
<input class="set" type="button" value="点击设置"/>
</body>
</html>
```

### 解决方法一：

把 input 改为 button : 

```
<button class="set" type="button" value="点击设置"/></button>
```

### 解决方法二:

在css中增加一个参数(input button 类型，只能用 class 来修改样式)；

```
<style type="text/css">
.set{
    -webkit-appearance:button;
    height: 40px;
    width: 90px;
    }
</style>
```

参考链接：

[［css］button的高度无法改变](https://segmentfault.com/q/1010000004034911)