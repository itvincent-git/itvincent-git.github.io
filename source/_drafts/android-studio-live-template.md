---
title: android-studio-live-template
categories:
keywords: AndroidStudio,Live Template
tags: AndroidStudio
---

## MOLD ——自定义Live Template

### Live Template介绍

Live Template可让您快速，高效且准确地将经常使用的或自定义的代码结构插入源代码文件。

例如在Android Studio的代码编辑框中输入`fori`，然后按回车，即可生成一个标准的for循环。

![](http://ojicajn2x.bkt.clouddn.com/18-1-25/75584320.jpg)



#### 常用Live Template

- `fori`生成for循环
- `geti`生成getInstance()静态方法




#### 查看Live Template

在Android Studio中打开**Preferencies > Editor > Live Templates** 

![](http://ojicajn2x.bkt.clouddn.com/18-1-25/40442453.jpg)



### MOLD定义的Live Template

MOLD是一套自定义的Live Template库。分为不几个不同的分类。

**MLog**是提供MLog日志输出的模板。

**Property**是提供生成对象属性的模板。



##### MLog

- `mlogp`**生成MLog.info日志，打印出当前方法所有传的参数，很实用 **
- `mlogt`**生成MLog TAG，把光标定位到类定义下的第一行，输入即可生成 **
- `mlogi`生成MLog.info日志
- `mlogd`生成MLog.debug日志
- `mlogv`生成MLog.verbose日志
- `mloge`生成MLog.error日志


##### Property

- `pfs`生成public final static String
- `pfsb`生成public final static boolean
- `pfsi`生成public final static int
- `pfsl`生成public final static long





##### Constructor

- `pubcon`生成public构造函数
- `pricon`生成private构造函数




### 如何导入MOLD到Live Template

##### Mac

把XXX.xml复制到`~/Library/Preferences/Android Studio<version>/templates`

例如`~/Library/Preferences/AndroidStudio3.0/templates`

##### Windows

把XXX.xml复制到` <your_user_home_directory>\.Android Studio<version_number>\config\templates`

例如`C:\Users\Administrator\.AndroidStudio3.0\config`



重启Android Studio后生效。



==============================================

**欢迎各位贡献出优秀好用的Live Template，加入到MOLD！**

==============================================

----

[官方文档]: https://www.jetbrains.com/help/idea/live-templates.html