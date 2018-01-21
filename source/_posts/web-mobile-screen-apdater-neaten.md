---
title: 移动端web屏幕适配方案整理
date: 2017-09-19 07:28:00
tags: 前端
---
网上关于移动端web适配的问题已经有很多很好的文章了，做一个简单的整理。

<!-- more -->

### 常用viewport属性
#### 1、width
常用设置width=device-width，视口宽度等于屏幕宽度
#### 2、initial-scale、maximum-scale、minimum-scale
常用设置initial-scale=1, maximum-scale=1, minimum-scale=1，初始化缩放、最大缩放、最小缩放都为1；或根据屏幕dpr动态计算缩放比例。
#### 3、user-scalable
常用设置为user-scalable=no，不允许用户缩放。
#### 4、target-densitydpi
常用设置target-densitydpi=device-dpi，资料显示部分浏览器已经废弃。

### 常用方案
#### 1、宽度使用百分比，高度自适应
viewport 配置 width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no。 
比较适合列表排列和一些布局比较简单的情况，列表图片可能需要在服务端进行统一处理，遇到需要设置的高度时候，可能需要动态计算。

![这里写图片描述](http://img.blog.csdn.net/20170920060552312?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDY0MTAxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

案例:腾讯新闻移动端。

#### 2、使用rem，dpr默认为1
viewport配置width=device-width,initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no。html标签设置dpr为1、根据屏幕尺寸动态计算html标签font-size。
页面布局相对灵活，图片高度可以设置rem，不依赖服务端处理。如果整体布局全部使用rem，各种手机展示效果基本一致，包括每行文字显示个数。

案例:网易新闻移动端。

#### 3、使用rem，根据devicePixelRatio设置dpr。
viewport配置initial-scale=1/dpr, maximum-scale=1/dpr, minimum-scale=1/dpr, user-scalable=no。html标签设置dpr为window.devicePixelRatio、根据屏幕尺寸动态计算html标签font-size。
本方案和方案2类似，不过对高清屏进行了相应的处理。字体用px需要进行dpr的适配处理，解决了1px显示问题。

案例：淘宝移动端

#### 4、使用target-densitydpi
viewport配置width=640, user-scalable=no, target-densitydpi=device-dpi。
和方案1效果类似。

案例：荔枝FM移动端

最后记录一个使用rem的小问题，图片使用rem设置等宽高，并设置border-radius为50%时，部分浏览器出现头像不是圆形的问题，应该是rem转换为px带有浏览器不支持的小数点导致的。

#### 参考文章:
[网易和淘宝移动WEB适配方案再分析](https://zhuanlan.zhihu.com/p/25216275)
[使用Flexible实现手淘H5页面的终端适配](https://github.com/amfe/article/issues/17)
[移动端适配方案(下)](http://web.jobbole.com/90084/)
[viewports剖析](http://www.w3cplus.com/css/viewports.html)
