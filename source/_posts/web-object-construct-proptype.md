---
title: 对象、构造函数、原型
date: 2017-09-20 20:57:56
tags: 前端  
---
### 一、创建对象的两种方式

#### 1、对象字面量

<!-- more -->

```
   var person1 = {
        name: 'tt',
        age: 50,
        speak: function (text) {
            console.log(this.name + ' speak ' + text);
        }
    };
```

#### 2、构造函数
```
    function Person(name, age) {
        this.name = name;
        this.age = age;
        this.speak = function (text) {
            console.log(this.name + ' speak ' + text);
        }
    }

    var person1 = new Person('tt', 50);
```
### 二、对象原型

```
	Person.prototype.walk = function () {
        console.log(this.name + ' walk');
    };

    Person.prototype.sleep = function () {
        console.log(this.name + ' sleep');
    };
```
为Person构造函数增加两个原型方法walk和sleep，所有通过Person生成的对象都具有这两个方法。

### 三、继承
#### 1、使用Object.create生成新对象

```
	var person1 = {
        name: 'tt',
        age: 50,
        speak: function (text) {
            console.log(this.name + ' speak ' + text);
        }
    };

    var person2 = Object.create(person1);
    console.log(person2);

	person2.__proto__ === person1 //true
```
使用object.create方法生成person2，person2本身输出结果为{}，一个空的对象，但是person2的原型上`person2.__proto__`包含了person1的属性和方法，所以说person1继承了person2，不过是通过原型的方式继承的。
#### 构造函数继承
```
	function Person(name, age) {
        this.name = name;
        this.age = age;
        this.speak = function (text) {
            console.log(this.name + ' speak ' + text);
        }
    }

    Person.prototype.walk = function () {
        console.log(this.name + ' walk');
    };

    Person.prototype.sleep = function () {
        console.log(this.name + ' sleep');
    };

    function Kid(name, age) {
        Person.call(this, name, age);
    }
    Kid.prototype = Object.create(Person.prototype);
    Kid.prototype.constructor = Kid;
```
紧接着Person构造函数的列子，创建一个新的构造函数Kid,构造函数内使用了call方法，拷贝了一份Person构造函数内用this创建的属性和方法，然后同样使用Object.create方法将Person原型上的属性和方法拷贝到Kid的原型上，此时Kid原型的constructor指向Person，所以使用`Kid.prototype.constructor = Kid` 将Kid原型的constructor指向Kid。

#### 原型式继承
```
	function inheritObject(o) {
        //临时构造函数
        function F() {};

        F.prototype = o;
        return new F();
    }

    var kid1 = {
        name: 'allen',
        hobby: ['basketball','read book']
    }

    var kid2 = inheritObject(kid1);
    kid2.hobby.push('football');
    console.log(kid2.hobby); //["basketball", "read book", "football"]
    console.log(kid1.hobby); //["basketball", "read book", "football"]
    kid2.name = 'jim';
    console.log(kid2.name);  //jim
    console.log(kid1.name);  //allen
```
其中值类型的属性被复制，引用类型的属性被共用。

#### 寄生式继承

```
	function inheritObject(o) {
        //临时构造函数
        function F() {};

        F.prototype = o;
        return new F();
    }

    var kid1 = {
        name: 'allen',
        hobby: ['basketball','read book']
    }
    
    function createKid(o) {
        var obj = inheritObject(o);
        obj.getName = function () {
            console.log(this.name);
        }
        return obj;
    }

    var kid2 = createKid(kid1);
    console.log(kid2.getName);  //allen
```
寄生式继承其实是对原型式继承的封装，可以为新的对象增加新的属性和方法，同时返回新的对象。

#### 寄生组合式继承

```
	function inheritObject(o) {
        //临时构造函数
        function F() {
        };

        F.prototype = o;
        return new F();
    }
    
	function inheritPrototype(SubClass, SuperClass) {
        var temp = inheritObject(SuperClass.prototype);
        temp.constructor = SubClass;
        SubClass.prototype = temp;
    }

    function Father(name) {
        this.name = name;
        this.bag = ['card', 'money'];
    }

    Father.prototype.getName = function () {
        console.log(this.name);
    }

    function Son(name, action) {
        Father.call(this, name);
        this.action = action;
    }

    inheritPrototype(Son,Father);

    Son.prototype.getAction = function () {
        console.log(this.action);
    }

    var son1 = new Son('tom','run');
    var son2 = new Son('marry','jump');

    son1.bag.push('sugar');
    console.log(son1.bag); //["card", "money", "sugar"]
    console.log(son2.bag); //["card", "money"]

    son2.getAction(); //marry
    son2.getName();   //jump
```
寄生组合式继承将父类构造函数内的属性和方法复制了一份，同时共享了父类原型上的属性和方法。