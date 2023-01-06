---
title: Android更改packageName包名
categories: android
keywords: 'android,packageName,包名,rename'
tags: android
date: 2017-04-26 18:35:45
---


如果需要把原有的app复制一份出来，新的app使用一个新的`packageName`，有如下几个步骤要做：
1. 更改`Manifest.xml`中的`packageName`
2. 更改`Manifest.xml`中`<activity><service><receiver><provider>`的name为新的完整包名，不能使用简写
3. 更改`build.gradle`中的`applicationId`
4. 更改所有`import`了本项目中的`R.class`语句
5. 更改所有`import`了本项目中的`BuildConfig.class`语句
  > 项目资源`R.class`的包名是根据`packageName`生成的

<!-- more -->

# 传统方式
修改上面1、2、3点比较简单，第4点需要进入每个`activity/view/fragment`进行修改，工作量很大，而且容易出错

# 效率方式
- 先不修改1、2、3
- 找到`R.class`文件的位置（一般在`ApplicationRoot/build/generated/source/r/debug`）（Android Studio）
- 然后`debug`目录下建立新包名的目录
- 把`R.class`移动到新的目录下，或者使用`refractor->move`，此时android studio会触发do refractor的逻辑，此过程会比较久
- 检查`activity/view/fragment`的中`import R.class`是不是都已经指向新的包名
- 再执行1、2、3

> 同样的方式应用到`BuildConfig.class`

# 补充注意
- 如果有使用到""字符串中包含有包名，要自己手动进行修改
