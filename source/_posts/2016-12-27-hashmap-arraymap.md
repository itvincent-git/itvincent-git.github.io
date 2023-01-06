---
layout: post
title:  "Android的HashMap和ArrayMap对比"
date:   2016-12-27 15:00:00 +0800
categories: android
keywords: android,HashMap,ArrayMap
tags:
- android
---
# 原理
-----
原理就不详细说了，看看这个文章：[Fun with ArrayMaps (Android Performance Patterns Season 3 ep1)](https://www.youtube.com/watch?v=ORgucLTtTDI)
不过做完我下面的测试后，结论有所不同。
<!--more-->
# 做了个性能测试
-----
```java

    private void testHashMap(){
        long start = System.currentTimeMillis();
        Map<Integer, Integer> m = new HashMap();
        for (int i = 0; i < 200000; i++){
            m.put(i, i + 100);
        }
        Log.i("Map", "testHashMap 1 time used " + (System.currentTimeMillis() - start));

        start = System.currentTimeMillis();
        m = new HashMap();
        for (int i = 0; i < 200000; i++){
            m.get(i);
        }
        Log.i("Map", "testHashMap 2 time used " + (System.currentTimeMillis() - start));
    }

    private void testArrayMap(){
        long start = System.currentTimeMillis();
        Map<Integer, Integer> m = new ArrayMap<>();
        for (int i = 0; i < 200000; i++){
            m.put(i , i + 100);
        }
        Log.i("Map", "testArrayMap 1 time used " + (System.currentTimeMillis() - start));

        start = System.currentTimeMillis();
        m = new ArrayMap<>();
        for (int i = 0; i < 200000; i++){
            m.get(i);
        }
        Log.i("Map", "testArrayMap 2 time used " + (System.currentTimeMillis() - start));
    }

```

# 测试结果
---
HashMap平均耗时363ms, ArrayMap平均耗时271ms，ArrayMap快了25%。

HashMap增加内存15M，ArrayMap增加内存12M，ArrayMap少增加20%。

HashMap『get()』查询耗时45ms，ArrayMap『get()』查询耗时48ms，ArrayMap慢10%左右

# 加大循环次数
---
把循环次数调大，大到100万，结果耗时内存上ArrayMap更加领先，**『get()』的耗时ArrayMap反超了HashMap**

# HashMap增加初始化容量
---
HashMap构造函数加入初始化的容量后，速度有大量提升，不过仍然比不上ArrayMap。
```java
    Map<Integer, Integer> m = new HashMap(1000000);
```

# 结论
---
测试结果比文章中说的有不同，除了查询时量少的情况下ArrayMap比HashMap慢，数据量大时ArrayMap更快。`put()`的耗时、内存上面ArrayMap是全面优势。






