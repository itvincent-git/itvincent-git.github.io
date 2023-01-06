---
title: Android业务设计模式
categories:
keywords:
tags:
---

## 1
### 功能：
点击Click1，请求服务器，使用服务器返回的数据Data1再做Oper1操作

### 要求：
保证Oper1操作的及时性，不能因为服务器请求不了，返回慢了导致没法进行Oper1操作

### 方案：
- 在未使用Click1，先执行一次Oper1操作，然后把Data1缓存到Data2。
- 当用户触发Click1时，会同时请求服务器和读取缓存Data2，如果服务器超时或失败时，会使用Data2提供给Oper1操作；如果服务器请求成功，则使用Data1提供给Oper1操作，同时更新Data1到Data2缓存。
