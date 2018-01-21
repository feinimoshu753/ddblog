---
title: effective javascript(七)——命名函数表达式
date: 2017-04-20 23:21:22
tags: 前端
---
```
function car(){return 'Benz';}
```
这是一个简单的的函数声明，我们也可以将它作为一个函数表达式。例如：

<!-- more -->

```
var funA = function car(){return 'Benz';}
```
根据ECMAScript规范，此语句将该函数绑定到变量funA，而不是变量car。当然给函数表达式命名是没有必要的，我们可以改为下面的匿名函数表达式的形式：

```
var funA = function(){return 'Benz';}
```
匿名和命名函数表达式的官方区别在于后者会绑定到与其函数名相同的变量中，该变量将作为该作为该函数内的一个局部变量。这可以用来写递归表达式。

```
var funB = function search(tree,key){
    if(!tree){
        return null;
    }
    if(tree.key === key){
        return tree.value;
    }
    return search(tree.left,key) || search(tree.right,key)
}
var tree = {key: 'a',value: 1 ,left: {key: 'b',value: 2},right: {key : 'c', value: 3}};
funB(tree,'b'); // 2
search(tree,'b'); //search is not define
```
其实上面的函数完全可以写成：

```
var funB = function(tree,key){
    if(!tree){
        return null;
    }
    if(tree.key === key){
        return tree.value;
    }
    return search(tree.left,key) || search(tree.right,key)
}
```
或者是

```
function search(tree,key){
    if(!tree){
        return null;
    }
    if(tree.key === key){
        return tree.value;
    }
    return search(tree.left,key) || search(tree.right,key)
}
var funB = search;
```
一般来说，命名函数表达式是没有必要的。


----------
end...