---
title: 函数柯里化及自定义bind
date: 2017-09-29 07:36:50
tags: 前端
---
函数柯里化(Currying)：是把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术。

<!-- more -->

上面是维基百科的解释，看着有点懵逼，还是用代码来测试一下。

```
		function currying(fn) {
            if (arguments <= 0) {
                return;
            }
            var args = Array.prototype.slice.call(arguments, 1);

            return function () {
                var innerArgs = Array.prototype.slice.call(arguments);
                var totalArgs = args.concat(innerArgs);
                return fn.apply(null, totalArgs);
            }
        }

        function totalFee(price, number) {
            return price * number;
        }

        var appleTotalFee = currying(totalFee, 8388);
        console.log(appleTotalFee(2)); //16776
        var huaWeiTotalFee = currying(totalFee, 3288);
        console.log(huaWeiTotalFee(5)); //16440
```
定义了一个curring函数，第一个参数为所需要柯里化的函数，然后获取柯里化时传递的其他参数，同时返回的函数将外层参数和内部参数合并，通过apply方法进行传递。这里定义了一个计算总价的函数，需要传递一个price和number，通过curring函数生成了一个appleTotalFee和huaWeiTotalFee函数，分别可以计算iphone x和huawei p10的总价。由结果可知2个iphone x可以购买5个huawei p10。。。

#### 函数柯里化的一个应用(自定义bind函数)
bind函数是在es5出现的，ie6~ie8不支持。所以需要自定义一个bind函数进行浏览器兼容。上代码
```
	if(!Function.prototype.bind){
		Function.prototype.bind = function (context) {
	        var that = this;
	        var args = Array.prototype.slice.call(arguments, 1);

	        return function () {
	            return that.apply(context, args);
	        }
	    }
    }

    var name = 'limei';
    var object = {
        name: 'hanleilei'
    }
    function getName(sex) {
        console.log(this.name + ' is a ' + sex);
    }
    getName('girl'); //limei is a girl
    var bindFun = getName.bind(object,'boy');
    bindFun(); //hanleilei is a boy
```
再来一个别的版本
```
	function curryedBind(fn, context) {
        var args = Array.prototype.slice.call(arguments, 2);

        return function () {
            var innerArgs = Array.prototype.slice.call(arguments);
            var totalArgs = args.concat(innerArgs);
            return fn.apply(context, totalArgs);
        }
    }

    var name = 'limei';
    var object = {
        name: 'hanleilei'
    }
    function getName(sex) {
        console.log(this.name + ' is a ' + sex);
    }
    getName('girl');
    var bindFun = curryedBind(getName, object, 'boy');
    bindFun();
    var bindFun2 = curryedBind(getName, object);
    bindFun2('boy');
```
两个实现其实差不多，不过第一个版本是将bind方法绑定到没有bind方法的Function的原型上，第二个版本直接定义了一个bind函数，可以直接调用。

函数柯里化我到现在都没有在实战中用到过。估计是开发时还没有这种应用概念，以后有机会在项目中用用才行。。

over...