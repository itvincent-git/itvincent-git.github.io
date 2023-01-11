## 安装 Hexo
所有必备的应用程序安装完成后，即可使用 npm 安装 Hexo。
`$ npm install -g hexo-cli`

## 安装
### Hexo 版本 >= 5.0 theme-melody v1.8.0+

进入你的 hexo 博客的工作路径

```
npm install hexo-theme-melody
```

## 设置

在 hexo 的工作目录下找到站点配置文件——`_config.yml`:

```
theme: melody # 将主题设置成melody
```

> WARNING
> 
> 如果你没有 pug 以及 stylus 的渲染器，请下载安装：
> 
> `npm install hexo-renderer-pug hexo-renderer-stylus --save` 或者
> 
> `yarn add hexo-renderer-pug hexo-renderer-stylus`

## 配置

### Hexo 版本 >= 5.0 theme-melody v1.8.0+

1.  在 hexo 的工作目录下创建一个 `_config.melody.yml`。
2.  将 `./node_modules/hexo-theme-melody/_config.yml`（这意味着你要先通过 npm 安装 hexo-theme-melody）里的内容拷贝至 `_config.melody.yml`。
3.  如果你是 `hexo-theme-melody` 的老用户，请将 `melody.yml` 的内容拷贝至 `_config.melody.yml`，然后将 `melody.yml` 删掉，因为它将会被废弃。

之后你就只需要通过`npm update hexo-theme-melody` 来平滑升级 `theme-melody` 了