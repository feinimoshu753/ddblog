---
title: effective javascript(二)——隐式强制转换
date: 2017-04-16 20:22:51
tags: 前端
---
###一、隐式强制转化示例###
js对类型错误相当宽容，一些类型会被强制转化成其他类型。进行-、*、/、和%等算术计算前，都会将参数转换为数字。+即代表了数字相加，也代表了字符串相连，+对字符串相连的优先级更高。

<!-- more -->


```
5 + true = 6
7 + 2 = 9
"hello" + "world" = "hello world"
"4" + 6 = "46"
4 + "6" = "46"
2 + 3 + "4" = "54"
2 + "3" + 4 = "234"
"12" * 6 = 72
"5" | "2" = 7
null + 17 = 17
var a;
a + 3 = NaN;
```
###二、一个特殊的类型NaN（not a number）
一个未定义的变量进行运算时会被转换为NaN。NaN的特殊之处为NaN不等于自身。同时判断一个变量是否为NaN，无法直接有isNaN进行判断（isNaN可以用于变量是否为数字的判断），非数字的变量isNaN同样返回true。但是NaN是唯一一个不等于自身的变量，可以通过不等于自身来判断一个变量是否为NaN。

```
var x = NaN;
x === NaN; //false
isNaN("abc") = true
isNaN(undefined) = true
isNaN({}) = true

//NaN不等于自身
var b = NaN;
b !== b; //true
var c = "abc";
c !== c; //false
var d = undefined;
d !== d; //false
var e = {};
e !== e; //false
var f = {valueOf : "abc"};
f !== f; //false
```
###三、valueOf和toString方法
js对象可以通过toString将自身转换为字符串，同时也可以通过valueOf转换为数字。当对象同时拥有valueOf和toString方法，对象进行+运算时，会优先执行valueOf方法。

```
"m" + {toString: function () {return "d"}} = "md"；
3 * {valueOf: function () {return "5"}} = 15；

var obj = {
	toString: function () {
		return "[object MyObject]";
	},
	valueOf: function () {
		return 13;
	}
}; 
"object: " + obj= "object: 13";
```
###四、真值运算###
js中一共有7个假值：false、0、-0、、""（空字符串）、NaN、null、undefined。其他所有值都为真值。由于数字和字符串有可能为假值，使用真值运算检查函数参数或者对象属性是否已定义不是绝对安全的。

```
function point(x,y) {
	if(!x){
		x = 2;
	}
	if(!y){
		y = 3;
	}
	return {x:x,y:y};
}
point(0,0); // {"x":2,"y":3}

function point2(x,y) {
	if(typeof x === "undefined"){
		x = 2;
	}
	if(typeof y === "undefined"){
		y = 3;
	}
	return {x:x,y:y};
}
point2(0,0); // {"x":0,"y":0}
point2(); // {"x":2,"y":3}
```


