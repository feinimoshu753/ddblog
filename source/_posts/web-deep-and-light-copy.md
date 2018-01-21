---
title: js对象深拷贝和淺拷贝
date: 2017-09-27 07:28:36
tags: 前端
---
深拷贝和浅拷贝之间的差距主要为一点，修改拷贝目标对象的内容是否影响拷贝源对象。

<!-- more -->

### 浅拷贝的实现:
源对象和目标对象:
```
	var target = {},
        data = {
            undefined: undefined,
            empty: null,
            empty_string: '',
            number: 1,
            string: 'test',
            boolean: true,
            object: {
                number: 2
            },
            array: ['a', 'b'],
            fun: function () {
                console.log('test');
            },
            data: data
        };
```

#### 一、for-in循环
```
	for (var key in data) {
        target[key] = data[key];
    }
    
    target.number = 2;
    console.log(data.number);  //1

    target.string = 'copy';
    console.log(data.string);  //test

    target.object.number = 5;  
    console.log(data.object.number); //5

    target.array.push('c');  //["a", "b", "c"]
    console.log(data.array);

    target.fun = function () {
        console.log('copy');  
    }
    data.fun(); //test
```
二、Object.assign
```
	target = Object.assign({},data);
	
	target.number = 2;
    console.log(data.number);  //1

    target.string = 'copy';
    console.log(data.string);  //test

    target.object.number = 5;  
    console.log(data.object.number); //5

    target.array.push('c');  //["a", "b", "c"]
    console.log(data.array);

    target.fun = function () {
        console.log('copy');  
    }
    data.fun(); //test
```
可以看出target修改对象中的值类型,data中相对应的值未发生变化，但是修改了对象中的引用类型，data中相对象的值发生了变化。这是因为for-in循环和Object.assign仅复制了源对象最外层的属性，对于内层的对象仅复制了一份引用，所以修改了目标对象会影响源对象。

### 深拷贝实现
#### 一、递归for-in循环

```
	function deepCopy(tar, resource) {
            for (var key in resource) {
                var source = resource[key];
                if (Object.prototype.toString.call(source) === '[object Object]') {
                    tar[key] = deepCopy(tar[key] || {}, resource[key]);
                } else if(Object.prototype.toString.call(source) === '[object Array]'){
                    tar[key] = deepCopy(tar[key] || [], resource[key]);
                } else if(resource.hasOwnProperty(key)){
                    tar[key] = resource[key];
                }
            }
            return tar;
        }

        target = deepCopy(target, data);
		
		target.number = 2;
        console.log(data.number); //1

        target.string = 'copy';
        console.log(data.string);  //test

        target.object.number = 5;
        console.log(data.object.number);  //2

        target.array.push('c');
        console.log(data.array);  //["a", "b"]

        target.fun = function () {
            console.log('copy');
        }
        data.fun();
        console.log(target); //test
```

二、JSON.parse和JSON.stringify
```
        target = JSON.parse(JSON.stringify(data));
        
        target.number = 2;
        console.log(data.number);  //1

        target.string = 'copy';
        console.log(data.string);  //test

        target.object.number = 5;
        console.log(data.object.number);  //2

        target.array.push('c');
        console.log(data.array);  //["a", "b"]
```
可以看出深拷贝不会出现修改目标对象影响源对象的问题，但使用JSON.parse和JSON.stringify方法进行深拷贝将无法拷贝JSON不支持的类型（如function，undefined等）。

over...