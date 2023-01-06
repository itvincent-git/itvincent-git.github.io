---
title: 学习kotlin
categories:
keywords:
tags:
---

# 主要特性

- kotlin是完全兼容java6的，不需要在最新的java虚拟机上运行
- 性能不会带来坏处，生成出来的java字节码跟常规的java是一样的
- 运行时的库资源也非常小，不会导致安装包大幅增加
- 使用lambda表达式时，kotlin标准库的函数会以肉联的方式出现，确保不会有新的对象创建，避免遇到额外的垃圾回收
- 强调实用性，对IDE工具的专注，idea上的插件开发跟随编译器
- 精简，消除了getter、setter、构建器参数赋给字段
- 安全，移除了NullPointerException，检查过类型就无需额外的类型转换
- 互操作性，与java互通，任何地方混合java和kotlin，能在不同语言中调试跟踪

### 静态方法

- 全都是静态方法的情况 : class 类名 改为 object 类名

```
object LogUtil {
    // java调用kotlin 的静态方法需要加上注解 @JvmStatic 
    @JvmStatic 
    fun d(msg : String){
        Log.d("", msg)
    }
}
```

- 部分方法是静态方法的情况 : 将方法用 companion object { } 包裹

```
companion object {
	fun first(){
        LogUtil.d("", "这里是静态方法")
    }
}
```
### 类型推导

```
var string: String = ""
var string = ""
var char = ' '

var int = 1
var long = 0L
var float = 0F
var double = 0.0

var boolean = true

var foo = MyFooType()
```



### 空安全

下面是这个问题的 kotlin 写法，我们定义一个空值，但是在我们尝试操作它之前，Kotlin 的编译器就告诉了我们问题所在：

```
val a:String = null
```

曝出的错误是：我们在尝试着给一个非空类型分配一个 null。在 Kotlin 的类型体系里，有空类型和非空类型。类型系统识别出了 string 是一个非空类型，并且阻止编译器让它以空的状态存在。想要让一个变量为空，我们需要在声明后面加一个 `?` 号，同时赋值为 null。

```
val a: String? = null
println(a.length())
```

我们尝试打印 stirng 的长度，但是我们遇到了跟 Java 一样的问题，这个字符串有可能为空，不过幸好的是：Kotlin 编译器帮助我们发现了这个问题，而不像 Java 那样，在运行时爆出这个错误。

```
val a: String? = null
println(a?.length())
```

如果值是空，则会返回空。如果不是空值，就返回真实的值。print 遇到 null 会输出空。



### 函数

### 变量

`val` 相当于java中的final变量

`var` 相当于java中的非final变量

### 属性

自定义获取器

```
val isXXX: Boolean
	get() {
      	return height == width;
	}
```

### 枚举

```
enum class Color {
  RED, ORANGE
}
```



### when

相当于java的`switch`

使用代码块



### is

相当于java的`instanceof`



### as

一个指定类型的显式转换

```
val n = e as Num
```



### 遍历

#### while

```
while (condition) {
  
}
```



#### for

```
for (i in 0...100) {
  
}
```



#### 遍历map



### 对象表达式

处理java中的匿名内部类

```
web.webViewClient = object : WebViewClient() {
    override fun shouldOverrideUrlLoading(view: WebView?, request: WebResourceRequest?): Boolean {
        return super.shouldOverrideUrlLoading(view, request)
    }   
}
```



### lambda表达式



### 函数引用



#### 转JSON



### 函数式编程

filter,map,reduce



### 函数扩展



### 访问外部类的this对象

可通过**this@类名**的方式引用

```
class A { // 隐式标签 @A
    inner class B { // 隐式标签 @B
        fun Int.foo() { // 隐式标签 @foo
            val a = this@A // A 的 this
            val b = this@B // B 的 this

            val c = this // foo() 的接收者，一个 Int
            val c1 = this@foo // foo() 的接收者，一个 Int

            val funLit = lambda@ fun String.() {
                val d = this // funLit 的接收者
            }


            val funLit2 = { s: String ->
                // foo() 的接收者，因为它包含的 lambda 表达式
                // 没有任何接收者
                val d1 = this
            }
        }
    }
}
```

