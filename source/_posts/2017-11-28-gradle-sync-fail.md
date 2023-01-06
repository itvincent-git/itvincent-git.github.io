---
title: Android gradle.zip文件下载失败
categories: gradle
keywords: gradle
tags: gradle
---


### Android Studio构建时出现下面的错误
```
Gradle sync failed: Could not run build action using Gradle distribution 'xxxxxx/gradle-2.14.1-all.zip'.
```

### 解决方法
1. 访问`https://services.gradle.org/distributions/`找到我们所要的版本
2. 然后在文件上点右键复制链接
3. 打开我们项目中`gradle/wrapper/gradle-wrapper.properties`文件
4. 修改`distributionUrl`为复制的链接
5. 重新build项目，会从我们设置的url下载`gradle-xxx-all.zip`

例如：
```properties
#Mon Dec 28 10:00:20 PST 2015
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-2.14.1-all.zip
```