---
layout: post
title:  "使用Sublime Text写markdown"
date:   2016-12-15 21:50:17 +0800
categories: markdown
keywords: markdown,Sublime Text,Package Control,MarkdownEditing,OmniMarkupPreviewer
tags:
- markdown
---
# Sublime Text3
---
首先安装或者更新到[Sublime3](http://www.sublimetext.com/3)。

# 安装Package Control
---
安装插件之前，我们需要首先安装一个Sublime 中最不可缺少的插件 Package Control, 以后我们安装和管理插件都需要这个插件的帮助。

[https://packagecontrol.io/installation](https://packagecontrol.io/installation)

打开使用Sublime，打开菜单，然后在console中执行下面代码：
<!--more-->
``
View > Show Console
``

```ruby
import urllib.request,os,hashlib; h = 'df21e130d211cfc94d9b0905775a7c0f' + '1e3d39e33b79698005270310898eea76'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```
这段代码可能会变，如果console执行失败，请到[packagecontrol.io](https://packagecontrol.io/installation)能查到更新后的命令。

# 安装MarkdownEditing
---
Package Control 安装成功后我们就可以使用它方便的管理插件了，首先使用快捷键 'command + shift + p ' 进入到Sublime 命令面板，输入 "package install" 从列表中选择 "install Package" 然后回车。这时候Sublime开始请求远程插件仓库的索引，所以第一次使用可能会有一些小的延时。

看到列表的更新之后输入 "markdown ed" 关键字，选择“MarkdownEditing" 回车。 插件安装完毕后需要重新启动Sublime插件才能生效。下面是我使用sublime编辑代码片断的显示效果。

# 安装OmniMarkupPreviewer
---
完成了基本的编辑需要，接下来查看Markdown的渲染效果或者输出html格式，OmniMarkupPreviewer就会派上用场了，我们同样需要借助Pacakge Control来安装这个插件。键入 "command + shift + p" 进入sublime的命令界面，输入 "package ins" 然后回车 ，键入 "ominmarkup" 选择OmniMarkupPreviewer , 回车。

插件安装成功后我们就可以使用快捷键对编辑的markdown源文件进行预览了。下面是几个常用快捷键.

- Command+Option+O: 在浏览器中预览
- Command+Option+X: 导出HTML
- Ctrl+Alt+C: HTML标记拷贝至剪贴板

# 结束
---
安装完2个插件后就能够做到一边在Sublime写markdown，另外一边在浏览器预览效果，而且是实时的边写边预览。





