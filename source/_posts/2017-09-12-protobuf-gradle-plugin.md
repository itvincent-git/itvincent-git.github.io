---
title: 使用protobuf gradle plugin创建 protobuf-lite
date: 2017-09-12 14:36:09
categories: protobuf
keywords: 'protobuf,android,protobuf gradle plugin'
tags: protobuf
---
> `protobuf`在android还推荐一种使用方式为`protobuf-lite`，使用`protobuf gradle plugin`在
构建时生成代码的方式来使用`protobuf`。

Protobuf的使用上问题，可以参考 [Protobuf在Android下的使用说明](/2017/03/28/protobuf-java-android-guildline/)。

# 添加protobuf-gradle-plugin
在项目根目录下的`build.gradle`文件中修改为如下代码：

```gradle
buildscript {
    repositories {
        jcenter()
        mavenCentral()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.3.0'
        classpath 'com.google.protobuf:protobuf-gradle-plugin:0.8.3'
    }
}
```
<!-- more -->

# 引用protobuf-gradle-plugin
在application的项目下`build.gradle`文件修改为如下代码：

```gradle
apply plugin: 'com.google.protobuf'//声明插件

...

protobuf { //protobuf生成的配置
    protoc {
        artifact = 'com.google.protobuf:protoc:3.2.0'
    }
    plugins {
        javalite {
            artifact = 'com.google.protobuf:protoc-gen-javalite:3.0.0'
        }
    }
    generateProtoTasks {
        all()*.plugins {
            javalite { }
        }
    }
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.google.protobuf:protobuf-java:3.2.0' //依赖的protobuf-java lib
}
```

执行`android studio`的`Build > Rebuild Project`后。在`app/build/generated/source/proto`下会生成相应的protobuf java代码。

程序样例:
https://github.com/itvincent-git/protobuf-sample/tree/gradleplugin
