---
title: 搭建 Python 服务容器运行环境
date: 2018-04-29
tags: 
- Docker
- Python
- Hexo
categories: Docker
---

> 关于部署 Docker 的 Python HTTP 服务容器运行环境笔记

<!-- more -->

# 背景

- 为了运行个人 Blog 服务，选用 Python 的 HTTP 服务；
- 为了方便在内网部署，选用 Docker 的容器环境

# 步骤

1. 运行容器

```shell
docker run -itd \
--name test-hexo \
-p 20002:8000 \
--restart=always \
python \
bin/bash
```

1. 进入容器

```shell
docker exec -it test-hexo /bin/bash
```

1. 在容器内 clone 代码

```shell
cd ~
git clone http://********:5006/sh.shen/my-hexo.git
```

1. 运行pyhton测试

```shell
cd my-hexo
python -m http.server
```

1. 自动执行命令:

`~/.bashrc` 加入

```shell
cd /root/my-hexo/
python -m http.server
```

---