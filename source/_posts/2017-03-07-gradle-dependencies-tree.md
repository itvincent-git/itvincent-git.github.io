---
title: gradle中显示包依赖树
categories: gradle
keywords: gradle,dependencies,tree
tags: gradle
date: 2017-03-07 18:02:34
---

网上查了一些资料，要在gradle中显示dependencies依赖树，要使用`dependencies`命令，不过由于
顶级项目中一般我们都不定义dependency的，一般是在子项目中定义，那么可以用这种方式来写：

```
./gradlew <sub module name>:dependencies
```

这样就能输出依赖树了。不过如果有多个子项目的话，要一个个子项目的查非常麻烦，所以可以通过下面的方法
来输出整个项目的依赖树。
<!--more-->

在顶级项目的`build.gradle`文件中加入：

```
subprojects {
    task allDeps(type: DependencyReportTask) {}
}
```

然后执行：
```
$ ./gradlew allDeps
```

这样就会显示所有子项目的依赖树了。
