---
title: android-constraint-layout
categories:
keywords:
tags:
---

>  ConstraintLayout是google官方出品的一种新的布局方式，相比LinearLayout和RelativeLayout，它有着更丰富的特性，而且在不断改良中，我们可以管它叫约束布局。

## 安装

ConstraintLayout支持Android 2.3 (API level 9)及以上版本。在gradle中添加依赖如下：

```
dependencies {
    compile 'com.android.support.constraint:constraint-layout:1.0.2'
}
```

## 特性



## 转换已有的布局

Android Studio自动创建的布局默认使用的是RelativeLayout，我们可以通过如下操作将它转换成ConstraintLayout。

![转换成ConstraintLayout](http://ojicajn2x.bkt.clouddn.com/17-12-2/63329469.jpg)

原RelativeLayout中的内容也会自动转换到ConstraintLayout中。



----

参考文献：

http://blog.csdn.net/guolin_blog/article/details/53122387