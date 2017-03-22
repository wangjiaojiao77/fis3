## 1、原因
- [Yahoo34条前端性能优化黄金法则](https://developer.yahoo.com/performance/rules.html/)
- 缓存利用：添加Expires头、配置Etag等
- 静态资源版本更新与缓存

## * 如何更新这些缓存？

## 2、同名覆盖
一般前端团队做法（query）：

```
<script src='a.js?v=1.0.0'></script>
```
问题：
- （1）先覆盖index.html，后覆盖a.js        错误的页面
- （2）先覆盖a.js，后覆盖index.html        错误的页面
- （3）CDN缓存攻击

## 3、不同名覆盖
基于文件内容的hash版本冗余机制

工程师：
```
<script src='a.js'></script>
```
线上：
```
<script src='a_8824e91.js'></script>
```
好处：
- 1、不存在间隙问题；
- 2、遇到问题回滚版本的时候，只须回滚页面即可；
- 3、缓存利用率大增；
- 4、不会受到构造CDN缓存形式的攻击。

## 4、fis3  起步
- 1、定义：面向前端的工程构建工具。解决前端工程中性能优化、资源加载（异步、同步、按需、预加载、依赖管理、合并、内嵌）、模块化开发、自动化工具、开发规范、代码部署等问题。

- 静态资源标记+动态解析资源静态表

- 2、安装

```
npm install -g fis3
fis3 -v
npm update -g fis3
```
- 3、构建：

```
fis3 release -d <path>
```


- 4、资源定位：分离    开发路径与部署路径。

```
fis.match('*.{png,js,css}', {
     release: '/static/$0'
});
```


- 5、配置文件：fis-conf.js

配置接口：

```
fis.match(selector, props);
```
- 6、文件指纹：添加MD5戳

```
fis.match('*.{js,css,png}', {
     useHash: true
});
```


- 7、压缩资源：压缩器

```
fis.match('*.js', {
  optimizer: fis.plugin('uglify-js')
});
```
- 8、CssSprite图片合并

```
fis.match('::package', {
  spriter: fis.plugin('csssprites')
})

fis.match('*.css', {
  useSprite: true
});
```


- 9、功能组合


## 5、内容嵌入
- ?__inline


- 1、html中嵌入base64图片
- 2、html中嵌入样式文件
- 3、 html中嵌入脚本文件
- 4、js中嵌入js
- 5、css中嵌入css文件

## 6、声明依赖
- __RESOURCE_MAP__


## 7、fis3   调试

1、内置Web Server
- （1）目录：fis3 server open
- （2）发布：fis3 release
- （3）启动：fis3 server start
- （4）监听：fis3 release -w
- （5）刷新：fis3 release -wL


2、替换内置Server

```
fis3 release -d C:/Users/PC-WJJ/Desktop/jojojojo/output

fis.match('*', {
    deploy: fis.plugin('local-deliver', {
        to: 'C:/Users/PC-WJJ/Desktop/jojojojo/output'
    })
})
```

## 8、扩展
- 1、fis与fis3

- 2、fis3工作原理：基于文件对象，每个进入 FIS3 的文件都会实例化成一个 File 对象，整个构建过程都对这个对象进行操作完成构建任务。

- 3、fis3与webpack：静态资源关系表
