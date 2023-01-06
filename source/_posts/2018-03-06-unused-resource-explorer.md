---
title: Unused Resource Explorer——Android无用资源浏览器
categories: android
keywords: Intellij Plugin, android
tags: 
  - Intellij Plugin
  - android
date: 2018-03-06 21:17:48
---


### 前言

- 全自动化的清理脚本，容易会出现多删除了资源的情况
- 升级了gradle版本后，原来一些根据lint分析的工具出现了问题



### 想要的工具

- 能对扫描出来的无用资源进行浏览，图片能够预览
- 能对扫描出来的无用资源进行分类
- 可自动清理，也可手动个别清理


<!-- more -->

#### 安装插件

- [下载插件文件](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/18-03-06-unused-resource-explorer.jar)

- [使用本地文件安装方式安装到AndroidStudio](https://www.jetbrains.com/help/idea/installing-a-plugin-from-disk.html)

  >IntelliJ IDEA 插件文件是一个存档文件：ZIP 或 JAR。在安装之前，您不需要解压缩它。您应该照原样使用它。
  如果您的计算机上有可用的插件文件，您可以安装下述步骤来安装它：
  >1. 打开 "设置/首选项" 对话框（例如：Ctrl+Alt+S）。
  >2. 在左侧窗格中，选择 "插件"。
  >3. 在右侧部分的 "插件" 页上，单击 "从磁盘安装插件"。
  >4. 在打开的对话框中， 选择插件存档文件，然后单击"确定"。
  >5. 在 "设置/首选项" 对话框中，单击 "应用" 或 "确定"。
  >6. 如果建议，重新启动 IntelliJ IDEA。



### 工具使用

#### 执行Android Lint

- 首先第一步执行**lint**命令来生成分析报告xml文件。
- 打开**Android Studio**的**Gradle Projects**面板，选择一个module下的**lint**命令，双击执行

![](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/unused-resource-explorer-2.png)

- Gradle执行成功后，会生成如下文件：**<module>/build/outputs/lint-results-debug.xml**，记下这个文件的位置，下面会用到。

  > 如果项目比较大，lint执行全部的任务会耗时很久，其实我们只需要检查UnusedResources，没必要所有类型的问题都检查，通过在**<module>.gradle**文件中添加如下配置，即可只检查UnusedResources，大大提高速度。
  >
  > ```groovy
  > android {
  >     lintOptions {
  >         check "UnusedResources"
  >     }
  > }
  > ```

  ​

#### 使用Unused Resource Explorer浏览lint.xml文件

- **Choose lint report xml**：选择刚才生成出来的**lint-results-debug.xml**或**lint-<XXX>.xml**文件，选择完后，把扫描出来的无用资源显示在下面的面板上
- **Open** ：选择一个列表中的资源，点击open在编辑器面板打开这个文件
- **Delete**：选择一个列表中的资源，点击Delete删除它
- **Clean**：清理列表中的**layout/anim/drawble/**下的资源，清理列表中**.png/.jpg**为后缀的资源



![](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/unused-resource-explorer-4.png)

