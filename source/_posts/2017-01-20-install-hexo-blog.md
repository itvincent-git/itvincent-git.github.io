---
title: 安装hexo博客
date: 2017-01-20 18:20:07
categories: hexo
keywords: hexo,安装,git,nodejs
tags: 
- hexo
- github
---
![hexo homepage](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/17-2-17/59463456-file_1487333574052_13fc6.png)
# hexo简介
hexo是个快速、简洁且高效的博客框架。

- 超快速度
> Node.js 所带来的超快生成速度，让上百个页面在几秒内瞬间完成渲染。

- 支持 Markdown
>    Hexo 支持 GitHub Flavored Markdown 的所有功能，甚至可以整合 Octopress 的大多数插件。

- 一键部署
>    只需一条指令即可部署到 GitHub Pages, Heroku 或其他网站。

- 丰富的插件
>    Hexo 拥有强大的插件系统，安装插件可以让 Hexo 支持 Jade, CoffeeScript。


<!-- more -->

# hexo安装
安装 Hexo 相当简单。然而在安装前，您必须检查电脑中是否已安装下列应用程序：

- Node.js
- Git

如果已经安装则直接跳到[安装hexo-cli](#安装hexo-cli)


## 安装 Git
- Windows：下载并安装 [git](https://git-scm.com/download/win)
- Mac：使用 [Homebrew](http://mxcl.github.com/homebrew/), [MacPorts](http://www.macports.org/) 或下载 [安装程序](http://sourceforge.net/projects/git-osx-installer/) 安装

- Linux (Ubuntu, Debian)： `sudo apt-get install git-core`

- Linux (Fedora, Red Hat, CentOS)：`sudo yum install git-core`


## 安装 Node.js
安装 Node.js 的最佳方式是使用 [nvm](https://github.com/creationix/nvm)

cURL:

``` bash
$ curl https://raw.github.com/creationix/nvm/master/install.sh | sh
```

Wget:

``` bash
$ wget -qO- https://raw.github.com/creationix/nvm/master/install.sh | sh
```

安装完成后，重启终端并执行下列命令即可安装 Node.js。

``` bash
$ nvm install stable
```

或者您也可以下载 [安装程序](http://nodejs.org/) 来安装。


## 安装hexo-cli

``` bash
$ npm install hexo-cli -g
```

# hexo运行
安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。

```bash
$ hexo init <folder>
$ cd <folder>
$ npm install
```

```bash
$ hexo server

INFO  Start processing
INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
```
启动服务器。默认情况下，访问网址为： [http://localhost:4000/](http://localhost:4000/)。看到一个hello world的页面，说明就启动成功了。

# 配置
新建完成后，指定文件夹的目录如下：

```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

## _config.yml
网站的 配置 信息，您可以在此配置大部分的参数。

## package.json
应用程序的信息。EJS, Stylus 和 Markdown renderer 已默认安装，您可以自由移除。

## package.json

```js
{
  "name": "hexo-site",
  "version": "0.0.0",
  "private": true,
  "hexo": {
    "version": ""
  },
  "dependencies": {
    "hexo": "^3.0.0",
    "hexo-generator-archive": "^0.1.0",
    "hexo-generator-category": "^0.1.0",
    "hexo-generator-index": "^0.1.0",
    "hexo-generator-tag": "^0.1.0",
    "hexo-renderer-ejs": "^0.1.0",
    "hexo-renderer-stylus": "^0.2.0",
    "hexo-renderer-marked": "^0.2.4",
    "hexo-server": "^0.1.2"
  }
}
```

## scaffolds
模版 文件夹。当您新建文章时，Hexo 会根据 scaffold 来建立文件。

## source
资源文件夹是存放用户资源的地方。除 \_posts 文件夹之外，开头命名为 \_ (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。Markdown 和 HTML 文件会被解析并放到 public 文件夹，而其他文件会被拷贝过去。

### 显示草稿

```bash
$ hexo --draft
```
显示 source/_drafts 文件夹中的草稿文章。

## themes
主题 文件夹。Hexo 会根据主题来生成静态页面。