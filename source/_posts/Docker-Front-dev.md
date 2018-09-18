---
title: Docker-搭建前端开发环境
date: 2018-07-10
tags: 
- Docker
- Front
- Yarn
- Nodejs
categories: Docker

---

![前端开发环境题图](http://ovlnfs1rj.bkt.clouddn.com/front-end-dev-title.jpg?imageslim)

> 关于使用 Docker 搭建前端开发环境，容器内包括 Node , Yarn。

<!-- more -->

# 环境配置

1. 启动容器

`docker run -d -it <imageID or imageName> /bin/bash`
> eg: `docker run --name front-dev-env  -d -it centos /bin/bash`

## 1. 容器安装软件

1. 到容器内部
> `docker exec -it <容器名或者ID> /bin/bash`
> eg: `docker exec -it front-dev-env /bin/bash`
1. yum 安装 node , yarn
> 1. 安装node
> `curl --silent --location https://rpm.nodesource.com/setup_10.x | bash -`
> `yum install -y nodejs`
> 2. 安装 yarn
> `curl --silent --location https://dl.yarnpkg.com/rpm/yarn.repo | tee /etc/yum.repos.d/yarn.repo`
> `yum install yarn -y`

## 1. 容器写成镜像

1. 容器提交成镜像
> `docker commit -m "change somth" -a "somebody info" container_id(docker ps -a获取id) 新镜像名字`
> eg: `docker commit -m "install node yarn" -a   "install front dev tool" front-dev-env ssh/front-dev-env`

1. 打包镜像
> `docker save [IMAGE ID]  > [IMAGE NAME].tar`
> eg: `docker save ssh/front-dev-env > front-dev-env.tar`

---

## 使用

1. 服务器加载镜像
> `docker load < [IMAGE NAME].tar`
> eg: `docker load < front-dev-env.tar`

1. 运行容器，日常开发

配置环境变量，启动容器
`docker run -itd -p 50001:22 -v ..:.. <刚才提交的镜像ID>  /bin/bash`
> eg: `docker run --name ssh-front-dev -i -td -p 28080:8080 -v /home/shshen/workspace:/home/shshen/workspace ssh/front-dev-env /bin/bash`

1. 在 WebStorm 上配置 STPF 环境，即可试行远程开发
> 1. WebStorm -> Tool -> Devloyment -> Config
> 2. Add Serve -> Type:SFTP
