---
title: Git 仓库迁移
date: 2017-09-11 
tags: 
- git
categories: Git

---

> 关于 Git 仓库 迁移的方法

<!-- more -->

# 迁移命令模板

1. 镜像克隆：

```shell
git clone --mirror  https://github.com/../old.git old.git
cd old.git
```

1. 然后推送镜像：

```shell
git remote set-url --push origin git@gitcafe.com/.../new.git
git push --mirror
```

1. 或者推送新建remote再推送：

```shell
git remote add mirror origin git@gitcafe.com/.../new.git
git push mirror --all
git push mirror --tags
```

## project-a 迁移实例

```shell
$ git clone --mirror ssh://git@****:****/****/project-a.git
> 克隆到纯仓库 'project-a.git'...
> remote: Counting objects: 7012, done.
> remote: Compressing objects: 100% (8/8), done.
> remote: Total 7012 (delta 0), reused 0 (delta 0)
> 接收对象中: 100% (7012/7012), 487.89 MiB | 41.89 MiB/s, 完成.
> 处理 delta 中: 100% (2835/2835), 完成.
```

```shell
$ cd project-a.git/
$
```

```shell
$ git remote set-url --push origin ssh://git@****:****/****/project-a.git
$
```

```shell
$ git push --mirror
> The authenticity of host '[server]:10090 ([172.24.***.***]:10090)' can't be established.
> ECDSA key fingerprint is SHA256:nVnsCJaQGoJh0ehUKJ2/v4YQkJIKM2VzadcoUvTyFKc.
> Are you sure you want to continue connecting (yes/no)? yes
> Warning: Permanently added '[server]:10090,[172.24.***.***]:10090' (ECDSA) to the list of known hosts.
> 对象计数中: 7012, 完成.
> Delta compression using up to 4 threads.
> 压缩对象中: 100% (3456/3456), 完成.
> 写入对象中: 100% (7012/7012), 487.89 MiB | 12.49 MiB/s, 完成.
> Total 7012 (delta 2835), reused 7012 (delta 2835)
> remote: Resolving deltas: 100% (2835/2835), done.
> To ssh://cmserver:10090/cartronics/project-a.git
>  * [new branch]      master -> master
>  * [new branch]      customer -> customer
>  * [new branch]      refs/keep-around/6780f93f4b5c07c2f9de9ee9f1af591b9ad1e874 -> refs/keep-around/6780f93f4b5c07c2f9de9ee9f1af591b9ad1e874
```

---