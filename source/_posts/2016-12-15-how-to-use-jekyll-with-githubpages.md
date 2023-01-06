---
layout: post
title:  "如何使用jekyll和github pages搭建博客"
date:   2016-12-15 18:50:17 +0800
categories: jekyll
keywords: jekyll,github pages
tags:
- jekyll
---
![jekyll](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/17-2-17/46801530-file_1487334263124_bdcf.png)
## 1.安装jekyll
在linux或mac系统中，打开终端，输入如下：

```ruby
~ $ gem install jekyll bundler
~ $ jekyll new myblog
~ $ cd myblog
~ /myblog $ bundle exec jekyll serve
# => Now browse to http://localhost:4000
```

看到如下信息代表运行成功

```
Server address: http://127.0.0.1:4000/
Server running... press ctrl-c to stop.
```

浏览器中打开 [http://localhost:4000](http://localhost:4000) 看见一个欢迎的页面代表安装成功了。
<!--more-->


## 2.申请github pages

打开github.com，Start a project

![](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/17-1-9/3839245-file_1483958471681_15c66.png)

或者从这里创建一个仓库

![](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/17-1-9/87356132-file_1483958471565_698e.png)

按照`username.github.io`的格式来创建一个仓库

![](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/17-1-9/54134474-file_1483958471420_c7b.png)

创建一个index.html，内容为`hello world`，保存然后上传到github repository。

打开 [https://itvincent-git.github.io/](https://itvincent-git.github.io/)。看到`hello world`的字样，就代表github pages创建成功了。


## 3.把jekyll放到github pages上

由于github pages是直接支持jekyll的，所以可以直接把刚才生成的`myblog`项目直接上传到[https://itvincent-git.github.io/](https://itvincent-git.github.io/)上。

上传前先修改`Gemfile`文件:

```ruby
source "https://rubygems.org"
ruby RUBY_VERSION

# Hello! This is where you manage which Jekyll version is used to run.
# When you want to use a different version, change it below, save the
# file and run `bundle install`. Run Jekyll with `bundle exec`, like so:
#
#     bundle exec jekyll serve
#
# This will help ensure the proper Jekyll version is running.
# Happy Jekylling!
#gem "jekyll", "3.3.1"

# This is the default theme for new Jekyll sites. You may change this to anything you like.
gem "minima", "~> 2.0"

# If you want to use GitHub Pages, remove the "gem "jekyll"" above and
# uncomment the line below. To upgrade, run `bundle update github-pages`.
gem "github-pages" #, group: :jekyll_plugins

# If you have any plugins, put them here!
group :jekyll_plugins do
   gem "jekyll-feed", "~> 0.6"
end
```

然后使用命令行进入`myblog`目录下，执行`bundle update github-pages`会更新jekyll的版本。

更新完毕后重新运行一次jekyll，运行正常。

把`myblog`下的全部文件commit到[https://itvincent-git.github.io/](https://itvincent-git.github.io/)的master上，稍等刷新下页面，就能看到jekyll的首页了。



