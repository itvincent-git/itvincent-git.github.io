---
title: Protobuf原理分析
categories: protobuf
keywords: 'protobuf,varint,tag,length,value'
tags: protobuf
date: 2017-04-04 17:41:19
---

![](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/18-1-10/86470598.jpg)
> 详细分析protobuf(以下简称pb)数据序列化Tag-WireType-Value方式，对VARINT、带符号整型的详细分析，分别对`int32, int64, uint32, uint64, sint32, sint64, bool, enum，fixed64, sfixed64, double, string, bytes, embedded messages, packed repeated fields, fixed32, sfixed32, float`所有protobuf支持的数据类型进行说明。通过demo和验证过程，相信能帮忙到大家理解protobuf的原理。
> <!-- more -->
> <!-- toc -->



### Tag-WireType-Value

pb里描述每一个数据对象均按这一规则。
1. Tag是每个字段后的1、2、3 ……
2. WireType是指数据类型和Value的长度，下表列出全部类型：

| Type | Meaning          | Used For                                 |
| ---- | ---------------- | ---------------------------------------- |
| 0    | Varint           | int32, int64, uint32, uint64, sint32, sint64, bool, enum |
| 1    | 64-bit           | fixed64, sfixed64, double                |
| 2    | Length-delimited | string, bytes, embedded messages, packed repeated fields |
| 3    | Start group      | groups (deprecated)                      |
| 4    | End group        | groups (deprecated)                      |
| 5    | 32-bit           | fixed32, sfixed32, float                 |
3. Value是指字段实际值

