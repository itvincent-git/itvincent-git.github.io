---
title: svn ignore的使用方法
categories: svn
keywords: 'svn,ignore'
tags: svn
date: 2017-02-24 13:21:17
---


# 查看修改`svn:ignore`
要修改svn:ignore要用到3个svn命令：
- `svn proplist <要查询的目录，留空就是当前目录`
    显示svn属性，可以查到目录有没有设置过`svn:ignore`

- `svn propget svn:ignore <要查询的目录，留空就是当前目录>`
    显示ignore属性`svn:ignore`的值

- `svn propset svn:ignore <要忽略的文件或者文件夹> <要修改的目录>`
    设置ignore属性

例如：
```
$ svn propset svn:ignore "obj" .
```
<!-- more -->
# 递归修改子目录
这个属性影响的只是当前的目录的忽略，如果要影响所有子目录下，则就使用下面的命令：
```
$ svn -R propset svn:ignore <要忽略的文件或者文件夹> <要修改的目录>
```

# 批量修改属性
如果想批量设置一批文件及目录的忽略，可以先配置到一个`.svnignore`文件，然后使用：
```
$ svn propset svn:ignore -F .svnignore .
```
最后一个`.`不要漏了，意思是把`.svnignore`文件应用到当前目录下。

# 提交
修改完后，不要忘记提交`$ svn commit -m "update svn:ignore"`。
