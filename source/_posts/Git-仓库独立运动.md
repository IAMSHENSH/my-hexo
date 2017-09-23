---
title: Git 仓库独立运动 - 实验笔记
date: 2017-09-11 
tags: 
- git
- 仓库独立
- note
categories: Git

---


> 关于 Git 仓库独立的笔记

<!-- more -->


## 背景

在开发的过程中，因考虑到：
> 1. 项目「文档类文件」与项目「代码类文件」应该分开管理；
> 1. 项目管理人员与测试人员等角色无需接触「代码类文件」。

因此采取将「代码类文件」从原仓库中剥离出来作为独立仓库的措施；

目前，我已经尝试过将「project-a」内的「src」目录，通过 `git submodule` 的方式分离出一个单独的仓库作为子模块。

## 问题


1. 在「src」内有「子模块A - model-a」，在实际开发过程中，项目工程代码经常因为不同的需求而进行不同分支上切换，直接导致在每次使用「子模块A」的时候就需要多次切换分支与重新编译，存在操作不便与严重拉低开发效率。因此需要将此目录「model-a」从「src」中独立出来。

1. 原方法 「`git submodule`」 由于以下原因，故而改之尝试使用 「`git subtree`」
> 1. 根据官方文档，`git submodule` 在日常使用过程中，每次提交后都需要在上一级模块中使用 `git submodule update` 等命令，目前我们团队中还没有使用过，存在操作不当的风险；
> 1. `git subtree` 是 Git 官方推荐用来代替 `git submodule` 的新方式，官方宣称能更方便更合理的管理子模块；

## 目的

1. 将「model-a」从 「src」仓库中独立；
1. 使用 「git subtree」 的方式，理解与「git submodule」的差别；
1. 决定在「model-a」使用哪种方式;

## 过程

### 一、subtree -> 在自己的 git 仓库上模拟试验
> - 措施： 使用 「subtree」
> - 结果： 试验未完全成功、暂不采用
> - 原因：「`subtree`」模式下，子模块的改动，在上级仓库能侦测到，并且能提交。使用方式与原「`submodule`」存在差异，在现在开发紧张的阶段，考虑到学习与适应成本，暂不采用。

#### 1. 分离：
目的：将 「test 目录」分离出来；（因为这个目录带有log）
1. 把所有 `test` 目录下的相关提交整理为一个新的分支 「test-split」;（ps:此命令执行速度不快）
        $ git subtree split -P test -b test-split
1. 另建一个新目录并初始化为 test 仓库 
        $ mkdir ../test
        $ cd ../test
        $ git init
1. 拉取旧仓库的 test-split 分支到当前的 master 分支；
        $ git pull ../test-mama test-split
1. 将此子仓库关联到远端； (ps:这里缺少其他分支的关联，未继深入尝试)
        $ git remote add origin ssh://git@****/****/****.git
        $ git push -u origin master
1. 回到原项目目录，清理 master 分支上所有跟 「test 目录」有关的痕迹；(ps:此命令执行速度不快)
> 遭遇问题：执行之后，母仓库就脱离了组织，无法 pull 与 push

        $ cd ../test-mama
        $ git filter-branch --index-filter "git rm -rf --cached --ignore-unmatch git" --prune-empty master
1. 另建一个新目录并初始化为 git 仓库；
        $ mkdir ../test-mama-fresh
        $ cd ../test-mama-fresh
        $ git init
1. 拉取 `test-mama` 的 `master` 分支；（到新仓库的 master 分支）
        $ git pull ../test-mama master
~~ 1. 改变远程仓库 -> 到这里无法 push 和 pull 「更改被拒绝」 ~~
        ~~ $ git remote add origin git@****/****/****.git ~~
        ~~ $ git push origin -u master ~~

#### 2. 关联：
目的：将分离出去的仓库与原项目关联
1. 将 git 的项目仓库下载到新仓库的指定目录下
        $ git subtree add --prefix=git ssh://git@****/****/****.git master

#### 3. 更新
目的：能进行推送代码与拉取代码的功能： 
1. 将子目录的更改提交到服务器上：
        $ git subtree push --prefix=git ssh://git@****/****/****.git master
1. 将子目录同步到服务器上最新的版本：
        $ git subtree pull --prefix=git ssh://git@****/****/****.git master

#### 4. 问题
1. 分离出来的母仓库脱离了组织。
1. 分离出来的子仓库，像是复制出来的样子。
1. 在子仓库做的改动，母仓库能有检索。 这与 `submodule` 有差异。


### 二、submodule -> 在自己的 git 仓库上模拟试验
> - 措施： 使用 「filter-branch + submodule」
> - 结果： 试验成功，未采用 `submodule`
> - 原因： `submodule` 已经使用过，然存有潜在的问题

#### 1. 剥离
目的：将目标子仓库分离出来
1. `clone` 出一个新的仓库到目录 test （这时候 test 的目录，与test-mama完全一样） 
        $ git clone test-mama test
