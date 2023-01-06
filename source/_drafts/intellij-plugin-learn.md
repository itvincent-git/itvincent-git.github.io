## IntelliJ Plugin插件开发

### 安装

window下的版本2017.2.6正常，建议使用此版本。

用mac下的版本2017.3.2有问题，会出现点击控件时`actionPerform()`没有触发等异常情况。



### 开发环境

- 配置Java SDK，需要1.8以上的版本

![](http://ojicajn2x.bkt.clouddn.com/18-1-11/73411527.jpg)
- 配置IntelliJ SDK

![](http://ojicajn2x.bkt.clouddn.com/18-1-11/12155071.jpg)



### 第一个Plugin

- 创建Plugin项目
- 增加一个Action
- 运行项目



### Plugin打包

创建一个插件存档

- 如果插件模块不依赖于其它的库，将创建一个JAR文件。 否则，将创建一个ZIP存档，其中将包含所有必需的库。

- 右键单击**“Project”**视图中的插件模块，然后在上下文菜单中选择**Prepare Plugin Module <module-name> For Deployment** 。

- 在项目的根目录下，将会看到一个**<module-name> .jar**的打包文件

- 这个文件将可以在Plugin Install界面进行手动安装

  ​

参考文档：

[PluginDevelopmentGuidelines](https://www.jetbrains.com/help/idea/plugin-development-guidelines.html)

