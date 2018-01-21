---
title: 事件绑定和事件委托整理
date: 2017-09-20 07:40:16
tags: 前端
---
### 一、事件绑定

<!-- more -->

#### 测试代码：
```
<div class="hierarchy-1">
    <p class="hierarchy-2">
        <a class="hierarchy-3">
            <span class="hierarchy-4">4</span>
            3
        </a>
        2
    </p>
    1
</div>

<script type="text/javascript">
    document.querySelector('.hierarchy-1').addEventListener('click',function (e) {
        console.log(e.currentTarget);
    },false);
    document.querySelector('.hierarchy-2').addEventListener('click',function (e) {
        console.log(e.currentTarget);
    },false);
    document.querySelector('.hierarchy-3').addEventListener('click',function (e) {
        console.log(e.currentTarget);
    },false);
    document.querySelector('.hierarchy-4').addEventListener('click',function (e) {
        console.log(e.currentTarget);
        e.stopPropagation();
        e.preventDefault()
    },false);
</script>
```
#### 示意图：
![这里写图片描述](http://img.blog.csdn.net/20170920072931033?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDY0MTAxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### 1、target和currentTarget
为每一层绑定一个点击事件，观察target和currentTarget的异同。

#### **点击hierarchy-1**
响应元素 | target | currentTarget
:----: | :----: | :----:
1 |  hierarchy-1 | hierarchy-1


----------


#### **点击hierarchy-2**
响应元素 | target | currentTarget
:----: | :----: | :----:
1 |  hierarchy-2 | hierarchy-1 
2 |  hierarchy-2 | hierarchy-2


----------


#### **点击hierarchy-3**
响应元素 | target | currentTarget
:----: | :----: | :----:
1 |  hierarchy-3 | hierarchy-1 
2 |  hierarchy-3 | hierarchy-2
3 |  hierarchy-3 | hierarchy-3 


----------


#### **点击hierarchy-4**
响应元素 | target | currentTarget
:----: | :----: | :----:
1 |  hierarchy-4 | hierarchy-1 
2 |  hierarchy-4 | hierarchy-2
3 |  hierarchy-4 | hierarchy-3 
4 |	 hierarchy-4 | hierarchy-4


----------


由上述测试可知，target为点击所响应的最底层的元素，而currentTarget则为点击所响应的当前元素，只有当被点击元素为最底层元素时，target才等于currentTarget。

#### 2、stopPropagation()
我们在开发的时候可能有这样一种需求，比如有一个用户消息列表，点击列表可以查看消息详情，同时点击用户头像可以前往用户个人主页，我们分别对列表item和item中的头像增加点击事件，这是点击用户头像时还是会跳转到消息详情，因为列表的点击事件是后响应的，所以此时我们应该响应了头像点击事件后，禁止事件继续传递下去。js提供了一个e.stopPropagation()方法，可以阻止事件冒泡。

```
<script type="text/javascript">
    document.querySelector('.hierarchy-1').addEventListener('click',function (e) {
        console.log(e.currentTarget);
    },false);
    document.querySelector('.hierarchy-2').addEventListener('click',function (e) {
        console.log(e.currentTarget);
    },false);
    document.querySelector('.hierarchy-3').addEventListener('click',function (e) {
        console.log(e.currentTarget);
    },false);
    document.querySelector('.hierarchy-4').addEventListener('click',function (e) {
        console.log(e.currentTarget);
        e.stopPropagation();
    },false);
</script>
```
还是上述例子，在hierarchy-4增加了一个e.stopPropagation()。点击hierarchy-4，hierarchy-1、hierarchy-2、hierarchy-3都不会响应点击事件。有时我们还需要阻止元素的默认事件，比如a标签的href，我们可以使用e.preventDefault()方法。

### 二、事件委托

来看一个简单的示例:
```
<style>
    ul {
        background-color: red;
    }

    li {
        height: 60px;
        line-height: 60px;
        background-color: green;
        border-bottom: 1px solid #000;
    }
</style>

<ul>
    <li>我是一号</li>
    <li>我是二号</li>
    <li>我是三号</li>
    <li>我是四号</li>
    <li>我是五号</li>
    <li>我是六号</li>
</ul>
```
#### 示意图：
![这里写图片描述](http://img.blog.csdn.net/20170922214955252?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDY0MTAxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

我们希望点击li标签，alert出对应的文字，这时我们可能会采用获取所有的li标签，然后为每个li标签绑定一个点击事件。代码如下:
```
		var list = document.querySelectorAll('li');
	        for (var i = 0; i < list.length; i++) {
                list[index].onclick = function (e) {
                    alert(list[index].innerHtml);
                }
            }
        }
```
这种实现方式主要有2个缺点:
1、li标签过多时，绑定了太多的点击事件，会对浏览器增加很多开销。
2、当我们想插入一个li标签时，还需要为新的li标签增加点击事件。

为了解决这两个问题，我们可以采用事件代理的方式解决，将所有的li的点击通过父级标签ul来处理，这样既可以大大减少事件绑定的个数，同时新增li标签也无需单独绑定事件。代码如下：

```
	document.querySelector('ul').onclick = function (e) {
        var event = e || window.event;
        var target = event.target || event.srcElement;
        if (target.nodeName.toLowerCase() === 'li') {
            alert(target.innerHTML);
        }
    }
```

over...
