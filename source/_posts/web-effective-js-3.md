---
title: effective javascript(三)——原始类型优于封装对象
date: 2017-04-16 20:22:51
tags: 前端
---
javascript除了object外一共有5个原始类型：布尔值、数字、字符串、null和undefined。（其中null类型进行typeof操作结果为“object”）

<!-- more -->

###字符串原始类型和封装对象异同
在某些地方，String对象的行为和封装的字符串值类似，如字符串连接:

```
var str = new String("learn");
str + " javascript" = "learn javascript";
```
如提取索引的值:

```
var str = new String("learn");
str[2] = a；
```
不同之处在于，原始字符串type为string，String对象的type为object。

```
typeof "learn" = string；
var str = new String("learn");
typeof str = object；
```
两个String对象比较，可以看出new的String对象，虽然值都等于"learning"，但str1和str2并不相等。

```
var str1 = new String("learning");
var str2 = new String("learning");
str1 === str2 // false
str1 == str2 // false
```
原始的字符串类型可以调用String原型对象里的方法，如toUpperCase:
```
"learn".toUpperCase() = LEARN；
```
可以对原始字符串类型设置属性值，但对原始字符串没有任何影响:
```
"learn".testProperty = 3;
"learn".testProperty = undefined;
```
这是因为每次原始字符串类型调用某个方法时，会进行隐式封装产生一个新的String对象，所以不会对之后的字符串进行影响。


----------


end...


