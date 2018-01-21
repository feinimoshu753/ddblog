---
title:【web前端】记webApp开发记录
date: 2016-06-10 16:20:37
tags: 前端
---
在做这个webapp之前，一直在使用的是响应式的网页，在移动端的效果也只能说是能看，但是效果总觉得不是那么好（特别是一些稍微复杂的页面，很难达到pc和移动端都有良好的效果），经过了近半个月的资料查询整理，加上和技术总监的沟通，最终决定单独做一个webapp。。从5月初开始准备webapp的开发到现在大概有一个月的时间，总共完成的页面18个左右，主要是一些浏览查看的功能，交互之类的操作比较少，下面主要介绍一下开发的经历。

<!-- more -->

### 第一步：ui框架的选择

为了减少开发的工作量，所以决定选择一个ui框架进行基础开发。查了各个地方的资料之后，选出了四个ui框架选择，分别是WeUI（微信出品）、Frozen UI（手Q出品）、SUI-Mobile（阿里出品）、mui（不太了解尴尬）。最终选择阿里的sui-mobile进行基础框架进行开发，这个框架提供了挺多不错的控件，大小也比较合理。下面贴出总结的四个ui框架的特点：

 #### 1、WeUI

 微信原生视觉体验一致的基础样式库。

 特点:提供微信的ui效果，样式较少，没有类似侧边栏等插件。

 js+css大小:40k

 github地址:https://github.com/weui/weui

 #### 2、Frozen UI

 基于手机QQ的样式库。

 特点:提供手机qq的ui效果，样式比较丰富，有少量的插件和动效库，但较长时间未更新。

 js+css大小:83k

 github地址:https://github.com/frozenui/frozenui

 #### 3、SUI-Mobile（iOS风格）

 基于Framework7开发的UI库

 特点:提供类似ios的ui效果，样式比较丰富，有少量的插件。

 js+css大小:52k

 github地址:https://github.com/sdc-alibaba/SUI-Mobile

 #### 4、mui（iOS风格）

 适合webApp的前端框架，封装了原生api。

 特点:提供类似ios的ui效果，样式比较丰富，不依赖任何第三方js库，在webapp上有较好的体验。

 js+css大小:160+k

 github地址:https://github.com/dcloudio/mui

 mui适用场景说明：

 为解决HTML5在低端Android机上的性能缺陷，mui引入了原生加速，其中最关键的就是webview控件，因此mui若要发

 挥其全部能力，需和5+ App配合适用，若脱离5+ App，mui功能会受限。



### 第二步 ：基础框架选择

1、sui-mobile自带zepto，所以选择zepto而放弃了jquery。

2、使用了淘宝适配框架flexible.js，基本达到各手机浏览一直的效果。（默默吐槽一句：虽然sui-mobile和flexble.js都是淘宝出的，但是用在一起的时候还得处理处理微笑，两个框架都用了rem，但是基准不同，得自己调整。）https://github.com/amfe/lib-flexible

3、使用了layer-mobile，一个小巧的弹窗框架。http://layer.layui.com/mobile

4、使用了arttemplate模板引擎，一个性能良好的模板引擎，简化了代码书写。https://github.com/aui/artTemplate

5、使用了jplayer，良好的多媒体播放功能。http://www.jplayer.cn/developer-guide.html



### 第三步：各页面编写

这个就没有太多好说了，按照设计图一一实现就好了，sui-mobile有很多现成的代码可以使用。



### 遇到的问题：

1、sui-mobile框架默认会拦截a标签，同时会有一个加载的动画。按照官方文档做法导致页面白屏，最后在每个a标签处加上external。

2、使用sui-mobile框架无限滚动时，需要加上$.init()。

3、上面提到的sui-mobile和flexble冲突，需要删除sui-mobile的css中代码如下。

``` 
'@media only screen and (min-width:400px){html{font-size:21.33px!important}}@media only screen and (min-width:414px){html{font-size:22.08px!important}}@media only screen and (min-width:480px){html{font-size:25.6px!important}}'
``` 


