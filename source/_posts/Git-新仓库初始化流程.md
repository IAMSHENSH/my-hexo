---
title: Git 新仓库初始化流程
date: 2017-09-11
tags: 
- git
- submodule
- note
categories: Git
---

> 关于使用 Git 初始化新仓库（同时下载子模块）的流程。

<!-- more -->

1. 登录代码托管平台,  添加本地「SSH 密钥」；

2. clone「project-a」仓库

```shell
$ git clone ssh://git@********:10023/cartronics/project-a.git
$
```

1. 进入「project-a」目录；

```shell
cd project-a
```

1. 初始化与更新子仓库

```shell
$ git submodule init
$ git submodule update
$
```

1. 进入子仓库

```shell
$ cd moddule-a/
$
```

1. 切换到自己工作分支，下面选的是「branck-a」分支

```shell
$ git checkout branck-a
$
```

---
