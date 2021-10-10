---
title: JavaScript数组去重问题
author: Ned
tag:
  - JavaScript
categories:
 - JavaScript进阶
abbrlink: 1826147754
---

## 前言

这应该是一个很常见的问题了，既然是常见的，那我们就更应该来学习一下！

## 开始研究

### 原始

数组去重，最开始我的思路是这样：定义一个新数组，完后两层for循环，如果数据第一次出现，就push到新数组里，如果重复就break掉，利用j的值与res长度相等这一点来判断数据唯一，最后返回新数组就行了。

<!--more-->

```js
var arr = [1,1,2,3,4,5,6,7,4,3,'1',8,'3','1','3','66']

function unique(arr){
	var res = []
	for(var i = 0; i < arr.length; i++){
		for(var j = 0; j < res.length; j ++){
			if(arr[i] === res[j]){
				break
			}
		}
		// 如果数据第一次出现，那么执行完上面for语句后，j的值应该等于res的长度才对
		if(j === res.length){
			res.push(arr[i])
		}
	}
	return res;
}

console.log(unique(arr));
```

### 利用indexOf优化原始方法

我们先来简单了解一下indexOf：

indexOf(item,start) 方法可返回数组中某个指定的元素位置。

该方法将从头到尾地检索数组，看它是否含有对应的元素。开始检索的位置在数组 start 处或数组的开头（没有指定 start 参数时）。如果找到一个 item，则返回 item 的第一次出现的位置。开始位置的索引为 0。

**如果在数组中没找到指定元素则返回 -1。**

看到这大家都明白我们利用的是哪一点了吧，没错，就是加粗的那一句话：**如果在数组中没找到指定元素则返回 -1。**

```js
var arr = [1,1,2,3,4,5,6,7,4,3,'1',8,'3','1','3','66']

function unique(arr){
	var res = []
	for(var i = 0; i < arr.length; i++){
		if(res.indexOf(arr[i]) === -1){
			res.push(arr[i])
		}
	}
	return res;
}

console.log(unique(arr));
```

### 再次优化，filter方法！

filter，顾名思义，过滤的意思，该方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素。

思路：用filter代替一层循环与indexOf配合，达到过滤效果，直接返回去重过后的数组。

```js
var arr = [1,1,2,3,4,5,6,7,4,3,'1',8,'3','1','3','66']

function unique(arr){
	var res = arr.filter(function(item,index,arr){
		return arr.indexOf(item) === index
	})
	return res;
}
console.log(unique(arr));
```

### 换种思路？变成有序数组？

不知道刷过几天力扣的小伙伴们有没有这种感觉，看见题目中出现数组，眼睛就立刻往前瞄了瞄，看看是有序数组还是无序数组~

回到这个问题上，我们将要去重的数组变成有序，重复的数据肯定都挨着了，用一个变量存放上一个元素值，再循环判断当前值与上一个元素值是否相同，如果不相同，就将它添加到res中。

```js
var arr = [1,1,2,3,4,5,6,7,4,3,'1',8,'3','1','3','66']

function unique(arr){
	var res = []
	var pre
	arr = arr.sort()
	for(var i = 0; i < arr.length; i++){
		if(!i || pre !== arr[i]){
			res.push(arr[i])
		}
		pre = arr[i]
	}
	return res;
}

console.log(unique(arr));
```

### 再再次优化，filter！

刚刚悟了~，filter好像也可以把排序这里重写一下，变得更为简洁，我们直接看代码：

```js
var arr = [1,1,2,3,4,5,6,7,4,3,'1',8,'3','1','3','66']

function unique(arr){
	var res = arr.sort().filter(function(item,index,arr){
		return !index || item !== arr[index - 1]
	})
	return res;
}

console.log(unique(arr));
```

### ES6，Set来袭！

ES6给我们带来了很多好处，其中，map、set尤为优秀。

Map 对象保存键值对。任何值(对象或者原始值) 都可以作为一个键或一个值。

Set 对象允许你存储任何类型的唯一值，无论是原始值或者是对象引用。

所以我们可以利用Set的这一特性，来进行去重处理。

```js
var arr = [1,1,2,3,4,5,6,7,4,3,'1',8,'3','1','3','66']

function unique(arr){
	return Array.from(new Set(arr))
}

console.log(unique(arr));
```

注：Set是对象，所以要转成数组进行返回。

#### 懂解构赋值的你，可以再简化一点

```js
var arr = [1,1,2,3,4,5,6,7,4,3,'1',8,'3','1','3','66']

function unique(arr){
	return [...new Set(arr)]
}

console.log(unique(arr));
```

> 想了解一下解构赋值的也可以先康康这个：[解构运算符的理解与运用 ](https://blog.wangez.site/posts/1586874348.html/)
>
> 之前学习，记录的笔记🎨

#### 继续优秀下去（箭头函数）

```js
var arr = [1,1,2,3,4,5,6,7,4,3,'1',8,'3','1','3','66']
var unique = (arr) => [...new Set(arr)]
console.log(unique(arr));
```

## 最后

从最开始的好几行代码，到最后利用箭头函数，可以一行就写完，足以见得，JavaScript是在逐渐变得更好。

那我们，作为开发者，也要努力学习，才能更好的去使用这门语言呀🎈

> 学无止境，不是说说而已。
>
> 点个赞，我们一起学习进步吧~