---
title: "git 同时 push 至多个仓库"
date: 2018-01-03T10:31:51+08:00
draft: true
slug: "git-Tong-Shi-push-Zhi-Duo-Ge-Cang-Ku"
tags: ["git"]
category: ["git"]
---

今天想把个人博客同时推送至 GitHub 和个人私有 Git，当使用

```
git remote add origin https://github.com/HJDev/Xday.git
```

添加仓库时，提示错误：

```
fatal: remote origin already exists.
```

根据错误提示，我们可以知道，origin 仓库已经存在，所以我们需要更改仓库名称,(如:origin1):

```
git remote add origin1 https://github.com/HJDev/Xday.git
```

然后再push 到仓库。

```
git push -u origin1 
```

<!-- more -->

### 终极秘籍

> 身为攻城狮的我们，一定不会满足与使用重复的体力来解决毫无意义的体力劳动

#### 使用一条命令同时推送至多个仓库

编辑配置文件

```
vim .git/config 
```

```
[remote "all"]
        url = http://git.teamleader.cn/hejun/blog.git
        url = https://github.com/HJDev/Xday.git
```

保存。

使用命令：

```
git push all
```

Done !