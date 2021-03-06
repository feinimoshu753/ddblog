---
title: react-native从入门到放弃
date: 2018-03-26 22:54:53
tags: react-native
---
刚刚度过了繁忙的一个月，连续不断的需求让自己有点招架不住了。写的代码质量有些堪忧，又导致不断的修改bug，陷入了恶性循环中了，不过随着最近最后一个需求即将完结，终于抽空写写rn相关的内容了。这个标题略有写浮夸，主要是为了吸引眼球，也有自己感受的原因。

<!-- more -->

### 一、rn历史简介
大家都知道rn是facebook开源的一个框架，不过关于rn的历史大家可能不太清楚。facebook在客户端2.0版本的时候，将大部分页面使用web技术实现，当时大概是2011年，android还在2.3版本、ios还在5.0版本。可想而知，结果当然是扑街了，在当时网页在手机上的体验相当的糟糕，用户吐槽声一片，facebook只能迅速的换成原生的实现。不得不说虽然是一次失败的尝试，但是facebook算是混合应用的先驱者和探索者，这也为后来facebook开发rn打下了基础。
rn的idea是在2013年的一个极客大会上提出的，2014年7月facebook内部开始尝试使用这项技术，到了2015年3月，rn的ios版本正式开源，到了同年9月，rn的android版本也开源了。大概的发展历程如下:
![这里写图片描述](https://img-blog.csdn.net/20180326210445589?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQ2NDEwMTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
rn虽然开源了接近了3年的时间了，但依然还没有到达1.0的正式版本，目前到了0.54版本，期待2021年1.0版本上线的时候.......不得不说，rn是历史上第一个没到正式版本，github却有6w+星星的项目，大写的服气。
### 二、rn原理简介
![这里写图片描述](https://img-blog.csdn.net/20180326210724654?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQ2NDEwMTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
盗了一张别人的android端架构图（侵删），rn的架构分为3层，java/oc层、c++层以及js层。这个结构和app内嵌h5基本一致，同样涉及到js和native通信等，只是将webview层替换为了c++层。
### 三、前端上手rn
#### 1、rn对前端意味着什么
我个人的理解是rn能帮助前端开发者使用熟悉的方式开发出一款接近原生体验的App，前提是你对react比较熟悉。接近原生体验要从两个方面来说这个问题，一个方面是rn的实现方式就决定了rn开发出的app只能是接近原生的体验，另一方面是前端同学一般对客户端开发不熟悉，在某些需要客户端方面进行支持优化的地方很难去处理。
#### 2、前端上手难度
会React开发就能很快上手RN，还可以使用React全家桶哦。rn直接和react开发有很多相似的地方，同时也可以用是redux等react常用的第三方库，上手难度不大。
#### 3、前端遇到的小问题
- 元素超过一屏不滚动
web端的页面天生会滚动，如果元素超过了一屏就会出现滚动条，但是客户端上元素超过一屏是不会滚动的，需要在最外层套上一个能滚动的元素，比如scrollview或则listview等等。
- ios和android的平台差异
虽然rn尽量处理了android和ios两端之前的差异，但是还是两端还是有一些不一致的地方，比如有些控件的属性只在某一端能够使用，这个时候我们尽量避免使用这样的属性，采用两端效果能一致的实现方式来处理
- 长列表的选择
rn目前已经提供了listview和flatList两种长列表的方式，listview因为性能问题基本被放弃使用了，所以在选择长列表的时候尽量选择flatlist或者其他开源的列表组件。
### 四、rn打开的正确方式
![这里写图片描述](https://img-blog.csdn.net/20180326211321940?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQ2NDEwMTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这是看到别人的博客上写的一段话，看了之后挺有感触的，因为自己在写rn的时候也觉得这是一个需要多方合作的一种开发模式。之前了解到携程好像将客户端和前端合并为大前端团队了，客户端和前端的同学一起协作开发。客户端的同学在app容器处理上肯定是最好的选择，但是一般在react开发上面不是很熟悉，前端同学明显在react的开发上比客户端同学更熟悉和了解，但是一般对客户端的开发比较陌生。双方可以完成自己更擅长的部分，同时可以相互学习各自领域的内容，这是一种极好的提升的方式。
### 五、存在的问题
#### 1、不断更新的版本
![这里写图片描述](https://img-blog.csdn.net/20180326211513916?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQ2NDEwMTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这是rn版本的截图，3年时间已经更新了四十几个版本，不同版本之间差距还是挺大的，所以如何做好rn版本管理以及升级是一个非常重要的工作，主要是兼容性问题。
#### 2、问题排查困难
![这里写图片描述](https://img-blog.csdn.net/20180326211455828?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQ2NDEwMTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这是做rn经常会看到的一张图，大红的背景里提示你出现了错误，虽然说有调试工具可以帮助你定位错误，但是和web开发不同，rn报错有可能是前端爆的错误，也可能是客户端的错误，问题排查起来会比较苦难。
#### 3、长列表及动画性能不佳
- 长列表快速滚动时会出现白屏的情况
	长列表快速向上滚动时，列表会出现白屏的情况。这主要是由于滚动时native和js频繁通信，导致客户端渲染不及时所导致的问题。目前还没有特别好的解决办法，有些公司会尽量避免在rn上使用长列表来规避这样的问题，如果列表内容比较简单的话，白屏出现的可能性会比较小。
- 动画效果出现卡顿（特别是android手机上）
rn的实现出来的动画会比较卡顿，原因和上一个问题基本相同。可能得解决方法是将动画完全由原生实现，js端直接使用原生动画，这样就可以避免动画卡顿的问题。
#### 4、首页白屏问题
打包成Bundle包后一般会有几M，甚至十几M，导致首次渲染时白屏时间很长。
在最新款的手机上，白屏的时间很短，然而在低版本的android手机上白屏时间缺非常的长。当然首页白屏问题现在已经有很多方案进行处理，比如拆包和预加载，这两种都是比较常见的处理方案了。
#### 5、优秀的热更新方案
- 自搭还是第三方？
一般公司都会选择自己搭建热更新框架，比较很少有公司愿意把这种更新的业务交由其他公司来完成，小公司的话可以尝试使用微软的热更新方案。
- 如何做好兼容性处理？
热更新也会导致兼容性问题，比如在之前的bundle包里没有这个功能，或者功能的结构发生变化，导致老版本无法正常使用新版本的内容。可能得解决方案有:
1、控制版本，不兼容的版本就不进行热更新
2、对客户端开方的接口进行封装，屏蔽客户端内部的实现
3、不使用或者尽量少使用热更新方案，将rn作为一个类似原生模块嵌入到app中，每次发布版本才更新一次rn包
- 傲娇的苹果
苹果之前封杀过JSPatch等热跟新方案，没准苹果有出台什么规则不允许rn之类的热更新，所以也得做好心理准备。

到这就写的差不用多了，最近rn的尝试也是挺有意义的，虽然开发流程等并没有太多新的东西，但是对rn开发流程、整体的结构等有了更深入的了解，之后准备再写写有关rn框架相关的东西，更深入的去了解这个框架。目前来说，rn还不是一个比较成熟的框架，在大型项目中的应用还需要更加谨慎的看待，所以最后点题，rn从入门到放弃，期待1.0版本的到来。

over...