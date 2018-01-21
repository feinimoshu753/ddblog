---
title: 【sui-mobile踩坑之旅】实现横向滚动列表
date: 2016-08-07 11:41:59
tags: 前端
---
好久没有跟新文章了，心里有些小激动呢。。

今天来讲一下sui-mobile(阿里前端框架)所遇到的一个问题，实现横向滚动列表。

<!-- more -->

google了一上午，最后决定使用iscroll.js实现横向滚动。把iscroll.js引入工程，在页面加上横向滚动的代码，兴高采烈的打开网页，结果横向滚动不了。。。。第一反应是代码用错了，仔细核对了一下，再次尝试，还是滚动不了。。。。推测可能是和sui-mobile冲突了，把代码添加到一个没有使用sui-mobile的页面进行测试。果然，这个横向滚动式可以的。。。马上去sui-mobile的github上去查一下有没有人和我遇到一样的问题，还真查到一个提问的童鞋，但是他们的回答并没有太多的帮助，只是让我知道sui-mobile是自带iscroll的。。。。。我之前是多么没仔细看文档尴尬。。果断把引入的iscroll.js删了。。。

之后又试了一个小时，还是滚动不了，就采用了最简单的方法，一步步的打断点查看源码。

最开始定位到这句代码：

``` 
me.addEvent = function(el, type, fn, capture) {
       el.addEventListener(type, fn, !!capture);

};
``` 

同时对sm.js和isroll.js打断点发现，上面代码的type参数不一致，然后定位到了utils.addEvent这个位置

var eventType = remove ? utils.removeEvent : utils.addEvent,
                target = this.options.bindToWrapper ? this.wrapper : window;
这个地方没有发现什么什么问题，继续定位到了初始化（init方法），最终发现了不同的地方：

iscroll.js的代码是：

``` 
this.wrapper = typeof el == 'string' ? document.querySelector(el) : el;
this.scroller = this.wrapper.children[0];
this.scrollerStyle = this.scroller.style; // cache style for better performance
``` 

sm.js的代码是：

``` 
this.wrapper = typeof el === 'string' ? document.querySelector(el) : el;
this.scroller = $(this.wrapper).find('.content-inner')[0]; // jshint ignore:line
``` 

sm.js重写了iscroll.js，获取scroller的方法不同。。。


解决办法：

在需要滚动的区域加上'content-inner'这个类就ok。。



sui-mobile已经半年都没更新了。。哎。。不多说了，继续踩坑去了。。。
