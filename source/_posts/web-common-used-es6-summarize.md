---
title: es6常用知识总结
date: 2017-10-03 07:13:36
tags: 前端
---
es6已经出了2年左右的时间了，虽然部分浏览器没有支持es6，不过在babel等帮助下，我们依然可以使用es6相关的内容，现在对es6常用内容的总结。

<!-- more -->

### 一、变量解构
变量结构是按照一堆的规则从数组或者对象中提取值并赋予给变量。

```
	数组解构:
	let [x, y, z] = [1, 2, 3];
    console.log(x); //1
    console.log(y); //2
    console.log(z); //3
	
	解构默认值:
    let [a = 1] = [];
    console.log(a); //1
    let [b = 2] = [undefined];
    console.log(b); //2
    let [c = 3] = [4];
    console.log(c); //4

	解构变量赋值:
    let [d = 1, e = d] = [5];
    console.log(d); //5
    console.log(e); //5
	
	对象解构:
    let {football, basketball} = {basketball: "dunk", football: "shoot"};
    console.log(football); //"shoot"
    console.log(basketball); //"dunk"
	
	结构失败:
    let {baseball} = {basketball: "dunk", football: "shoot"};
    console.log(baseball); //undefined
	
	对象解构完整写法:
    let {football: pingpong} = {basketball: "dunk", football: "shoot"};
    console.log(pingpong); //"shoot"
	
	复合解构:
    let {f: {g, h}} = {f: {g: 1, h: 2}};
    console.log(g); //1
    console.log(h); //2

	字符串解构:
    const [i, j, k] = 'abc';
    console.log(i); //'a'
    console.log(j); //'b'
    console.log(k); //'c'
    let {length: len} = 'kkkkk';
    console.log(len); //5
	
	函数内解构应用:
    function arrayTest([a, b, c]) {
        console.log(a + b + c); 
    }
    arrayTest([1, 2, 3]); //6
    
    function objTest({a, b}) {
        console.log(a + b); 
    }
    objTest({a: 1, b: 3}); //4
```
### 二、字符串拓展
常用的字符串拓展:

 - for-of循环
 - 模板字符串

```
	for-of循环:
	let a = 'abcde';
    for(let i of a){
        console.log(i); //a b c d e
    }
    
	模板字符串:
    let dd = 'amazing';
    document.body.innerHTML = `<div>
        <p>
           string template test ${dd}
        </p>
    </div>`;
    //<div><p>string template test amazing</p></div>
```
### 三、数组拓展
常用的数组拓展:

 - 拓展运算符(...)
 - Array.from

```
	拓展运算符:
	console.log(...['x', 'y', 'z']); //'x' 'y' 'z'

    let a = [];
    a.push(...[1, 2, 3]);
    console.log(a); //[1,2,3]

    function test(x, y) {
        console.log(x + y); 
    }
    test(...[1, 2, 3]); //3

    let b = [1,2,3];
    let c = [...b];
    console.log(c); //[1,2,3]

    console.log([...[1,2],...[3,4],...[5,6]]); //[1,2,3,4,5,6]
    
	Array.from方法（将类数组转换为数组）:
    function fromTest() {
        console.log(Array.from(arguments)); 
    }
    fromTest('a','b','c'); //['a','b','c']
```
### 四、对象拓展
常用的对象拓展:

 - 对象简洁表达式
 - Object.assign

```
	对象简洁表达式:
	let a = 'simple';
    let b = {a};
    console.log(b); //{a:"simple"}

    let obj = {
        a,
        fun(){
            console.log('simple');
        }
    }
    obj.fun(); //'simple'

    let obj1 = {
        ['a' + 'bc']: 1,
        d: 2,
        ['f' + 'un'](){
            console.log('amazing'); //'amazing'
        }
    }
    console.log(obj1.abc); //1
    obj1.fun();
	
	Object.assign:
    let target = {a: 1, b: 2, c: 3};
    let source1 = {b: 3, d: 4};
    let source2 = {c: 4, e: 5};
    Object.assign(target, source1, source2);
    console.log(target); //{a: 1, b: 3, c: 4, d: 4,e: 5}
	
	Object.assign为浅拷贝:
    let obj2 = {a: 1, b: {c: 2}};
    let obj3 = Object.assign({}, obj2);
    obj2.a = 2;
    obj2.b.c = 3;
    console.log(obj3); //{a:1,b:{c:3}}
```
### 五、函数拓展
常用的函数拓展:

 - 参数默认值
 - rest参数
 - 箭头函数

```
	函数默认值:
	function defaultValue(x = 0, y = 0) {
        console.log(x + y);
    }
    defaultValue(); //0
    defaultValue(1); //1
    defaultValue(1, 2); //3
	
	rest参数:
    function restTest(...values) {
        console.log(values);
    }
    restTest(1, 2, 3, 4); //[1,2,3,4]

	箭头函数:
    var arrow = (x, y) => x + y;
    console.log(arrow(1, 2)); //3

    var arrow2 = () => {a:1};
    console.log(arrow2()); //undefined
    var arrow3 = () => ({a: 1});
    console.log(arrow3()); //{a: 1}
    //箭头函数返回对象需要加圆括号

	//箭头函数的this会保持不变
    var a = 1;
    function fun() {
        setTimeout(() => {
            console.log(this.a); //2
        }, 100);
        setTimeout(function () {
            console.log(this.a); //1
        },100);
    }
    fun.call({a: 2});
```

### 六、Set和Map
Set和和Map是新的数据结构，常用的情景是数组去重。

```
	数组去重:
	let a = new Set();
    [1, 2, 3, 3, 4, 5, 6, 1].forEach(i => a.add(i));
    console.log(Array.from(a)); //[1,2,3,4,5,6]

    let b = new Map();
    b.set('a', 1);
    b.set('b', 2);
    console.log(b); //Map(2) {"a" => 1, "b" => 2}
    let c = {a: 1};
    b.set(c, 3);
    console.log(b.get(c)); //3
```

### 七、class
新增了类的支持，统一了继承的写法。
```
	类的基本结构:
	class A {
        constructor(x, y) {
            this.x = x;
            this.y = y;
        }

        getValue() {
            console.log(this.x + this.y);
        }
    }

    let a = new A(1, 2);
    a.getValue(); //3
    let aa = new A(2, 2);
    aa.getValue(); //4

    a.x = 5;
    a.getValue(); //7
    aa.getValue(); //4

	类的继承:
    class B extends A {
        constructor(x, y) {
            super(x, y);
        }

        getOtherValue(){
            console.log(this.x - this.y);
        }
    }

    let b = new B(1,2);
    b.getValue(); //3
    b.getOtherValue(); //-1
```

好了，es6常用的一些方法已经总结完了，不过还有一些遗漏的:比如let和const，这个比较简单；还有es6模块化机制，这个放在模块化的文章中来说；当然还有最重要的异步操作Promise、Generator、async函数，这个也会单独开一篇文章来说。

over...