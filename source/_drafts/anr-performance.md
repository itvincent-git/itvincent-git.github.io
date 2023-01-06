---
title: anr-performance
categories:
keywords:
tags:
---

# 导致ANR的问题分析

## UI线程IO操作
例如 `file.exist()`这种很容易被遗漏

## slow inflate

```
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <!--<item android:state_checked="true" android:drawable="@drawable/ww_invite_btn_in_room_down"/>-->
    <item android:state_pressed="true" android:drawable="@drawable/ww_invite_btn_in_room_down"/>
    <!--<item android:state_selected="true" android:drawable="@drawable/ww_invite_btn_in_room_down"/>-->
    <item android:drawable="@drawable/ww_invite_btn_in_room"/>
</selector>
```

# Choreographer
4.1之后butter project加入的改进android流畅度的重要改进。统一的UI绘制的最小间隔为1/60秒。通过监控Choreographer的doframe方法
，可以知道每次ViewRootImpl.performTraversals()所画一帧的时间，当所用时间超过了1/60秒，可判断UI丢失了一帧画面。

# 调用系统service的性能

# 调用