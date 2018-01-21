---
title: 前端跨域问题
date: 2017-09-25 07:32:34
tags: 前端
---
跨域在前端开发中是一个很常见的问题，为什么会出现跨域这种情况呢？主要是为了解决安全问题。首先看一下跨域是怎么造成的：

<!-- more -->

### 浏览器的同源策略
MDN的解释：同源策略限制从一个源加载的文档或脚本如何与来自另一个源的资源进行交互。这是一个用于隔离潜在恶意文件的关键的安全机制。
浏览器的同源策略限制了不同源之间的html或者js进行互相交互的可能，可以很好的防止有些网站恶意获取其他网站数据，当然也限制了部分页面间的交互功能。

### 发生跨域的情况

 - 不同域名（包括不同子域名）
 - 不同协议
 - 不同端口号

### 常见的跨域场景

 - 获取页面内iframe高度
 - 与子页面通信
 - ajax请求

前两个场景出现的情况都比较少，当然也有很多解决方案（如增加一个代理页面，通过document.domain等等）。主要说一下ajax请求的跨域问题，这是一个非常常见的问题。
### 解决方案：
#### **一、jsonp**
JSONP（JSON with Padding）是一种json的使用方式，用来处理跨域问题。Jsonp通过在页面内定义一个方法，同时插入一个script脚本，然后服务端返回数据放入脚本请求url中callback方法，然后页面就可以在之前定义的方法中取到数据。
#### 前端：
```
<script type="text/javascript">
	function todo(response){
		...
	}
</script>
<script src="http://www.test.com/datas.php?callback=todo"></script>
```
#### 服务端：

```
<?php
	$callback = $_GET['callback'];
	$datas = array('data1','data2');
	echo $callback.'('.json_encode($datas).')';
?>
```
#### 优点
 - 封装后使用比较方便，无其他属性配置
 - 兼容老版本浏览器

#### 缺点

 - 仅能用于get请求
 - 存在安全性问题

#### 二、CORS
CORS(Cross-origin resource sharing)全称为跨域资源共享。主要可以解决ajax跨域的问题。使用CORS主要配置：
#### 前端

```
var xhr = new XMLHttpRequest();
xhr.withCredentials = true; //支持跨域传递cookie
```
#### 服务端
```
Access-Control-Allow-Origin: http://example.com
Access-Control-Allow-Credentials: true
```
#### 优点

 - 配置简单，服务端可以全局配置或者单个接口配置
 - 支持所有网络请求类型

#### 缺点

 - 需要传递cookie只能配置一个域名
 - 有兼容性问题，不过大部分浏览器已经支持


#### 参考文章
[跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html)
[前端跨域整理](https://zhuanlan.zhihu.com/p/24101549)
[前端跨域及其解决方案](https://juejin.im/entry/57877e927db2a2005cd58f38)
[前端跨域问题及解决方案](https://github.com/wengjq/Blog/issues/2)

over...