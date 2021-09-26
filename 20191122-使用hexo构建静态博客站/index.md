# 使用Hexo构建静态博客站


## Blog经历

从2014年起，我的博客先后经历了博客第三方平台[**博客园**](https://www.cnblogs.com/tongtianxiao/)，`Wordpress`，`Django` 框架自建， `Tornado`框架自建，`Pelican` 静态博客框架及现在使用的`Hexo`静态博客框架。 可谓是第三方，动静结合，都经历过了， 但最终还是被爆表的颜值主题和易用性强大的`Hexo`所深深折服 ，`Hexo`真香ヾ(o◕∀◕)ﾉヾ！

### 博客框架特点： 

|框架|开发语言|类型|特点|
|-|-|-|-|
|Wordpress|PHP|动态博客|功能插件完备，拿来就用| 
|Django|Python|动态博客|模块完备，开发速度快|
|Tornado|Python|动态博客|小巧玲珑，支持系统级异步并发|
|Pelican|Python|静态博客|简洁，纯粹|
|Hexo|Node.js|静态博客|美，易用|



## `Hero`安装简述

首先安装[Node.js](https://nodejs.org/en/), 然后安装`hexo-cli`:

```sh
npm install hexo-cli -g
hexo init myblog
cd myblog
# 预览
hexo server
```
默认主题模板是`landscape`， 这里推荐国人设计开发的[`melody`](https://github.com/Molunerfinn/hexo-theme-melody), [文档](https://molunerfinn.com/hexo-theme-melody-doc/)完备，通俗易懂，非常棒！

```
npm i hexo-wordcount --save
npm install hexo-tag-aplayer
npm install hexo-tag-dplayer
```
