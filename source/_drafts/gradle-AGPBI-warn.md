### 问题

在gradle的编译过程中会出现如下的一堆warn警告，虽然不影响编译，但很影响观看


```
AGPBI: {"kind":"error","text":"warning: Ignoring InnerClasses attribute for an anonymous inner class","sources":[{}]}
AGPBI: {"kind":"error","text":"(com.alipay.android.phone.mrpc.core.j) that doesn\u0027t come with an","sources":[{}]}
AGPBI: {"kind":"error","text":"associated EnclosingMethod attribute. This class was probably produced by a","sources":[{}]}
AGPBI: {"kind":"error","text":"compiler that did not target the modern .class file format. The recommended","sources":[{}]}
AGPBI: {"kind":"error","text":"solution is to recompile the class from source, using an up-to-date compiler","sources":[{}]}
AGPBI: {"kind":"error","text":"and without specifying any \"-target\" type options. The consequence of ignoring","sources":[{}]}
AGPBI: {"kind":"error","text":"this warning is that reflective operations on this class will incorrectly","sources":[{}]
AGPBI: {"kind":"error","text":"indicate that it is *not* an inner class.","sources":[{}]}

```

### 解决方案

1. `proguard-rules.pro`中添加`-keepattributes EnclosingMethod`
2. 解决dependencies中的存在重复的compile声名，例如重复定义了不同版本的support包