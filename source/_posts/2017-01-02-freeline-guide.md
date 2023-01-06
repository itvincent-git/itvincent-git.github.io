---
layout: post
title:  "Freeline使用指南"
date:   2017-01-01 15:00:00 +0800
categories: android
keywords: android,freeline,Android Studio,插件,构建
tags:
- android
- freeline
- 构建
---

# 概述
Freeline是蚂蚁金服旗下一站式理财平台蚂蚁聚宝团队在Android平台上的量身定做的一个基于动态替换的编译方案，稳定性方面：完善的基线对齐，进程级别异常隔离机制。性能方面：内部采用了类似Facebook的开源工具buck的多工程多任务并发思想, 并对代码及资源编译流程做了深入的性能优化。
<!--more-->
# 安装
详细可以访问[Freeline主页](https://github.com/alibaba/freeline)。

* 首先修改gradle项目中顶级目录下的`build.gradle`，加入freeline的配置:

```Groovy
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.antfortune.freeline:gradle:0.8.4'
    }
}
```

* 在修改application运行项目下的`build.gradle`，引用freeline插件:

```Groovy
apply plugin: 'com.antfortune.freeline'

android {
    ...
}
```

* 运行命令行，下载freeline插件。

```
Windows下执行: gradlew initFreeline
Linux/Mac下执行: ./gradlew initFreeline
```

如果上面的速度太慢的话，国内的用户也可以通过下面的方式提高速度：

```
gradlew initFreeline -Pmirror
```

> 此时执行如果返回错误`Permission Deny`，是因为`gradlew`文件没有设置执行权限，执行下面命令行可增加权限，然后重新运行上面的命令：

```
$ chown +x gradlew
```

# 运行

安装freeline的Android Studio插件。

安装方式:

* 打开Android Studio
* 进入Preferences → Plugins
* 搜索freeline，然后安装
