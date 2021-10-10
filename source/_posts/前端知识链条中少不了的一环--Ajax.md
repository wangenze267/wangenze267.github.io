---
title: 前端知识链条中少不了的一环--Ajax
author: Ned
tags:
  - JavaScript
categories:
 - JavaScript基础
abbrlink: 1172116075
---

> 不知不觉中又好久没写博客了，这几日因为在看计网，没有记录，于是又觉得自己没有学到什么东西，正好今天在牛客刷面经的时候看见了手撕Ajax，题材有了，于是文章它来了！
## 前言

当我们步入前端大门，走过HTML，看过CSS，翻过JavaScript，接下来你该遇到的，就是它了--Ajax。
​这个也是前端与后端交互所必需的东西，非常之重要。所以才有了标题的说法，它是前端知识链条中少不了的一环。

## 什么是Ajax？
Ajax的核心是JavaScript对象XmlHttpRequest，XmlHttp使我们可以使用JavaScript向服务器提出请求并处理响应，而不阻塞用户。
​通过XMLHttpRequest对象，前端开发人员就可以在页面加载以后进行页面的局部更新等操作。

<!-- more -->

## Ajax作用是什么？
- 通过异步模式，提升了用户体验
- 不在使用form表单提交（会出现跳转情况），提升了用户体验
- Ajax可以实现局部刷新，不用在更新整个页面，传统的网页（不适用Ajax），想要更新内容必须重载整个页面。

这就使得web应用程序能够更加敏捷的回应用户操作，避免了多次向服务端发送那些重复的数据。

## 详解Ajax创建请求步骤

### GET请求
步骤如下：
	1.创建一个对象
	2.设置请求参数
	3.发送请求
	4.监听请求成功后的状态变化

```javascript
	var request = new XMLHttpRequest()
	request.open("GET", "url")
	request.send()
	request.onreadystatechange = function() {
  		if (this.readyState == 4 && this.status == 200) {
    		console.log(request.responseText)
  		}
	}
```

- open()

  > open()方法创建了http请求，它有三个参数，第一个参数指定了提交的方式是POST还是GET，第二个参数是指定要提交的地址是哪，第三个参数是指定异步还是同步（true or false）【目前已经废弃第三个参数】

- send()

  > send()方法表示着发送请求，不需要任何参数

- 监听请求成功后的状态变化

  > onreadystatechange：监听请求状态改变，readyState改变时会调用此方法，一般用于指定回调函数
  >
  > readyState：共有五个状态
  >
  > - 0：未初始化
  > - 1：open方法成功调用后
  > - 2：服务器已经答应客户端的请求
  > - 3：交互中，http头信息已经接收，响应数据未接收
  > - 4：完成
  >
  > responseText：服务器端返回的文本内容，默认是字符串
  >
  > status：服务器端返回的状态码
  >
  > statusText：服务器端返回的状态码文本说明信息

### POST请求

步骤如下：
	1.创建一个对象
	2.设置请求参数
	3.设置请求头
	4.发送请求
	5.监听请求成功后的状态变化
可以看出，post请求比get请求多出了一个设置请求头的步骤，也请大家记住，这个步骤千万不要忘记！
```
	let XHR = new XMLHttpRequest(); 
	XHR.open("POST", "url"); 
	XHR.setRequestHeader("Content-type","application/x-www-form-urlencoded"); 
	XHR.send(data);
	XHR.onreadystatechange = function () {
		if (XHR.readyState == 4 && XHR.status == 200) {
			console.log(XHR.responseText); 
			XHR = null; // 此处是为了释放对象，不写也行，js本身也会进行回收
		}
	}
```
- send()：解释一下这里send里为什么有个data，这是我们通过Ajax发送post请求给服务器发送了一组data数据。

- setRequestHeader()：设置请求头

- readyState值说明 ：
	0.初始化,XHR对象已经创建,还未执行open 
	1.载入,已经调用open方法,但是还没发送请求 
	2.载入完成,请求已经发送完成
	3.交互,可以接收到部分数据 
	4.数据全部返回

- status值说明:
	1.200：成功
	2.404：没有发现文件、查询或url
	3.500：服务器内部发生错误
## Ajax中 GET与POST请求的区别
1.刚刚我们已经说过，get请求我们不用设置请求头，而post请求需要。
2.使用get请求的时候，参数在url中显示，而使用post方式则不会显示出来。
3.使用get请求发送的数据量小，post请求发送的数据量大。
4.get请求能够被缓存，post不进行缓存。get请求能够被保存在浏览器的历史记录中（密码等私密数据采用get方式提交，别人查看历史记录就能直接看到这些数据，造成数据泄漏）。
5.get产生一个TCP数据包，post产生两个tcp数据包。
	a. 对于get，浏览器会把header跟data一起发出去，服务器响应200，并返回数据。
	b. 对于post，浏览器先发送header，服务器响应continue，浏览器再发送data，服务器响应200，并返回数据。

## 最后

于端午前在牛客购买的课程，如今已经看完了百分之80，随着课程将基础滚了一遍，觉得差不大多了emm，下次回滚就应该是在面试前期了，跟随着面经来复习了！最近跟小伙伴们的风气真是越来越快乐了，卷的要命！

> 每日一问：今日你卷了没？

都是一群为了自己未来拼命的人，希望未来的我们会因为当时的环境而感激在大一大二就开始耗费时光，开始奋斗的自己！
