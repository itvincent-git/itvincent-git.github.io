---
title: Protobuf在Android下的使用说明
categories: protobuf
keywords: 'protobuf,android,使用说明,新手教程'
tags: protobuf
date: 2017-03-28 22:06:12
---
![](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/18-2-3/89483667.jpg)
> protobuf nano是一个比较新的Protobuf官方实现，很适合在android应用中使用，相比原来的java实现，减少代码量和方法数，原为了这个问题而用C++实现的朋友们，可以换到这个实现，避免尴尬。

<!-- more -->
# 定义`.proto`文件
```proto
package tutorial;

option java_package = "com.example.tutorial";
option java_outer_classname = "AddressBookProtos";

message Person {
  required string name = 1;
  required int32 id = 2;
  optional string email = 3;

  enum PhoneType {
    MOBILE = 0;
    HOME = 1;
    WORK = 2;
  }

  message PhoneNumber {
    required string number = 1;
    optional PhoneType type = 2 [default = HOME];
  }

  repeated PhoneNumber phones = 4;
}

message AddressBook {
  repeated Person people = 1;
}
```

> 常用的类型：`bool, int32, float, double, and string`。
> "= 1", "= 2" 叫Tag，代表是编码时的编号，这个编号很重要，解码时也是根据这个编码来的。
> 尽量使用1-15范围内的Tag定义，16开始会占用更多的数据空间。建议使用嵌套或者`repeated`的方式来避免超过15。

- `required`必填字段，如果创建数据对象时不传入参数，编码时就会抛出`Exception`。
- `optional`可选字段，可以不传入数据，或者设置默认值。

> 使用`required`要注意，如果你升级协议时把这个字段改为`optional`，接收方没有升级，你发送的数据对方将无法解释。因此 **不建议** 使用它，我们只使用`optional`和`repeated`。

官方的`.proto`文档，可以参考[Protocol Buffer Language Guide](https://developers.google.com/protocol-buffers/docs/proto)。

# 选择一个编译器
当你有了`.proto`文件后，你需要把它用编译器生成为一份代码。编译器有不同的平台/实现，我们这里选择`protoc`，它将会生成“bean”、“编解码”。

## 下载编译器
从 https://github.com/google/protobuf/releases 这里可以下载source到本地编译，也可以下载可执行文件。我选择了可执行文件 https://github.com/google/protobuf/releases/download/v3.2.0/protoc-3.2.0-osx-x86_64.zip 。下载后解压，运行`$ bin/protoc --version`，能正常显示版本。

## 执行编译器
执行
```bash
$ ./protoc -I $SRC_DIR --java_out=$JAVA_DIR $SRC_DIR/addressbook.proto
```
例如：
```bash
./protoc -I ../../proto --java_out=../../java ../../proto/addressbook.proto
```

建议生成javanano版本，这个版本特别适合android，生成的文件小非常多，当然性能是比java差一点点。
```bash
$ ./protoc -I $SRC_DIR --javanano_out=$JAVA_DIR $SRC_DIR/addressbook.proto
```

# 使用Java Protobuf API来读写消息
刚才生成的代码是编译不通过的，需要导入`protobuf`的jar包。
在`build.gradle`中加入：
```gradle
compile 'com.google.protobuf.nano:protobuf-javanano:3.1.0'
```
如果使用非nano版本可以用：
```gradle
compile 'com.google.protobuf:protobuf-java:3.2.0'
```

这里推荐使用nano版本。

## 读取数据

```java
public static List<AddressBookProtosNano.Person> list(Context context) throws Exception {

    File bookFile = new File(context.getFilesDir(), "addressbook.data");

    FileInputStream input = new FileInputStream(bookFile);
    ByteArrayOutputStream arrayOutputStream = new ByteArrayOutputStream();
    byte[] buff = new byte[256];
    int read = 0;
    while ((read = input.read(buff)) != -1) {
        arrayOutputStream.write(buff, 0, read);
    }
    // Read the existing address book.
    AddressBook addressBook =
            AddressBook.parseFrom(arrayOutputStream.toByteArray());

    return Arrays.asList(addressBook.people);
}
```

## 写入数据

```java
public static void create(Context context, String name, String email, String number, String type) throws Exception {
    File bookFile = new File(context.getFilesDir(), "addressbook.data");

    // Add an address.
    AddressBook addressBook = new AddressBook();
    addressBook.people = new Person[]{
            setPersonForAddress(name, email, number, type)};
    Log.i(TAG, "addPeople:" + addressBook);

    // Write the new address book back to disk.
    FileOutputStream output = new FileOutputStream(bookFile);
    byte[] serialized = MessageNano.toByteArray(addressBook);

    try {
        output.write(serialized);
        Log.i(TAG, "File written size:" + serialized.length);
    } finally {
        output.close();
    }
}
```

## 源码
参照官方的文档写了个sample的项目，详细可以去这里下载：https://github.com/itvincent-git/protobuf-sample
