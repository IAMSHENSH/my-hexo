---
title: 服务器内网部署 Jenkins
date: 2017-09-11
tags: 
- jenkins
- 内网
- docker
- note
categories: 服务器
---

> 关于在内网服务器部署 Jenkins 的笔记

<!-- more -->


### 一、将外网的 Jenkins 镜像拉入内网；

### 部署 Jenkins 到 Docker；

1. 容器初始化
``` 
sudo docker run --name jenkins -d \
-p 10091:8080 -p 50000:50000 \
-v /opt/mydisk/jenkins:/var/jenkins_home/ \
-u root \
jenkins:latest
```

### 上传 Git 相关插件；

1. 在 Jenkins 官网的插件模块下载插件。
> Git 插件需要依赖许多插件

### 拉取 my-data 仓库；

1. 在 Jenkins 内进行设置；
> 例如：
> Repository URL : http://**************/****/Data.git

### 打包压缩某个目录

1. 使用 linux 的 shell 命令；
> To do something
