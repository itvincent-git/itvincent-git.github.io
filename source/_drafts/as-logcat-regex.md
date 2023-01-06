---
title: 在Android Studio中使用Regex进行Logcat过滤
date: 2017-01-13 18:09:54
categories: android
tags:
- android
- logcat
---
## 使用方法
Android Studio 的logcat里提供了使用正则式来过滤的规则


在Logcat最右边的下拉选项中选择 Edit Filter Configuration。可以分别对Log Tag、Log Message、Package Name、PID、LogLevel进行过滤

![](http://ojicajn2x.bkt.clouddn.com/17-1-13/12699987-file_1484302977903_16618.png)

## 不显示某类信息

```
^(?!.*(dalvikvm|wpa|Status|ActivityManager)).*$
```