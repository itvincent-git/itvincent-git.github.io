当出现以下错误时：
`Error:Failed to crunch file C:\xxxx\xxxxxxxxxxxxxxxxxxxx\xxxxxxxxxxxxxxxxxxxxxxxxxxx\xxxxxxxxxxxxxxx into  C:\yyyyyy\yyyyyyyyyyyyyyyyy\yyyy\yyyyyyyy`

是由于在window上文件路径有256字符数的限制，我们的项目路径太多了，就会报错。

修复方法：
```
allprojects {
    repositories {
       mavenLocal()
    }
    buildDir = "C:/tmp/${rootProject.name}/${project.name}"
}
```
