---
title: 让Hexo站点在百度和Google中被搜索到
categories: hexo
keywords: hexo,baidu,google,seo,收录,抓取,排除外链,keywords
tags:
  - seo
  - hexo
date: 2017-02-06 17:58:38
---


> 由于我的Hexo站点是部署在Github Pages上的，但Github Pages屏蔽了Baidu的爬虫，现在增加了一份代码部署在了coding.net，这样Baidu就可以正常抓取了，详细可以看[Hexo站点部署到Github Pages和coding.net](/2017/02/06/hexo-deploy/)。

# 收录
一个新的站点要被搜索引擎搜索到，首先就要手动将站点收录到搜索引擎里，下面主要讲述Baidu及Google搜索引擎的收录方式。

<!-- more -->
## Baidu收录

### 站点验证
百度站点验证入口： http://zhanzhang.baidu.com/dashboard/index

点击添加网站：
![](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/17-2-6/32775264-file_1486367579749_52d8.png)

有三种验证方式：
- 文件验证
- HTML标签验证
- CNAME验证

我选择Html标签验证：

```
<meta name="baidu-site-verification" content="<baidu verify code>" />
```

Hexo Next主题提供了简便的方式，在站点根目录下的`_config.yml`，增加一行配置`baidu-site-verification: <baidu verify code>`。然后重启Hexo，查看一下网页源码，包含了`<meta>`信息，证明就OK了。

在Baidu中点击验证站点，通过后进入下面的阶段。

### 抓取诊断
进入站点的管理界面 http://zhanzhang.baidu.com/dashboard/index ，在左侧菜单中选择`网页抓取 -> 抓取诊断`，分别选择PC/移动进行抓取操作。返回结果正常则正常Baidu Spider可以正常抓取你的站点了。

### 给站点添加sitemap
为了让Baidu掌握你站点的所有链接内容，需要增加网站地图sitemap。

1. baidusitemap.xml适合提交百度搜索引擎
2. sitemap.xml适合提交给谷歌搜素引擎
```
$ npm install hexo-generator-baidu-sitemap --save 
$ npm install hexo-generator-sitemap --save 
$ hexo g
```
看看`/public/`下生成了这2个文件就OK了。

### 网页抓取
在左侧菜单中选择`网页抓取 -> 链接提交`，选择`自动提交`，填写数据文件地址`http://example.com/baidusitemap.xml`，验证正常。

Baidu共提供外4种方式，官方推荐是`主动推送 > 自动推送 > sitemap`
1. 主动推送：最为快速的提交方式，推荐您将站点当天新产出链接立即通过此方式推送给百度，以保证新链接可以及时被百度收录。
2. 自动推送：最为便捷的提交方式，请将自动推送的JS代码部署在站点的每一个页面源代码中，部署代码的页面在每次被浏览时，链接会被自动推送给百度。可以与主动推送配合使用。
3. sitemap：您可以定期将网站链接放到sitemap中，然后将sitemap提交给百度。百度会周期性的抓取检查您提交的sitemap，对其中的链接进行处理，但收录速度慢于主动推送。
4. 手动提交：一次性提交链接给百度，可以使用此种方式。

主动推送就是自己手动提交的意思，Hexo Next主题提供了_自动推送_功能，这个全自动化比较简单好用，打开站点主题`theme/next/_config.yml`文件：

```
# Enable baidu push so that the blog will push the url to baidu automatically which is very helpful for SEO
baidu_push: true
```

查看源码，看到有下面一段代码证明就OK了。
```javascript
<script>
(function(){
    var bp = document.createElement('script');
    var curProtocol = window.location.protocol.split(':')[0];
    if (curProtocol === 'https') {
        bp.src = 'https://zz.bdstatic.com/linksubmit/push.js';        
    }
    else {
        bp.src = 'http://push.zhanzhang.baidu.com/push.js';
    }
    var s = document.getElementsByTagName("script")[0];
    s.parentNode.insertBefore(bp, s);
})();
</script>
```

