---
title: effective javascript(六)——掌握闭包
date: 2017-04-19 23:18:10
tags: 前端
---
###一、什么是闭包的？
MDN的解释：闭包是指那些能够访问独立(自由)变量的函数 (变量在本地使用，但定义在一个封闭的作用域中)。换句话说，这些函数可以“记忆”它被创建时候的环境。

<!-- more -->

直接看定义也比较难以理解，还是先看一个例子：

```
function funA() {
  var name = "hello";
  function hello() {
    window.console.log(name);
  }
  return hello;
}

var funB = funA();
funB(); //"hello"
```
var funB = funA();执行后,上面的funB可以等价于：

```
funB = function hello(){
	window.console.log(name);
}
```
调用funB的结果为"hello"，hello函数依然能够引用外部的name变量。可以说明funB函数依然保留了funA的环境，能够继续引用funA内的变量，funB就称为闭包。

###二、闭包可以引用外部函数的参数
```
function funA(firstName) {
  function displayName(lastName) {
    window.console.log(firstName + ' ' + lastName);
  }
  return displayName;
}

var funB = funA('jack');
funB('lee'); //"jack lee"
funB('young'); //"jack young"

var funC = funA('louise');
funC('nancy'); // "louise nancy"
funC('mark');  // "louise mark"	
```
上面的例子创建了funB和funC2个完全不同的函数，虽然他们都是通过funA进行创建的，但是它们是完全不同的对象。其中funB的firstName为jack，funC的firstName为louise。
###三、闭包可以更新外部变量的值

```
function myFunction(){
    var value = 'hello';
    return{
        set: function (newValue) {
            value = newValue;
        },
        get: function () {
            return value;
        },
        type: function () {
            return typeof value;
        }
    }
}

var func = myFunction();
log(func.type()); //string
log(func.get());  //hello
func.set(22);
log(func.type());  //number
log(func.get());  //22
```
func一共产生了3个闭包对象（set，get，type），它们共同访问value对象，set更新了value值，之后get和type可以查看到更新的结果。
###四、闭包的常见使用（容易出错）
html代码中有3个img标签，点击不同的img标签，查看不同图片的大图。事例如下：
```
html:
<img src="img1"/>
<img src="img2"/>
<img src="img3"/>
```
```
 var imgs = document.getElementsByTagName('img');
 for(var i=0;i<imgs.length;i++){
     imgs[i].onclick = function () {
        imgBrowser(imgs[i].getAttribute('src'));
     };
 }
```
表面上看这段代码没有任何问题，但是点击图片会报错。调用imgBrowser(imgs[i].getAttribute('src'))代码时，其中的i的值为3；因为for循环结束后，i的值为3，所以点击事件响应的结果都是i为3时的结果。
参考上面的代码，我们可以使用闭包来解决i值的问题：

```
var imgs = document.getElementsByTagName('img');
for(var i=0;i<imgs.length;i++){
    imgs[i].onclick = (function(j){
        return function () {
	        imgBrowser(imgs[j].getAttribute('src'));
        }
    })(i);
}
```


----------
end...