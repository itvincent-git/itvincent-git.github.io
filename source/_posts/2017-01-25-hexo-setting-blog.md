---
title: 构建一个自己的hexo博客
date: 2017-01-25 18:20:07
categories: hexo
keywords: hexo,安装,raytaylorism,github,git,GithubPages
tags:
  - hexo
  - github
---

hexo的主题确实不错，所以把我的blog从jekyll转到hexo下来了。上一篇讲到如何[安装一个hexo博客](/2017/01/20/hexo-blog/)，这次讲讲怎么配置及使用它。
<!-- more -->
推荐一个不错的主题叫：[raytaylorism](https://github.com/raytaylorlin/hexo-theme-raytaylorism)。访问它的站点，里面有详细的说明。

_最近也使用了Next主题，也非常不错的，官方主页看[这里](http://theme-next.iissnan.com/)，里面的说明很清晰，配置过一次raytaylorism，再配置Next就是照样画葫芦。_

# 安装

```bash
cd yourblog
git clone https://github.com/raytaylorlin/hexo-theme-raytaylorism.git themes/raytaylorism
```

请不定期git pull一下主题以便获得最新的功能。请在pull之前先备份好你原来的配置。

# 启用（重要）

- 修改 _config.yml 中的theme一项的值为raytaylorism

- 由于本主题使用了Data Files数据文件和额外的layout文件，所以请复制以下文件到你的博客目录中，否则在启动server时可能会报错。

- 复制yourblog/themes/raytaylorism/_data文件夹到yourblog/source目录下

- 复制yourblog/themes/raytaylorism/_md/下所有文件夹（about和reading）到yourblog/source目录下

- 在你的yourblog/_config.yml配置文件的#pagination的位置添加下面配置（禁用归档、标签、目录页面的分页功能）

```
archive_generator:
  per_page: 0
tag_generator:
  per_page: 0
category_generator:
  per_page: 0
```

# 重新启动
```
$ hexo server
```
重启看到整个界面都换了。

# 开始第一篇文章

## 新建草稿

```bash
$ hexo new draft my-first-blog
INFO  Created: ~/myblog/source/_drafts/my-first-blog.md
```

用SublimeText编辑这个文件，开始写你的第一篇文章。（如何用SublimeText编辑可以参考：[使用Sublime Text写markdown](/2016/12/15/markdown-guide/)）

要预览写的文章需要打开草稿预览功能：
```bash
$ hexo server --draft
```

## 发布文章
未发布前，文章一直在\_draft目录下，需要发布到\_post下，才会在正式环境下看到。

```
$ hexo publish my-first-blog
```
成功后会文件会被移到\_post目录下。

** 刷新你的网页，就能看到你新发布的文章了 **

# 部署到github pages

github pages原生支持jekyll解析，hexo部署到github不像jekyll，需要generate好生成出来的/public目录放到github pages上。

不过一点都不麻烦，因为hexo支持了直接部署到git上。
github pages的申请，可以参考该文章[如何使用Jekyll和github Pages搭建博客](/2016/12/15/how-to-use-jekyll-with-githubpages/#2-申请github-pages)

## 安装hexo-deployer-git
```bash
$ npm install hexo-deployer-git --save
```

## 修改_config.yml
```
deploy:
  type: git
  repo: <repository url>
  branch: [branch]
  message: [message]
```

例如：
```
deploy:
  type: git
  repo: https://github.com/itvincent-git/itvincent-git.github.io.git
  branch: master
```

## 生成public及部署上github
```
$ hexo d -g   (相当于hexo deploy -g)
```

运行过程中会提醒输入github的账号及密码。而且会在本地目录下生成一个`/.deploy_git/`的目录，里面放着跟`/public`里相仿的文件，实际上这就是本地的git repo。如果出现有目录的同步问题，可以把该目录删除，hexo会重新生成好。

```
INFO  Deploy done: git
```
看到输出这个，说明已经部署到git，打开你的github pages主页看看效果吧。







