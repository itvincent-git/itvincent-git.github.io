---
title: ndk-addr2line
date: 2017-01-17 18:07:59
tags:
---

### 问题描述

当jni代码出现崩溃时，取回的崩溃栈可能如下：

```
xxx
```

对于这样的信息我们无法知道错误在哪里。



### 解决方案

可通过addr2line工具翻译出错误的行信息

``` bash
// -f 输出函数名  
// -e 输出错误代码行数和文件路径  
// xxx.so 对应出错的so文件, 在android工程obj目录下  
// addr 是具体的地址  
arm-linux-androideabi-addr2line -f -e xxx.so addr
```

例如：
``` bash
$ANDROID_NDK_HOME/toolchains/arm-linux-androideabi-4.8/prebuilt/darwin-x86_64/bin/arm-linux-androideabi-addr2line -C -f -e libmyjni.so c82343
```