> 下面的例子均按照proto3的协议定义来编写，相比proto2，optional是默认的。详细请看[官方文档](https://developers.google.com/protocol-buffers/docs/proto3)

```
syntax = "proto3";
package test;
option java_package = "com.example.test";
option java_outer_classname = "TestProtos";

message User {
    int32 id = 1;
    string name = 2;
    repeated string icon_url = 3;
}
```
如果数据设置如下：
```json
{
  id : 10,
  name : "Jo"
}
```

则编码后的数据为`080a12024a6f`

| hex  | 说明                                       |
| ---- | ---------------------------------------- |
| 08   | 这是Tag和WireType部分，规则为 $tag << 3 &#124; WireType。 tag:id(1)，字段类型为VARINT(0)，所以值为 1 << 3 &#124; 0。 |
| 0a   | 10的值。                                    |
| 12   | tag:id(2)，字段类型为Length-delimited(2)，所以值为 2 << 3 &#124; 2。 |
| 02   | 由于是字符串类型，这个段表示字符串的长度，"Jo"长度为2。           |
| 4a   | J的ASCII码                                 |
| 6f   | o的ASCII码                                 |



### 可变长整形VARINT

一个int32来说，无论是1，还是65535，都会占用4个字节，而pb为了使数据更紧凑更节省空间，使用了可
变长的int来表示数字。小的数用1个字节来表示，大的数用多个字节来表示。VARINT就是一种可使用1个或多个字节来表示整型的方法。
每个VARINT的最后一个字节是一个标志位(msb)，表示这个数字除了当前字节是否有后一个字节来一起表示。
剩下的低7位表示数字的实际数值。
例如数字1，用VARINT表示：
```
0000 0001
```
由于不需要后面的字节来表示，所以msb为0，`000 0001`表示1。
数字300，这个比较复杂一点，编码后为`08ac02`，转为二进制：
```
1010 1100 0000 0010
```
分析：
```
  1010 1100 0000 0010
→ 010 1100  000 0010 把msb去掉后
→ 000 0010  010 1100 pb使用的整形型是低位方式(little-endian)，所以要把2个字节互换位置
→ 000 001 0010 1100  2个字节拼接在一起
→ 1 0010 1100        去掉前面的0
→ 256 + 32 + 8 + 4 = 300  2进制转换为10进制
```
**可见，编码如1、300这样值不高的数字时，结果为1个字节到2个字节之间，对于我们大多数的实际场景来说是非常适合的，能有效的节省编码长度。**



### 有符号整型

前面章节中看到，所有VARINT(0)为0的都是用VARINT来表示的，包括的具体类型有`int32, int64, uint32, uint64, sint32, sint64, bool, enum`。但是`sint32/sint64`，与`int32/int64`在用于表示 **负数** 是有区别的。如果使用`int32`表示一个负数，结果会占用10个字节，实际上用了一个很大无符号数来表示。如果使用`sint32`，会使用`ZigZag`编码方式来提高效率。



### ZigZag

`ZigZag`编码是用一种正负数的交错方式来表达，当数值的绝对值小的时候，会比使用`int32`的方式节省很多字节。例如-1编码为1，1编码为2，-2编码为3，如下表：

| 带符号数        | 编码后        |
| ----------- | ---------- |
| 0           | 0          |
| -1          | 1          |
| 1           | 2          |
| -2          | 3          |
| 2147483647  | 4294967294 |
| -2147483648 | 4294967295 |

例如：
```
message Signed {
    int32 a = 1;
    sint32 b = 2;
}
```

如果数据设置如下：
```json
{
  a : -10,
  b : -10
}
```

则编码后的数据为`08f6ffffffffffffffff011013`

| hex                  | 说明                                       |
| -------------------- | ---------------------------------------- |
| 08                   | tag:id(1)，int32字段类型为VARINT(0)，所以值为 1 << 3 &#124; 0。 |
| f6ffffffffffffffff01 | 用一个很大的正数来表示的-10                          |
| 10                   | tag:id(2)，sint32字段类型为VARINT(0)，所以值为 2 << 3 &#124; 0。 |
| 13                   | 用ZigZag表示的-10, 0x13 = 19，符合上面编码表推算出来的值   |

`sint32`的值用这个公式表达：`(n << 1) ^ (n >> 31)`

**可见`sint32`只用1个字节表示-10，`int32`要用10个字节。**



### int32分析

接下来分析上面那串10个字节的int32：
> 内容比较长，建议copy出来看

```
f6ffffffffffffffff01
→ 1111 0110 1111 1111 1111 1111 1111 1111 1111 1111 1111 1111 1111 1111 1111 1111 1111 1111 0000 0001 转成2进制
→  111 0110  111 1111  111 1111  111 1111  111 1111  111 1111  111 1111  111 1111  111 1111  000 0001 把msb去掉后
→ 000 000 1111 1111  1111 1111 1111 1111 1111 1111 1111 1111 1111 1111 1111 1111 1111 0110 把字节顺序反转，再拼在一起
→            ff         ff        ff        ff        ff        ff        ff        f6 去掉前面的0，转为16进制，这个数就是0xffffffffffffffff - 9 = -10（可以打印看Integer.toHexString(-10)的16进制字符串）。
```



### 非VARINT的数字

如上图的字段类型表，`float/fixed32/sfixed32`的字段类型为5，`double/fixed64/sfixed64`的字段类型为1，它们使用固定长度方式来记录数字，同样也是使用little-endian字节顺序，有符号类型也是使用ZigZag方式。



### enum枚举

enum枚举也是用VARINT来表示的，enum枚举是带默认值的，默认为0的元素，默认值是可以不编码的，因为解码端默认就是0。空代表元素0，01代表元素1，02代表元素2。



### bool布尔值

bool布尔值也是用VARINT来表示的，bool是带默认值的，默认为false，默认值是不编码的，因为解码端默认就是false。空代表false，01代表true。



### string字符串

string字段类型为Length-delimited(2)，最早的例子里出现过，tag << 3 | 2，后面跟一个VARINT来表示字符串长度，紧跟着就是字符串的内容。



### bytes二进制数据

编码方式跟string一致，但不会对字符串编码进行处理。转换成Java类型为byte[]。



### 嵌套对象

一个类型嵌套另外一个类型：
```
message Test1 {
    int32 a = 1;
}

message NestTest {
    Test1 t = 1;
}
```

设置a的值为300，编码后输出为：`0a0308ac02`。上面的例子已经分析过300编码后为`08ac02`，对比可以看出，嵌套多出了前面`0a03`。嵌套对象的字段类型为Length-delimited(2)，因此 tag << 3 | 2 = 0a，后面跟着嵌套对象的长度，即`08ac02`的长度为3，因此为03。



### Repeated标记可重复

在proto2里，Repeated标记的字段可以是0个或多个的数值，在Java里翻译过来是array。编码后Repeated的数值可以是连续，也可以不连续，因为每次编码都是按Tag-WireType-Value，最后把各个值merge合并。
举例：
```
message RepeatedTest {
    repeated int32 a = 1;
}
```
数据为：1, 2, 3
编码后：`080108020803`
```
0801  tag << 3 | 0  , 1
0802  tag << 3 | 0  , 2
0803  tag << 3 | 0  , 3
```
同一个tag出现多次，解码的时候把多次进行merge处理，还原出array结果。



### Packed Repeated标记为打包一块可重复

表示把数据打包在一起形成一个类似于嵌套的形式，打包一起的数据不能分开，必须连续。相比repeated少编码了重复的tag。
举例：
```
message RepeatedPackedTest {
    repeated int32 a = 1 [packed=true];
}
```
数据为：1, 2, 3
编码后：`0a03010203`
```
0a  tag << 3 | 2 (Length-delimited(2))
03  payload，后面的数据长度
01  1
02  2
03  3
```


### 总结&建议

- proto2版本所有字段加上optional，proto3默认为optional，不用手动再添加。
- 有负数的数值时使用`sint32/sint64`。
- 正数没超过2 ^ 28 - 1 = 268435455，则使用int32类型，否则使用fixed32类型，当然超过了2 ^ 32 - 1，就要使用`int64/fixed64`了。



### 源码

至此把protobuf所有数据类型的原理进行了分析，上面的示例代码可到此下载：[https://github.com/itvincent-git/protobuf-sample](https://github.com/itvincent-git/protobuf-sample ) 。测试代码在`app/src/test/../ExampleUnitTest.java`。
