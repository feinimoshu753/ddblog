---
title: 常见css布局问题整理
date: 2017-09-18 21:58:12
tags: 前端
---
### 一、box-sizing
box-sizing为css3属性，有border-box和content-box两个属性。效果如图所示：

<!-- more -->

![box-sizing示意图](http://img.blog.csdn.net/20170918211755427?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDY0MTAxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

```
	.box-sizing-item{
        display: inline-block;
        width: 120px;
        height: 120px;
        padding: 5px;
        margin: 5px;
        border: 5px solid green;
        background-color: red;
        vertical-align: middle;
    }

    .box-sizing-border-box{
        box-sizing: border-box;
    }
    .box-sizing-content-box{
        box-sizing: content-box;
    }
    .box-sizing-inherit{
        padding: inherit;
    }

	<div class="box-sizing">
        <span class="box-sizing-item box-sizing-border-box">border-box</span>
        <span class="box-sizing-item box-sizing-content-box">content-box</span>
        <span class="box-sizing-item box-sizing-inherit">inherit</span>
    </div>
```
属性为border-box的span中的宽高里包括了padding和border，属性为content-box的span中的宽高不包括padding和border。属性为inherit则是继承父级的box-sizing属性，可以看出元素默认为content-box。个人觉得border-box属性更贴近使用习惯和理解，使用的话主要看各种场景了。
最后提一下兼容问题，ie8以上即可支持。

### 二、垂直居中
垂直居中的情况很多，比如容器高度固定、内容不固定，容器高度不固定、内容固定等等。
#### 1、单行文本固定高度容器垂直居中
![这里写图片描述](http://img.blog.csdn.net/20170918213935576?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDY0MTAxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
```
	.vertical-center-container-1 {
        width: 300px;
        height: 60px;
        background-color: red;
    }

    .vertical-center-content-1 {
        line-height: 60px;
    }

	<div class="vertical-center-container-1">
	    <p class="vertical-center-content-1">单行文本固定容器垂直居中</p>
	</div>
```
主要是利用和容器相同的line-height处理。

#### 2、多行文本固定高度容器垂直居中
![这里写图片描述](http://img.blog.csdn.net/20170918214450329?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDY0MTAxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

```
	.vertical-center-container-2 {
        display: table-cell;
        width: 300px;
        height: 120px;
        background-color: red;
        vertical-align: middle;
    }

    .vertical-center-content-2 {
        vertical-align: middle;
    }
	
	<div class="vertical-center-container-2">
	    <p class="vertical-center-content-2">多行文本固定容器垂直居中，我是多余的、我是多余的</p>
	</div>
```
主要是利用table-cell和vertical-align实现。

#### 3、固定高度内容不固定高度容器垂直居中
![这里写图片描述](http://img.blog.csdn.net/20170918215138803?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDY0MTAxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

```
    .vertical-center-container-3 {
        position: relative;
        width: 300px;
        height: 150px;
        background-color: red;
        vertical-align: middle;
    }

    .vertical-center-content-3 {
        position: absolute;
        width: 80px;
        height: 80px;
        top: 50%;
        left: 0;
        margin-top: -40px;
        background-color: green;
    }
	
	<div class="vertical-center-container-3">
	    <div class="vertical-center-content-3"></div>
	</div>
```
主要是利用绝对布局+负margin实现。

#### 4、flex布局实现垂直居中
![这里写图片描述](http://img.blog.csdn.net/20170918215917014?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDY0MTAxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

```
    .vertical-center-container-4 {
        width: 300px;
        height: 150px;
        display: flex;
        align-items: center;
        background-color: red;
    }

    .vertical-center-content-4 {
        display: inline-block;
        width: 80px;
        height: 80px;
        background-color: green;
    }

	<div class="vertical-center-container-4">
	    <div class="vertical-center-content-4">2号</div>
	</div>
```
flex布局实现各种情况的垂直居中都比较简单，目前移动端基本都可以正常使用，部分浏览器需要增加前缀，pc端的兼容性还是有一些问题。

### 三、圣杯布局和双飞翼布局
我也是最近才听说这两个布局的，听起来挺高端，现学现卖吧，这两个布局用在pc端比较多。效果是左右各有一个固定元素，中间元素可以缩放拉伸。
![这里写图片描述](http://img.blog.csdn.net/20170918221631081?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDY0MTAxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

方案一：
```
	.grail-layout-container{
        padding: 0 200px;
    }

    .grail-layout-middle{
        float: left;
        width: 100%;
        height: 200px;
        background-color: red;
    }

    .grail-layout-left{
        position: relative;
        float: left;
        width: 200px;
        height: 200px;
        left: -200px;
        margin-left: -100%;
        background-color: green;
    }

    .grail-layout-right{
        position: relative;
        float: left;
        width: 200px;
        height: 200px;
        right: -200px;
        margin-left: -200px;
        background-color: blue;
    }
    <div class="grail-layout-container">
	    <div class="grail-layout-middle">圣杯布局。我是多余的、我是多余的、我是多余的、我是多余的</div>
	    <div class="grail-layout-left"></div>
	    <div class="grail-layout-right"></div>
	</div>
```
方案二:
```
    .grail-layout-middle-2{
        float: left;
        width: 100%;
        height: 200px;
        background-color: red;
    }

    .grail-layout-middle-inner-2{
        margin: 0 200px;
    }

    .grail-layout-left-2{
        float: left;
        width: 200px;
        height: 200px;
        margin-left: -100%;
        background-color: green;
    }

    .grail-layout-right-2{
        float: left;
        width: 200px;
        height: 200px;
        margin-left: -200px;
        background-color: blue;
    }

	<div class="grail-layout-container-2">
	    <div class="grail-layout-middle-2">
	        <div class="grail-layout-middle-inner-2">圣杯布局2。我是多余的、我是多余的、我是多余的、我是多余的</div>
	    </div>
	    <div class="grail-layout-left-2"></div>
	    <div class="grail-layout-right-2"></div>
	</div>
```
方案二和方案一相比多了一个内部div，但是没有使用position:relative。

### 四、flex相关内容速记
#### 1、flex容器
```
flex-direction：
row（水平排列）、column（垂直排列）、row-reverse （反向水平排列）、column-reverse（反向垂直排列）。
```
```
flex-wrap:
wrap（换行）、nowrap（不换行）、wrap-reverse（反向换行）
```
```
flex-flow:
flex-direction、flex-wrap组合到一起的属性。
```

```
justify-content:
flex-start（居左）、flex-end（居右）、center（居中）、space-between（左右排布）、space-around（等分排布）
```
```
align-items:
flex-start（居上）、flex-end（居下）、center（居中）、stretch（项目和容器高度一致、默认值）、baseline（基线对齐）
```
```
align-content:
支持多行的align-items，没有baseline属性。
```
#### 2、flex项目
```
order:
容器内项目排序,默认为0.
```
```
flex-grow:
值为0和1，默认为0不占满容器、1为占满容器。
```
```
flex-shrink:
值为0和1，默认为1可以被其他容器挤压，0为不可被其他容器挤压。
```
```
flex-basis:
设置项目的初始宽度值，默认为auto。
```
```
flex:
flex-grow、flex-shrink、flex-basis组合到一起的属性。
绝对flex项目:flex:1或者flex:1 1 0，等分排布。 
相对flex项目:flex:auto或者flex: 1 1 auto，按项目大小排布。
```
```
align-self:
flex-start（居上）、flex-end（居下）、center（居中）、stretch（项目和容器高度一致、默认值）、baseline（基线对齐）、比align-items多了一个auto（默认为flex容器属性）
```
