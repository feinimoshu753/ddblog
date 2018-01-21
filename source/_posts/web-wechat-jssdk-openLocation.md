---
title: 微信jssdk调用地图(openLocation)小坑
date: 2017-09-11 22:57:32
tags: 前端
---
今天发现微信jssdk里调用微信地图的一个小坑，记录一下。具体调用如下：

<!-- more -->

```
wx.openLocation(function(){
	longitude: longitude,
	latitude: latitude,
	name: name,
	address: address
});
```
乍看之下调用没有什么问题，android可以正常调用，但是ios无法调起。amazing...然后alert出各个状态发现ios出现一个错误。

```
errMsg:openLocation:fail invalid_coordinate
```
后面查到是微信的经纬度必须传浮点数，而自己传了个字符串过去。后面检查了下文档，的确是这样，果然还是得仔细看文档才行。但是为啥android正常呢...
还有一个小问题openLocation需要使用火星坐标才能定位准确，所需getLocation的时候，type可以传gcj02。

----- over
