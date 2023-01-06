---
title: Android ANR分析办法
categories:
keywords: ANR,Android,分析,查找
tags:
---
# bugreport报告
adb bugreport > bugreport.txt

# traces文件
adb pull /data/anr/traces.txt

## 搜索dump出的app
Cmd line:

# 搜索
搜索以下字符串，可看到anr发生时间
Wrote stack traces to '/data/anr/traces.txt'

## 搜索包名

## 搜索anr
VM TRACES AT LAST ANR

## Input Dispatcher ANR
查询输入事件
Input Dispatcher State at time of last ANR

## WINDOW MANAGER LAST ANR
查询窗口日志
WINDOW MANAGER LAST ANR (dumpsys window lastanr)

## SYSTEM LOG
查询系统日志，也有人称为Main Log
SYSTEM LOG (logcat -v threadtime -d *:v)

## EVENT LOG
EVENT LOG (logcat -b events -v threadtime -d *:v)

## dead lock
waiting to lock <0x12312> (xxx) held by tid=12 (xxxx)
如果循环lock就是dead lock了
