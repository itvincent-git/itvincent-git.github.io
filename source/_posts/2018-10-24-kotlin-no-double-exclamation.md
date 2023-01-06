---
title: Kotlin如何避免“!!”（非空断言）
date: 2018-10-24 19:27:07
categories: kotlin
keywords: kotlin,!!
tags: kotlin
---


当我们把Java自动转成Kotlin的时候，代码里会出现很多非空断言`!!`。或者某些场景下因为IDE提示或编译错误，也让我们自己加上了一些`!!`。
但使用`!!`的后果是有可能抛出`IllegalArgumentException：Parameter specified as non-null is null`。

### 如何避免`!!`？

#### 使用?.let/?.apply/?.run
这种是最常用的方法，也是**首选的方法**。但当有多个变量同时要判空时，或者需要处理为null时的逻辑，这种方式稍微有一点麻烦，下面会讲到一些新的方式。

```kotlin
disposable?.let {
    if (!it.isDisposed) it.dispose()
}
```



#### 用Val替代Var
``` kotlin
var mutableString:String? = null

fun run() {
    mutableString = "a"
    printText(mutableString)
}

fun printText(text: String) {
    ...
}
```
此时会报错`Smart cast to 'String' is impossible, because 'multableString' is a mutable property that could have been changed by this time
:app:compileDebugKotlin FAILED`。由于multableString是Var变量，为了避免多线程对变量的修改而出现Null的情况，kotlin从编译上进行了限制。

- 解决方法1是把var变量改为val变量
```kotlin
val mutableString:String = "a"

fun run() {
    printText(mutableString)
}
```


- 解决方法2是写一个新的val变量，将var变量赋值给它，将val作为参数
``` kotlin
fun run() {
    mutableString = "a"
    val string = mutableString ?: ""
    printText(string)
}
```

#### <!-- more -->

#### 使用Elvis操作符

``` kotlin
fun run() {
    multableString = "a"
    printText(multableString ?: "")
}
```


#### 声明lateinit
使用`lateinit`声明到变量上，表示这个变量延迟初始化，比较适合在`Activity.onCreate`这种有生命周期的方法里初始化。
```kotlin
lateinit var mutableString: String
override fun onCreate(savedInstanceState: Bundle?) {
    multableString = "a"
    printText(mutableString)
}
```

> 需要注意的是，访问未初始化的 lateinit 修饰的属性会抛出UninitializedPropertyAccessException异常

注意：基本类型是不能使用`lateinit`的。会抛错`'lateinit' modifier is not allowed on properties of primitive types`。
```kotlin
lateinit var mutableInt: Int
```

#### 代理属性
如果需要对基本类型等做非空处理，可以使用代理属性。
```kotlin
var mutableInt: Int by Delegates.notNull<Int>()
override fun onCreate(savedInstanceState: Bundle?) {
    mutableInt = 1
}
```
一定要在初始化赋值之后才能读取`mutableInt`，不然会抛`IllegalStateException:Property ${property.name} should be initialized before get.`


#### 空与非空处理

``` kotlin
val result = multableString.notNullElse {
    "$it is not null"
} ({ "is null" })
```
新开发的方法`notNullElse`，对单个变量判空处理，非空时传入`it`为非空类型，提高了便捷性，为空时使用第二个block来返回值。适合那里需要判空，返回值`result`也是非空的类型，比较实用。[源码在此下载](https://github.com/itvincent-git/kotlin-ex/blob/master/lib/src/main/java/net/kotlin/ex/lib/NullPointerEx.kt)


#### 多个值非空
``` kotlin
private var mLinearLayout: LinearLayout? = null

...
private fun initView(context: Context) {
    mLinearLayout = LinearLayout(context)
}

...

if (tvItem == null) {
    mLinearLayout!!.addView(childTvItem)
} else {
    mLinearLayout!!.addView(childTvItem, mLinearLayout!!.indexOfChild(tvItem) + 1)
}
```
当我们要对多个值判断的时候，`let`就不那么好用了，但如果不使用`let`就拿不到非空的类型，像上面要判断2个都不为空时做操作，为空时另外一个逻辑。其实一早我们就已经判断空了，有没有更好的方法呢？

``` kotlin
allNotNullElse(tvItem, mLinearLayout) { a, b ->
    b.addView(childTvItem, b.indexOfChild(tvItem) + 1)
} ({ mLinearLayout?.addView(childTvItem) })
```
新开发的方法`allNotNullElse`返回的a, b 两个值已经是非空类型了，这样`addView`使用的也是非空类型，使用起来更方便了。[源码在此下载](https://github.com/itvincent-git/kotlin-ex/blob/master/lib/src/main/java/net/kotlin/ex/lib/NullPointerEx.kt)



