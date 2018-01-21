---
title: effective javascript(四)——避免对于混合类型使用==运算符
date: 2017-04-17 23:16:43
tags: 前端
---
###先来看一个简单的例子：###
```
"1.0e0" == {valueOf: function () { return true;}}
结果为：true
```

<!-- more -->

这两个看似毫无关系的对象，使用"=="符号的结果却是true。js的隐式强制转换看似比较方便，也埋下了许多隐患。左边的等式被转换为1，右边的匿名函数结果为true，同样被转换为1，所有结果为true。
所有在作相等运算符的时候，尽量使用“===”符号，避免强制转换隐藏的问题。

###js中的“==”运算符部分强制转换规则如下：###

```
null == undefined // true
null == "sss" // false
undefined == "sss" // false
undefined == NaN // false
undefined == true // false
undefined == 1 // false
```
null和undefined与其他类型使用“==”都返回false，但是null与undefined使用“==”返回true。

###再来看一个关于date的例子:###
```
var date = new Date("2017/4/15");
date == "2017/4/15" //false
```
new的date对象和字符串对象是不相等的，因为date对象被转换成了别的格式。

```
date.toDateString() = "Sat Apr 15 2017"
```
再次补充说明：js中的强制转换有些隐藏的处理，可能导致意外的情况发生，尽量避免使用"=="运算符。


----------
end...