1. 刚克隆出来的 test 仓库本地只会有一个分支，如果需要其他分支，则创建；（本仓库没有分，故没进行这一步）
        $ git branch br1 origin/br1  
        $ git branch br2 origin/br2 
1. 最后origin这个remote是不需要的，把它删除了
        $ git remote rm origin

#### 2. 独立
1. 将本仓库转化为 git 子模块！（这个最重要）
> 该命令过滤所有历史提交，保留对 test 子目录有影响的提交，并且把子目录设为该仓库的根目录。
> 该命令执行完毕后，查看当前目录结构就会发现里面已经是子目录的内容了。

        $ git filter-branch --tag-name-filter cat --prune-empty --subdirectory-filter git -- --all

2. 清理仓库
        $ git reset --hard
        $ git for-each-ref --format="%(refname)" refs/original/ | xargs -n 1 git update-ref -d
        $ git reflog expire --expire=now --all
        $ git gc --aggressive --prune=now

3. 最后，我们就可以把这个新的仓库提交到服务器上，然后把旧仓库中的 git 子目录删除并以 `submodule` 的方式添加 test-mama 仓库就好了。


#### 3. 关联

1. 将整理好的仓库同步到服务器新的仓库地址；
> 未尝试

2. 然后把旧仓库中的 git 子目录删除，并以 submodle 的方式添加即可。
> 未尝试

### 三、在 project-a 项目上实现
> - 措施： 使用 「filter-branch + submodule」
> - 结果： 试验完成，正在使用，等待一段时间后的反馈
> - 原因： 因为 `subtree` 非完全独立仓库，也因为原来就是用着 `submodule` 存在潜在的问题，所以只采取了 `filter-branch` 分离措施；

#### 1. 剥离
目的：将目标子目录「model-a」分离出来
1. `clone` 出一个新的仓库到目录 plan-tool
        $ git clone ssh://git@****/****/project-a.src.git plan-tool

2. 添加其他必要的分支
        $ git branch park-data  origin/park-data 
        $ git branch publish/data origin/publish/data

3. 最后origin这个remote是不需要的，把它删除了
        $ git remote rm origin

#### 2. 独立
目的：将子目录独立出新的仓库

1. 将本目录中的 「model-a」 转化为本仓库的根目录
> 这个没有删除 tag 

        $ git filter-branch --tag-name-filter cat --prune-empty --subdirectory-filter model-a -- --all

1. 整理仓库，清除无用信息
> 下面的命令具体干了什么不知道，但是文件夹真的变小了。  本次操作中，4G -> 256M

        $ git reset --hard
        $ git for-each-ref --format="%(refname)" refs/original/ | xargs -n 1 git update-ref -d
        $ git reflog expire --expire=now --all
        $ git gc --aggressive --prune=now

#### 3. 关联
目的：a. 将本地仓库与服务器的新仓库关联； b. 将子仓库与母仓库关联；

1. 将本地仓库，与服务器上新建的仓库关联起来
        $ git remote add origin ssh://git@****/****/project-a.src.plat-tool.git

2. 推送仓库 
> 推送指定 <branch> 分支

        $ git push -u origin <branch>
> 推送所有分支到远程仓库

        $ git push --all

3. 与母仓库关联
> 暂时不做关联，直接以目录的方式放在「src」目录下

### 四、总结

#### submodule 与 subtree 对比
> 在使用感觉上， `git submodule` 类似于引用仓库，而 `git subtree` 似徐于复制仓库。
使用 `git submodule`
1. 母仓库与子仓库各自的 `pull` 与 `push` 互不关联，母仓库只知道子仓库有改动，但具体的改动不能知道。
使用 `git subtree`
1. 子仓库做具体的修改，母仓库可知道。并且在母仓库可提交与记录子仓库的改动。
2. 使用 `git subtree push` 可在母仓库的提交中遍历出与子仓库相关的改动，单独提交与记录到子仓库上。

#### filter-branch 使用
> 此命令用于仓库分离，以后再详细研究；

#### Git仓库清理
> 有命令可以让 Git 仓库瘦身，下次在 「project-a」上尝试
> 用一下命令清理「project-a」仓库，从 25.8 GB -> 24.3 GB
> 以下为操作指令，效果很不明显。 原因不明，日后再查 

        $ git gc --aggressive --prune=now
        对象计数中: 77604, 完成.
        Delta compression using up to 4 threads.
        压缩对象中: 100% (73701/73701), 完成.
        写入对象中: 100% (77604/77604), 完成.
        Total 77604 (delta 35291), reused 40791 (delta 0)
        正在删除重复对象: 100% (256/256), 完成.
        检查连接中: 77604, 完成.

---
本站点采用[知识共享署名-非商业性使用-禁止演绎 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)进行许可。