---

## Google收录

### 站点验证
打开如何将内容提交给 Google http://www.google.cn/intl/zh-CN/submit_content.html 
![](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/17-2-6/64619652-file_1486351808275_2ef1.png)

打开将您的网址添加到 Google 索引中 https://www.google.com/webmasters/tools/submit-url?hl=zh-CN 
![](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/17-2-6/93370609-file_1486351893453_5b72.png)

验证你的网站是否你所拥有，如果使用了godaddy.com的DNS解释服务，Google默认支持，按指示操作一下即可，如果是其它DNS服务商，请按提示操作。

验证结束后，查看一下DNS解释，多了一项Verify Code。应该就是Google验证用来验证用的。
`TXT  @   google-site-verification=<Your verify Code>    1 Hour`

_除了使用DNS验证，也可以使用其它验证方式，跟Baidu相似。_ 

打开站点首页 https://www.google.com/webmasters/tools/home?hl=zh-CN ，查看自己验证通过的URL。

### 抓取工具
验证完站点后，点击站点名称，进入到站点管理界面
`https://www.google.com/webmasters/tools/dashboard?hl=zh-CN&siteUrl=<Your Site URL>`。

- 左边菜单选择`抓取 -> Google 抓取工具`

![](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/17-2-6/25454670-file_1486361925084_6de6.png)

- 分别选择“桌面”和“移动版”，点击“抓取”。

- 等待抓取成功后，点击旁边的“请求编入索引”。

### 站点地图
- 左边菜单选择`抓取 -> 站点地图`。

- 选择“添加站点地图”。
![](http://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/17-2-6/5191345-file_1486362236075_1280a.png)
- 输入`sitemap.xml`，提交。

### 验证
验证Google是否收录成功站点，可以通过在Google搜索框中输入`site:example.com`来查看搜索结果。Google的收录速度比Baidu快很多，抓取完不久就能搜索出结果。

# 关键词
## 添加默认关键词
打开主题配置文件`_config.yml`，添加keywords字段。

```
keywords: keyword1,keyword2
```

## 文章Front-Matter中加入keywords
在每篇post文章的Front-Matter加入keywords字段，Next主题支持该字段，如果这里输入了keywords，那么上面的默认关键词就不会出现。

```
---
title: 文章标题
categories: 文章分类
keywords: keyword1,keyword2
---
```
## 验证
重启hexo，打开浏览器打开你的post，右键查看源代码，看看是否有

```
<meta name="keywords" content="keyword1,keyword2" />
```

# 防止外链搜索
搜索引擎爬到你站点的外链多，会降低站点的PR值，也就是影响了你的排名，所以需要告诉爬虫哪些链接不要爬了。
原理就是在`<a>`里增加一个属性`rel="external nofollow"`，这样爬虫就不会去爬这个链接。
hexo上已经有这样的插件工具，简单配置就能做到这项操作，叫做[hexo-autonofollow](https://github.com/liuzc/hexo-autonofollow)。

## 安装插件

```
$ npm install hexo-autonofollow --save
```

## 配置
站点配置文件`_config.yml`里增加如下配置：

```
nofollow:
    enable: true
    exclude:
    - exclude1.com
    - exclude2.com
```

# 提高资源加载速度
各种搜索引擎都对网站的速度有需要，速度越快，排名自然越前。对站点资源的压缩很有必要。有一个hexo插件[hexo-all-minifier](https://github.com/chenzhutian/hexo-all-minifier)，很简单就帮我们完成了这件事情，包括html、css、js、image的压缩。打开上面的链接，按照上面的说明`install`，再配置一下`_config.yml`就可以了。



# 总结
这些是收录的必要步骤，然后就是等待和更新自己站点的内容，这样才能更容易的提升搜索排名及增加搜索关键字。



-------------
参考文献：
http://www.jianshu.com/p/619dab2d3c08