---
title: vue+webpack+thinkphp多页应用配置
date: 2017-03-23 23:14:51
tags: 前端
---
去年一直开发的前端项目越来越大的时候，问题就逐渐凸显出来了。一个很重要的问题是视图无法复用，重复冗余的代码很多；视图是用模板引擎引擎写的，Html代码看起来也不方便，需要采用一些方案来解决问题。

<!-- more -->

观望和体验vue已经很长一段时间了，终于在3月的时候开始在尝试在项目中使用vue。首先遇到的一个问题就是vue的脚手架是单页的，而项目本身是多页的，需要进行多页处理；多页处理在网上已经有较多的文章介绍了，不过大多都是基于webpack1.0版本的。还有一个问题是前端项目是和thinkphp结合在一起的，路由由thinkphp来控制，所以文件需要打包到thinkphp对应的文件路径。

刚开始改webpack配置的时候出现各种问题，主要原因还是webpack相关的内容不熟悉，最后还是去webpack官网仔仔细细的把文档看了一下，终于把基本的配置搞定了。

中文官网地址：中文官网

英文官网地址：英文官网



### 一、项目结构

接下来介绍一下完整的项目结构（既包括了thinkphp的文件夹，也包括了vue脚手架的文件夹），如下：

![alt text](http://img.blog.csdn.net/20170323225226277?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDY0MTAxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

### 二、代码编写

开发的时候主要编写src文件夹下得文件，包括了assets，components，pages。assets放资源文件、components放vue的组件、pages下面放thinkphp下对应的模块名，模块下面再放对应的页面文件夹，页面文件夹包括一个app.vue文件，一个index.html和一个index.js。

![alt text](http://img.blog.csdn.net/20170323225508922?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDY0MTAxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)


### 三、多页配置及使用

主要更改了vue脚手架中的webpack.base.conf.js、webpack.prod.conf.js、webpack.dev.conf.js；将entry改为了多入口，生产环境打包时，将路径调整为thinkphp对应的路径；具体配置参考下面github项目。

npm run dev在生产环境中编写和调试页面效果

npm run build打包到thinkphp对应的目录下



### 四、存在的部分问题

1、因为使用的thinkphp3.2.3版本，所有View文件夹下得模块需要大写，所以pages下面的文件夹为大写，打包出来resouse文件夹下面也为大写。

2、每个页面都有一个html文件，可能大部分html文件都是相同的，考虑使用同一html文件打包


github地址如下：https://github.com/feinimoshu753/vue-webpack-mutilpage

有什么问题欢迎大家留言讨论。





