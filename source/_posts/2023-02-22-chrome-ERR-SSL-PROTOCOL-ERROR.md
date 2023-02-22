---
title: Chrome 访问 http 地址报错：ERR_SSL_PROTOCOL_ERROR
date: 2023-02-22 19:02:59
categories: tools
tags:
    - chrome
---
# 问题
当用 Chrome 访问某些 http 的网站时，会无法打开，换一台设备或其他浏览器是能正常打开的。

```
此网站无法提供安全连接
www.host.com 发送的响应无效。
ERR_SSL_PROTOCOL_ERROR
```

<!-- more -->

# 分析
由于访问某些 http url 的时候，最终 chrome 会将地址转为 https 。但有些网站是未支持 https 的，因此会导致无法访问。

例如输入 http://www.host.com ，被转换成 https://www.host.com 。

由于 chrome 安全缓存模块错误地缓存了这个域名的 https 信息，误认为这是支持 https 的域名。

# 解决办法
清除 chrome 中的安全缓存信息，移除[SSL/TLS](https://howchoo.com/webdesign/use-chrome-developer-tools-to-check-whether-resources-are-loading-over-ssl)下该域名的安全信息。

1. 访问：[chrome://net-internals/#hsts](chrome://net-internals/#hsts)

![](https://tuchuang-1256050518.cos.ap-chengdu.myqcloud.com/picgo/202302221856403.png)

2. 在 Delete domain security policies 中输入有问题的域名，点击 delete 删除
3. 再次访问 http://www.host.com 页面加载正确
