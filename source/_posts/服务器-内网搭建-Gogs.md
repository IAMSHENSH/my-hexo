---
title: 服务器内网搭建 Gogs
date: 2017-09-11
tags: 
- gogs
- 内网
- docker
- note
categories: 服务器
---

> 关于在服务器内网搭建 Gogs 的方法

<!-- more -->


## 加载 gogs/gogs 镜像
> 具体步骤可参考 「docker-image-导出载入.md」

1. 在外网下载此镜像；

1. 搬运到内网虚拟机中；

1. 在虚拟机载入镜像；
```
$ docker load < gogs-gogs.tar
$ docker tag 245abb5da84e gogs/gogs:latest
```

## 部署 Gogs

创建一个本地文件，用于储存 Gogs 的重要文件 
```
$ mkdir -p /opt/mydisk/gogs
```

使用 docker run 初始化启动容器
```
$ docker run --name=gogs -d  \
-p 10023:22 -p 10083:3000  \
-v /opt/mydisk/gogs:/data \
gogs/gogs 
```

如果容器停止，使用 docker start 重现开启
```
$ docker start gogs 

```
