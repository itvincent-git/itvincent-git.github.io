---
layout: post
title:  "使用七牛云存储markdown用的图片"
date:   2017-01-09 15:00:00 +0800
keywords: qiniu,mardown,图床,极间图床
categories: markdown
tags:
- markdown
---
![qiniu homepage](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/17-2-17/95692112-file_1487333779860_1483d.png)
## 概述
使用md写文章的人，喜欢他的方便性，可移植性，写一个文本就包括了文章及排版。不过图片链接却不是那么方便，每个博客站点都有一些自己的图片上传方式。你想把这篇文章再copy到其他站点，由于源站点会设置图片的防盗链，新的站点上的文章就看不到图片了。为了解决这个问题，ka老师告诉了我一个图床的方式来解决。
<!--more-->
## 图床
1. 首先选择一个图床网站或软件，例如[极间图床](http://yotuku.cn/)。

2. 打开站点后，可以把图片拖到这里或选择文件来上传图片。

    ![](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/17-1-9/5155253-file_1483963360014_7779.png)

3. 上传完后，在下面会出现链接，可以直接复制链接，或者点击复制markdown链接。

    ![](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/17-1-9/85911607-file_1483963456256_11ddb.png)

不过由于这个服务上传的地址是一个公用的地址，有可能被删除或失效。因此极简图床也提供给我们自己设置七牛空间来上传图片。

## 七牛存储
1. 首先打开[七牛站点](http://qiniu.com)。注册一个账号，输入邮箱、手机、姓名等身份认证之后完成注册。

2. 然后创建一个对象储存。

    ![](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/17-1-9/75752041-file_1483964119462_509c.png)

3. 自己起一个空间名称，区域选择自己附近的，访问控制选择“公开空间”。

    ![](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/17-1-9/52203369-file_1483964002644_706.png)

4. 在“个人面板”->“密钥管理”中查看AccessKey/SecretKey。

    ![](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/17-1-9/63087181-file_1483964002789_c3e8.png)

5. 在储存空间的“空间概览”里，记住这里的“测试域名”

    ![](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/17-1-9/20046002-file_1483964605921_e3f8.png)

## 在图床上设置七牛账号

在极简图床上配置上刚才七牛储存的“空间名称”、“AccessKey”、“SecretKey”、“域名”后，保存。然后就可以上传到自己专属的七牛空间上了。

![](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/17-1-9/44269451-file_1483964002921_bea1.png)

