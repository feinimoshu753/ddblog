---
title: effective javascript(五)——变量作用域
date: 2017-04-19 00:08:55
tags: 前端
---
javascript中创建全局变量十分简单，并不需要特别任何形式的声明就可以被整个程序的所有代码访问。
定义全局变量最大的问题是污染了命名空间，有可能会导致意外的冲突。
同时全局变量不利于代码模块化，容易导致独立组件、之间的耦合。
当然全局变量是必要的，定义的模块也需要暴露一个全局变量供给其他代码调用。

<!-- more -->

###一、全局变量命名冲突

```
var i,n,sum; //全局变量
function averageScore(teams){
	sum = 0;
	n = teams.length;
	for(i=0;i<n;i++){
		sum += score(teams[i]);
	}
	return sum / n;
}

var i,n,sum; //相同的全局变量
function score(team){
	sum = 0;
	n = team.players.length;
	for(i=0;i<n;i++){
		sum += team.players[i].score;
	}
	return sum;
}

var teams = [{players:[{score:5},{score:3},{score:6}]},{players:[{score:12},{score:1},{score:7}]}];
averageScore(teams) = 4.666666666666667;
```
```
function averageScore(teams){
	var i,n,sum; //局部变量
	sum = 0;
	n = teams.length;
	for(i=0;i<n;i++){
		sum += score(teams[i]);
	}
	return sum / n;
}

function score(team){
	var i,n,sum; //局部变量
	sum = 0;
	n = team.players.length;
	for(i=0;i<n;i++){
		sum += team.players[i].score;
	}
	return sum;
}

var teams = [{players:[{score:5},{score:3},{score:6}]},{players:[{score:12},{score:1},{score:7}]}];
averageScore(teams) = 17;
```
第一个代码片段由于混乱的全局变量导致结果出现错误。
###二、js中只有函数作用域，没有块级作用域（容易出错）

```
function test(teams){
	for(var i = 0; i < teams.length ; i++){
		window.console.log(i);
	}		
	window.console.log(i); // 3
}
test([1,2,3]);
```
for循环外依然可以取到i的值，上面的代码等价于：

```
function test(teams){
	var i；
	for(i = 0; i < teams.length ; i++){
		window.console.log(i);
	}		
	window.console.log(i); // 3
}
test([1,2,3]);
```
###三、意外创建的全局变量（需要注意）

```
function swap(array,i,j){
		temp = array[i];
		array[i] = array[j];
		array[j] = temp;
}
swap([5,6,7,8],2,3);
window.console.log(temp) // 7;
```
swap方法内的temp变量没有使用var进行定义，创建了一个意外的全局变量temp。

```
function swap(array,i,j){
		var temp = array[i];
		array[i] = array[j];
		array[j] = temp;
}
swap([5,6,7,8],2,3);
window.console.log(temp) // undefined;
```
###四、全局变量用法之一
如es5中提供的全局JSON对象。在部分浏览器并没有JSON对象，这时可以自己实现一个全局的JSON对象，来替代没有JSON对象浏览器的实现。

```
if(!window.JSON){
	window.JSON = {
		parse: function(){
			...	
		}
		stringify: function(){
			...
		}
	}
}
```

----------
end...