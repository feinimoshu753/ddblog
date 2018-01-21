---
title: canvas入门学习
date: 2017-10-02 17:49:55
tags: 前端
---
canvas是一个可以使用js在其中绘制图形的 HTML元素，同时也可以绘制动画等内容，今天进行简单的入门学习。

<!-- more -->

#### 绘制矩形

```
<canvas id="canvas-demo" width="500" height="500"></canvas>
var canvas = document.getElementById('canvas-demo');
var context = canvas.getContext('2d');
context.fillStyle = '#00ffff';
context.strokeStyle = '#ff00ff';
context.fillRect(20,20,100,100);
context.clearRect(30,30,80,80);
context.strokeRect(40,40,60,60);
```
![这里写图片描述](http://img.blog.csdn.net/20171002174026415?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDY0MTAxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### 绘制三角形

```
context.beginPath();
context.moveTo(20,20);
context.lineTo(200,200);
context.lineTo(20,200);
context.fillStyle = '#00ffff';
context.fill();

context.beginPath();
context.moveTo(40,20);
context.lineTo(220,20);
context.lineTo(220,200);
context.closePath();
context.strokeStyle = '#00ffff';
context.stroke();
```
![这里写图片描述](http://img.blog.csdn.net/20171002174246943?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDY0MTAxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### 绘制圆形

```
context.strokeStyle = '#00ffff';
context.beginPath();
context.arc(100,100,50,0,Math.PI * 2);
context.stroke();

context.fillStyle = '#ffff00';
context.beginPath();
context.arc(150,100,50,0,Math.PI);
context.fill();
```
![这里写图片描述](http://img.blog.csdn.net/20171002174420482?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDY0MTAxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### 二次贝塞尔曲线

```
context.strokeStyle = '#00ffff';
context.beginPath();
context.moveTo(20,20);
context.quadraticCurveTo(50,120,200,20);
context.stroke();
```
![这里写图片描述](http://img.blog.csdn.net/20171002174613062?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDY0MTAxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### 三次贝塞尔曲线

```
context.fillStyle = '#ff0000';
context.beginPath();
context.moveTo(20,20);
context.bezierCurveTo(60,200,160,200,200,20);
context.fill();
```
![这里写图片描述](http://img.blog.csdn.net/20171002174726481?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDY0MTAxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### 使用Path2D绘制矩形

```
context.strokeStyle = '#ff0000';
var rectangle = new Path2D();
rectangle.rect(20,20,150,100);
context.stroke(rectangle);
```
![这里写图片描述](http://img.blog.csdn.net/20171002174852675?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDY0MTAxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

over...