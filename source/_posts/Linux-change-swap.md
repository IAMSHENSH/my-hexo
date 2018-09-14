---
title: Linux 调整 Swap 
date: 2017-09-11 18:25:32
tags: 
- swap
- note
categories: Linux
---

![linux-swap](http://ovlnfs1rj.bkt.clouddn.com/linux-swap.JPG)
> 关于在 Linux 安装后调整 Swap 的方法

<!-- more -->

## 调整步骤

1. 切换到超级用户
``` 
$ su root
> Password:
```

1. 在 「/root/」目录下创建一个 2GB 名为「myswaofile」的交换文件
```
# dd if=/dev/zero of=/root/myswapfile bs=1G count=2 
> 2+0 records in
> 2+0 records out
> 2147483648 bytes (2.1 GB, 2.0 GiB) copied, 24.9961 s, 85.9 MB/s

# ls -l /root/myswapfile 
> -rw-r--r--. 1 root root 2147483648 Sep  6 11:20 /root/myswapfile
```

1. 设置文件权限，以保护此 swap 分区
```
# chmod 600 /root/myswapfile
```

1. 使用 `mkswap`命令，让此文件设置 「swap」
```
# mkswap /root/myswapfile
> Setting up swapspace version 1, size = 2 GiB (2147479552 bytes)
> no label, UUID=efdad09d-d48c-4c2b-a2e4-7704b4991881
```

1. 启动交换文件
```
# swapon /root/myswapfile
```

1. 为了确保开机启动，在「/etc/fstab」末尾加入：
> CoreOS 无此文件
```
# cat /etc/fstab
> cat: /etc/fstab: No such file or directory
```

1. 查看系统中交换文件设置情况
```
# swapon -s
> Filename				         Type		Size	Used	Priority
> /root/myswapfile               file    	2097148	0	    -1
```

1. 使用 `free` 确认再次确认
```
# free -h
>              total       used       free     shared    buffers     cached
> Mem:           11G       4.6G       7.2G        16M        71M       2.6G
> -/+ buffers/cache:       1.9G       9.8G
> Swap:         2.0G         0B       2.0G
```

## 其他命令

打开所有交换空间
```
# swappon -a
```
关闭所有交换空间
```
# swappoff -a
```
---

题图：来源于 Google