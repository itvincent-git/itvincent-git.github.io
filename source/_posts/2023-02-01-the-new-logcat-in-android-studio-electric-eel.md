---
title: The New Logcat in Android Studio Electric Eel(电鳗)
date: 2023-02-01 18:36:50
categories: android
tags:
    - android
    - android studio
    - logcat
---
> 更新到刚出的 Android Studio Electric Eel(电鳗)，首先体验到全新的 Logcat 工具，我们来看看这次更新有什么新变化

![](https://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/picgo/202302011808149.png)

<!-- more -->

# 搜索过滤
最大的更新莫过于搜索过滤栏的改动了
使用 key:value 的方式来定义，可以编写多个规则
按 ^ + Space 或者在输入时可以打开提示
![|400](https://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/picgo/202302011228240.png)


## 规则历史记录
点击左边的漏斗可展开历史记录
![|300](https://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/picgo/202302011540478.png)

鼠标移动到记录的左边，点击★可以收藏该条记录
![|300](https://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/picgo/202302011542788.png)


## 匹配模式
新的 Logcat 匹配模式有如下3种，分别为`:` , `=:` , `~:`：
`tag: ` 包含有文字
`tag=: ` 完全等于
`tag~: ` 正则式匹配

## 排除
在 tag 等标签前添加 `-` 则表示排除掉这个匹配结果
![|400](https://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/picgo/202302011409129.png)

例如 `tag:H -tag:Hi` 则表示包含 H 的 tag 中排除掉包含 Hi 的结果

## 逻辑运算符和括号
支持使用逻辑运算符，比旧版本只能在正则式里写些复杂规则要直观很多，易写易读性得到提高。
`(tag:foo | level:ERROR) & package:mine`

如何不写运算符，则按照相关的标签并且*没有排除*时使用 `OR` ，不同的标签使用 `AND`，例如：

`tag:foo tag:bar package:myapp` 

等于：

`(tag:foo | tag:bar) & package:myapp`

但如何使用了排除则是：

`tag:foo -tag:bar package:myapp`

等于：

`tag:foo & -tag:bar & package:myapp`

如果不用运算符与使用运算符混合的情况下，`AND`的优先级会降低，例如：

`foo bar tag:bar1 | tag:bar2` 

等于：

`'foo bar' & (tag: bar1 | tag: bar2)`

> 因此当条件比较多于2个以上时，**建议是使用运算符来编写规则**

## 专用的标签
### **`message`**
按照 Log 的消息体进行匹配

### **`tag`**
按照 Log 的 Tag 进行匹配

### **`package`**
按照App的包名进行匹配

### **`process`**
按照进程名进行匹配

### `package:mine`
匹配当前打开项目的包名，相当于旧版的当前项目过滤器

### **`level`**
表示过滤日志级别要高于等于该定义的级别

### **`age`**
表示过滤时间戳距离现在小于一个时间长度
```
age:30s 过滤最近30秒的日志
age:5m 过滤最近5分钟的日志
age:3h 过滤最近3小时的日志
age:1d 过滤最近1天的日志
```
以你输入规则的时候开始往前计算时间，而且日志会不停叠加，所以你不想要再看到之前的日志，则要重新再输入或选择一次规则

### **`is`**
- `is:crash` ：过滤崩溃日志
- `is:stacktrace` ：过滤堆栈日志
是一种**全新且实用的标签类型**，希望官方后续可以再增强，实现一些正则式不好实现的规则

### `name`
给规则定义一个名称，在历史记录里方便查找
![|300](https://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/picgo/202302011827178.png)



# 配置Logcat显示格式

有标准(Standard)和紧凑(Compact)模式，当然还有自定义（Modify Views)。

![|150](https://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/picgo/202302011218072.png)

从信息量上来看，平时选择**紧凑模式**就够了。

## 自定义Format
![|500](https://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/picgo/202302011219026.png)

- Show timstamp 时间：可选择日期&时间，或时间
- Tag column width 设置 Tag 显示的宽度
- Show repeated tags 去掉勾选可隐藏重复的 tag

![|300](https://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/picgo/202302011223195.png)
- 右边的 Package Names 与 Tags 的功能类似的

新版在格式配置上提供了更多灵活度。