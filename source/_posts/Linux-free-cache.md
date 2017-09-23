---
title: Linux 手动释放 Cache
date: 2017-09-11 18:25:32
tags: 
- cache
- note
categories: Linux
---


> 关于在 Linux 手动释放内存的方法介绍

<!-- more -->

## 手动内存步骤

1. 使用 `free` 查看内存使用情况
```
$ free -h
>              total       used       free     shared    buffers     cached
> Mem:           11G       4.6G       7.2G        16M        76M       2.6G
> -/+ buffers/cache:       1.9G       9.9G
> Swap:         2.0G         0B       2.0G

```

1. 切换到超级用户
```
$ su root
> Password:
```

1. 查看 「/proc/sys/vm/drop_caches」的值，默认为 0 
```
# cat /proc/sys/vm/drop_caches
> 0
```

1. 手动执行 `sync`命令 
```
# sync
```
    > 描述：sync 命令运行 sync 子例程。如果必须停止系统，则运行sync 命令以确保文件系统的完整性。sync 命令将所有未写的系统缓冲区写到磁盘中，包含已修改的 i-node、已延迟的块 I/O 和读写映射文件

1. 将/proc/sys/vm/drop_caches值设为3
```
# echo 3 > /proc/sys/vm/drop_caches
# cat /proc/sys/vm/drop_caches
> 3
```

1. 使用 `free` 查看内存使用情况
```
# free -h
>              total       used       free     shared    buffers     cached
> Mem:           11G       1.9G       9.8G        16M       2.6M       217M
> -/+ buffers/cache:       1.7G        10G
> Swap:         2.0G         0B       2.0G

```

---
本站点采用[知识共享署名-非商业性使用-禁止演绎 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)进行许可。