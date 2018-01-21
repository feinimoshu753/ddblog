---
title: js数组常见使用整理
date: 2017-10-01 07:47:34
tags: 前端
---
数组是js中最常见的数据结构，同时js的数组功能十分强大，本次整理经常使用的一些数组方法，同时实现两个简单的功能。

<!-- more -->

### 常见方法
使用两个简单数组:
```
var arr1 = ['a', 'b', 'c', 'b'];
var arr2 = ['x', 'y', 'z'];
```
#### 1、Array.isArray()
判断一个变量是否为数组。
```
console.log(Array.isArray(arr1)); //true
```

#### 2、concat()
数组合并，返回一个新数组。

```
console.log(arr1.concat(arr2));  //['a','b','c','b','x','y','z']
```

#### 3、every()
判断数组中每个元素是否都满足条件。

```
var everyResult = arr1.every(function (value, index, array) {
    return value.length === 1;
});
console.log(everyResult); //true
```
#### 4、filter()
获取数组中满足条件的元素，并返回一个新数组。

```
var filterResult = arr1.filter(function (value, index, array) {
    return value !== 'b';
});
console.log(filterResult);//['a','c']
```
#### 5、forEach()
数组循环。
```
arr1.forEach(function (value) {
    console.log(value);  //a、b、c、b
});
```

#### 6、indexOf()
获取元素所在数组位置（正向）

```
console.log(arr1.indexOf('a')); //0
```

#### 7、join()
将数组元素使用某种内容分割，并拼接成一个字符串。

```
console.log(arr1.join(',')); //a,b,c,b
```

#### 8、lastIndexOf()
获取元素所在数组位置（反向）

```
console.log(arr1.lastIndexOf('m')); //-1
```
#### 9、map()
对数组中每个元素按某一方法进行处理，并返回一个新数组

```
var mapResult = arr1.map(function (value) {
    return value + value;
});
console.log(mapResult); //['aa','bb','cc','bb']
```
#### 10、pop()
删除数组中最后一个元素并返回。
```
var popResult = arr1.pop();
console.log(popResult); //'b'
console.log(arr1); //['a','b','c']
```
#### 11、push()
在数组末尾添加一个元素，返回数组长度。
```
var pushResult = arr1.push('d');
console.log(pushResult);  //4
console.log(arr1); //['a','b','c','d']
```

#### 12、reverse()
数组倒序排列。

```
arr1.reverse();
console.log(arr1); //['d','c','b','a']
```

#### 13、shift()
删除数组的第一个元素。

```
var shiftResult = arr1.shift();
console.log(shiftResult); //d
console.log(arr1); //['c','b','a']
```

#### 14、slice()
复制一个元素，可以选择数组起始位置。

```
var sliceResult = arr1.slice(1,3);
console.log(sliceResult); //['b','a']
```

#### 15、some()
判断数组中是否有一个元素满足条件。

```
var someResult = arr1.some(function (value) {
       return value === 'c';
});
console.log(someResult);  //true
```

#### 16、sort()
数组排序方法。默认排序顺序是根据字符串Unicode值。
```
var sortResult = [1,2,3,10,20,25].sort();
console.log(sortResult); //[1,10,2,20,25,3]
```

#### 17、splice()
从数组中的某个位置删除或者添加元素。

```
var spliceResult = arr1.splice(1,1,'e','f','g');
console.log(spliceResult); //b
console.log(arr1); //['c','e','f','g','a']
```
#### 18、unshift()
在数组起始位置添加一个元素。
```
var unshiftResult = arr1.unshift('m');
console.log(unshiftResult); //6
console.log(arr1); //['m','c','e','f','g','a']
```

### 二、数组去重处理

#### 1、利用set和...
```
var arr3 = ['a','b','c','a','d','b'];
var newArray = new Set(arr3);
console.log([...newArray]); //["a", "b", "c", "d"]
```

#### 2、利用filter

```
var arr3 = ['a','b','c','a','d','b'];
var newArray = arr3.filter(function (value,index,array) {
    return index === array.indexOf(value);
});
console.log(newArray); //["a", "b", "c", "d"]
```

### 三、数组排序
#### 1、利用sort方法
```
var arr4 = [1,5,3,7,8,4,6];
var newArray = arr4.sort(function (a,b) {
    return a-b;
});
console.log(newArray); //[1, 3, 4, 5, 6, 7, 8]
``` 

over...