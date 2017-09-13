---
title: Docker-Image-导出载入
date: 2017-09-11 
tags: 
- docker
- load
- export
categories: Docker

---

> 关于使用 Docker 对镜像进行导出与载入的方法

<!-- more -->

## Docker 镜像进行导出与载入命令

1. 将外网的镜像导出
```
docker save [IMAGE ID]  > [IMAGE NAME].tar 
```

2. 在内网导入镜像
```
docker load < [IMAGE NAME].tar
```

3. 更改镜像的名字和标签
```
docker tag [IMAGE ID] [IMAGE NAME]:[TAG]
```

## 其他命令

1. 容器的导出
```
docker export [CONTAINER ID] > [NAME].tar
```


