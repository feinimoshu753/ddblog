---
title: effective javascript(一)——认识javascript浮点数
date: 2017-04-12 23:35:31
tags: 前端
---
###一、javascript中所有数字的类型都为number类型###

<!-- more -->

```
typeof 10: number
typeof 15.8: number
typeof -30.2: number
```
###二、javascript中所有数字都是双精度浮点数，大多数的算术运算可以使用整数、实数或者两者进行计算###

```
0.5 * 1.7 = 0.85
-20 + 35 = 15
18 - 3.65 = 14.35
2.8 / 4 = 0.7
29 % 5 = 4
```
###三、位运算采用特殊的计算方式，javascript将操作数隐式转换为32位二进制后进行运算

```
5 | 8 = 13
(5).toString(2) = 101
(8).toString(2) = 1000
parseInt("1101",2) = 13
```
###四、浮点数计算精度问题（常见问题）

```
0.2 + 0.4 = 0.6000000000000001
0.5 + 0.4 = 0.9
(0.2 + 0.4) + 0.3 = 0.9000000000000001
0.2 + (0.4 + 0.3) = 0.8999999999999999
```
解决方法之一：计算结果使用toFixed方法保留一定的小数位数。

```
如：(0.2 + 0.4).toFixed(1)= 0.6
```


