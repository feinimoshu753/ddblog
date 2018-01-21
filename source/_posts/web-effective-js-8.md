---
title: effective javascript(八)——eval函数
date: 2017-04-24 23:07:42
tags: 前端
---
javascript中的eval函数功能十分强大，错误使用eval函数的方法之一就是允许它干扰作用域。下面来看个例子：

<!-- more -->

```
function demo(a) {
    eval("var b = a");
    return b;
}
demo("hello")// hello
```
放在eval函数中声明的变量b和直接放在demo函数体中略有不同。只有当eval函数被调用时，eval函数中的var声明语句才会被调用。下面再看一个例子：

```
var b = "global";
function demo(a) {
    if(a){
        eval("var b = 'local';");
    }
    return b;
}
demo(true)// local
demo(false)// global
```
eval函数有能改变局部作用域内变量的能力，错误的使用eval函数可能造成一些难以预测的问题。

```
function demo(b) {
    var a = 'local';
    eval(b);
    return a;
}
demo('var a = "eval_local"')// eval_local
demo('var c = "eval_local"')// local
```
为了避免eval函数改变局部变量，解决办法之一是将eval函数放在一个闭包中。

```
function demo(b) {
    var a = 'local';
    (function () {
        eval(b);
    })();
    return a;
}
demo('var a = "eval_local"')// local
demo('var c = "eval_local"')// local
```


----------
end...
