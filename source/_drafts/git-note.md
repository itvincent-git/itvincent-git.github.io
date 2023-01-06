---
title: Git笔记
categories: git
keywords: git
tags: git
---

# Git基础知识
## 分布式版本控制系统
## 直接记录快照，而非差异比较
Git 并不保存这些前后变化的差异数据。实际上，Git 更像是把变化的文件作快照后，记录在一个微型的文件系统中。每次提交更新时，它会纵览一遍所有文件的指纹信息并对文件作一快照，然后保存一个指向这次快照的索引。为提高性能，若文件没有变化，Git 不会再次保存，而只对上次保存的快照作一链接。


## 近乎所有操作都是本地执行
## 时刻保持数据完整性
在保存到 Git 之前，所有数据都要进行内容的校验和（checksum）计算，并将此结果作为数据的唯一标识和索引。换句话说，不可能在你修改了文件或目录之后，Git 一无所知。这项特性作为 Git 的设计哲学，建在整体架构的最底层。所以如果文件在传输时变得不完整，或者磁盘损坏导致文件数据缺失，Git 都能立即察觉。

## 多数操作仅添加数据

## Git的三种状态
在 Git 内都只有三种状态：已提交（committed），已修改（modified）和已暂存（staged）

# Git命令

## git help [command]
想了解 Git 的各式工具该怎么用，可以阅读它们的使用帮助

## git config
Git 提供了一个叫做 git config 的工具（译注：实际是 git-config 命令，只不过可以通过 git 加一个名字来呼叫此命令。），专门用来配置或读取相应的工作环境变量。而正是由这些环境变量，决定了 Git 在各个环节的具体工作方式和行为。
 
## git init
在工作目录中初始化新仓库

## git add [file]
开始跟踪一个新文件

## git commit
现在的暂存区域已经准备妥当可以提交了。在此之前，请一定要确认还有什么修改过的或新建的文件还没有 `git add` 过，否则提交的时候不会记录这些还没暂存起来的变化。所以，每次准备提交前，先用 `git status` 看下，是不是都已暂存起来了，然后再运行提交命令 `git commit`

尽管使用暂存区域的方式可以精心准备要提交的细节，但有时候这么做略显繁琐。Git 提供了一个跳过使用暂存区域的方式，只要在提交的时候，给 git commit 加上 -a 选项，Git 就会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 git add 步骤

## git clone [url]
从现有仓库克隆

## git status
检查当前文件状态。要确定哪些文件当前处于什么状态。

## .gitignore
一般我们总会有些文件无需纳入 Git 的管理，也不希望它们总出现在未跟踪文件列表。通常都是些自动生成的文件，比如日志文件，或者编译过程中创建的临时文件等

## git diff
此命令比较的是工作目录中当前文件和暂存区域快照之间的差异，也就是修改之后还没有暂存起来的变化内容。


若要看已经暂存起来的文件和上次提交时的快照之间的差异，可以用 git diff --cached 命令。（Git 1.6.1 及更高版本还允许使用 git diff --staged，效果是相同的，但更好记些。）来看看实际的效果：

## git rm
要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。可以用 git rm 命令完成此项工作，并连带从工作目录中删除指定的文件，这样以后就不会出现在未跟踪文件清单中了。

## git mv
要在 Git 中对文件改名

## git log
在提交了若干更新之后，又或者克隆了某个项目，想回顾下提交历史，可以使用 git log 命令查看。

## git tag
列出现有标签的命令非常简单`$ git tag`
新建标签`$ git tag -a v1.4 -m 'my version 1.4'`

# 理解git的分支
## git branch

创建一个分支`$ git branch testing`

切换一个分支 `$ git checkout testing`

理解`HEAD`的含义

## 分支的新建与合并

### 创建分支
```
$ git checkout -b hotfix
$ vim index.html
$ git commit -a -m 'fixed the broken email address'
```

### 合并分支
```
$ git checkout master
$ git merge hotfix
```
合并分支前，当前目录不能有未暂存的文件。

### 删除分支
```
$ git branch -d hotfix
Deleted branch hotfix (was 3a0874c).
```
### 显示分支
git branch 命令不仅仅能创建和删除分支，如果不加任何参数，它会给出当前所有分支的清单

### 推送本地分支
git push
```
$ git push origin serverfix
$ git push origin serverfix:serverfix
```

上传我本地的 serverfix 分支到远程仓库中去，仍旧称它为 serverfix 分支

别人用`$ git fetch origin`可以拉取到这个分支。
在本地创建一个serverfix，并且与远程仓库中的serverfix代码一致。
$ git checkout -b serverfix origin/serverfix


### 删除远程分支

```
$ git push origin :serverfix
```

咚！服务器上的分支没了。你最好特别留心这一页，因为你一定会用到那个命令，而且你很可能会忘掉它的语法。有种方便记忆这条命令的方法：记住我们不久前见过的 git push [远程名] [本地分支]:[远程分支] 语法，如果省略 [本地分支]，那就等于是在说“在这里提取空白然后把它变成[远程分支]”。

### git rebase

