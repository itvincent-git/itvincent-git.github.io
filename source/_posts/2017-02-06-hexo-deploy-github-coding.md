---
title: Hexo站点部署到Github Pages和coding.net
categories:
  - hexo
tags:
  - hexo
  - github
keywords: hexo,Github,GithubPages,coding.net
date: 2017-02-06 17:54:00
---

> Github Pages屏蔽了Baidu的爬虫，百度搜索引擎没法抓取到数据。coding.net也提供了类似Github Pages的功能，Baidu爬虫可以抓到数据，Google也能抓到数据。hexo支持部署到多个仓库的能力，下面我们就来操作一下。


<!--more-->
# 申请coding.net
申请一个coding.net的账号，然后创建一个跟账号一致的仓库，这点跟Github Pages相似。

# Git SSH认证
## 生成密钥
首先本地需要安装git客户端，然后需要生成`id_rsa`(私钥)和`id_rsa.pub`(公钥)。

```
$ ssh-keygen -t rsa -C "yourmail@domain.com"
```

可以一路按enter键，然后可在`~/.ssh/`下找到上面的2个文件。

```
$ ssh-add
```

## 记录密钥
在coding.net的`个人设置 》 SSH公钥` 添加这个公钥，如下图：

![](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/17-2-6/42704688-file_1486373356226_139a3.png)

之后执行

```
$ ssh -T git@git.coding.net
```

提示如下就证明OK了。

```
Hello <Your Name>! You've connected to Coding.net via SSH successfully!
```

_如果github上也是这个邮箱，那就不用再生成一次了，否则按照另外一个域名操作一次。_

在github上`个人设置 》 SSH and GPG keys 》new SSH key`添加公钥，如下图：
![](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/17-2-6/624973-file_1486374591691_8ce.png)

# 安装hexo-deployer-git
用于hexo部署到git的插件，如果之前安装过就不需要了。

```bash
$ npm install hexo-deployer-git --save
```

# 一次部署到Github Pages和coding.net
修改hexo根目录的配置文件`_config.yml`：

```
deploy:
  type: git
  repo: 
    github: https://github.com/<your github name>/<your repo>.github.io.git
    coding: git@git.coding.net:<your github name>/<your repo>.git
  branch: master
```
这里github好像只能是https，ssh是不行的，但是coding.net的ssh没有问题。

# 执行部署
```
$ hexo d -g
```
看到两个平台上的日志证明就OK了。