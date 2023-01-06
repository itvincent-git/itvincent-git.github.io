---
title: 绑定博客到自己的godaddy域名
categories: hexo
keywords: '自定义域名,godaddy,hexo,github,coding.net'
tags:
  - hexo
  - github
  - coding.net
date: 2017-02-10 19:44:52
---


> 买了个自己的域名，想把Github Pages绑定到这个域名下。结合了网上的文章及Github Pages的帮助文档，整理出下面的完整设置方式。

![godaddy](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/godaddy-01.jpg)

<!-- more -->

# Github上设置
首先要在Github Pages的仓库上增加一个名叫`CNAME`的文件，我们可以用我们hexo项目的source目录下增加，内容为：

```
example.com
```

`$ hexo g`后，`/public/`目录下也会有`CNAME`文件，然后部署到Github Pages的仓库上。

此时再访问你的`http://example.github.io`，浏览器就已经跳转到`example.com`上了。

# godaddy上配置

> 我们先把`www`域名解释到Github Pages的服务器上，我们需要配置一个`CNAME`。

## CNAME
CNAME就是把域名映射到另外一个域名上，这样访问`www.example.com`就能访问到`example.github.io`的服务器，浏览器上显示的仍然是`www.example.com`。如下设置，name是唯一的，你看已有配置里面有就直接修改，没有就新增：

Type |   Name  |  Value |  TTL Actions
---    |    ---   |  ---    | ---
CNAME |  www | example.github.io | 600s

完成后隔一段时间等生效，访问`www.example.com`就已经能看到你的博客了。

如果着急还可以用`dig`命令查看dns解释情况：

```
$ dig example.com +nostats +nocomments +nocmd

; <<>> DiG 9.8.3-P1 <<>> example.com +nostats +nocomments +nocmd
;; global options: +cmd
;example.com.             IN      A
example.com.      3600    IN      A       192.30.252.153
```

看到ip地址是`192.30.252.153`或`192.30.252.154`证明就正确，否则就再等等吧。

> 只要做到`www`域生效，其实就可以推广你的站点了，可以看[让Hexo站点在百度和Google中被搜索到](/2017/02/06/hexo-blog-baidu-google-seo/)。但如果想要让顶级域名`example.com`也生效就会复杂一点，可以继续看下面。


## A
由于godaddy不支持顶级域名配置`CNAME`。有一种办法，通过配置A记录来实现。A记录就是把域名映射到IP上。

Type |   Name  |  Value |  TTL Actions
---  |   ---   |  ---    | ---
A    | @        | 192.30.252.153 |  600s 
A    | @        | 192.30.252.154 |  600s 

`@`就代表顶级域名，如上把顶级域名绑定到这2个IP上，等生效，或者再用上面`dig`命令验证，用浏览器查看`example.com`。

> 上面这个方法由于绑定到固定的IP上的，例如你想把`coding.net`的pages服务也换上去的话，就比较难控制了。

## 301转发
然而godaddy还支持设置把一个域名301跳转到另外一个域名，所以可以把`example.com`跳转到`www.example.com`。

godaddy在你的域名下配置`Forwarding`，链接输入`www.example.com`，选择301，保存。


**用`A`或者`301`都是可选操作，大家可以尝试后再决定。**








