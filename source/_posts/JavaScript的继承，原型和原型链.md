---
title: JavaScript的继承，原型和原型链
tags:
  - JavaScript
abbrlink: 2566346867
date: 2021-10-04 12:26:58
---

## 前言

想必，学过 java 和 C++ 的小伙伴们，对于继承这个词应该不陌生，最近我也是一直在巩固JavaScript的知识，今天就来一起学习一下JavaScript里的继承吧。

<!--more-->

## 继承是什么？

首先我们要明确继承的概念：

**继承就是一个对象可以访问另外一个对象中的属性和方法**

<img src="01.png" alt="image-20210617142202137" style="zoom: 50%;" />

B继承了A，所以B也有A具有的`color`属性，这个是不是我们接触CSS的时候，会有样式继承这个东西，可以这么理解一下下~

## 继承的目的？

继承的目的我觉得殊途同归，都是实现了父类的设计，并且进行代码复用。

## 继承的方式

java、c++等：java是通过`class`类，C++是通过`:`

而我们的JavaScript，是通过**原型链** ，ES2015/ES6 中引入了 class 关键字，但那只是语法糖，JavaScript 的继承依然和基于类的继承没有一点关系。

## 原型与原型链

**JavaScript 只有一种结构：对象。**

JavaScript 的每个对象都包含了一个隐藏属性__proto__，我们就把该隐藏属性 __proto__ 称之为该**对象的原型** (prototype)，__proto__ 指向了内存中的另外一个对象，我们就把 __proto__ 指向的对象称为该**对象的原型**，那么该对象就可以直接访问其原型对象的方法或者属性。


![image-20210617143041487](02.png)

我们可以看到使用 C.name 和 C.color 时，给人的感觉属性 `name` 和 `color` 都是对象 C 本身的属性，但实际上这些属性都是位于原型对象上，我们把这个查找属性的路径称为**原型链**

每个实例对象（ **object** ）都有一个私有属性（称之为 __proto__ ）指向它的构造函数的原型对象（**prototype** ）。该原型对象也有一个自己的原型对象( __proto__ ) ，层层向上直到一个对象的原型对象为 `null`。根据定义，`null` 没有原型，并作为这个**原型链**中的最后一个环节。

> 查到到null，证明链子到头啦~


总结一下：**继承**就是一个对象可以访问另外一个对象中的属性和方法，在JavaScript 中，我们通过**原型和原型链**的方式来实现了继承特性。

## 继承的方式

### 构造函数如何创建对象

> 有一点java基础的看这块会不会特别得劲~，我当初是学过java之后接触到的这个概念，就很顺利的就理解了。

```javascript
function DogFactory(type, color) {
    this.type = type;
    this.color = color
}

var dog = new DogFactory('Dog','Black')
```

创建实例的过程

```javascript
var dog = {};
dog.__proto__ = DogFactory.prototype;
DogFactory.call(dog,'Dog','Black');
```

<img src="03.png" alt="image-20210617153157853" style="zoom: 33%;" />

观察这个图，我们可以看到执行流程分为三步：

- **首先，创建了一个空白对象 dog；**

- **然后，将 DogFactory 的 prototype 属性设置为 dog 的原型对象，这就是给 dog 对象设置原型对象的关键一步；**
- **最后，再使用 dog 来调用 DogFactory，这时候 DogFactory 函数中的 this 就指向了对象 dog，然后在 DogFactory 函数中，利用 this 对对象 dog 执行属性填充操作，最终就创建了对象 dog。**

> 每个函数对象中都有一个公开的 prototype 属性，当你将这个函数作为构造函数来创建一个新的对象时，新创建对象的原型对象就指向了该函数的 prototype 属性，所以通过该构造函数创建的任何实例都可以通过原型链找到构造函数的prototype上的属性



**实例的proto属性 ==  构造函数的proyotype**

也就是说**dog.__proto == DogFactory.prototype**

### 原型链继承

**原理：** 实现的本质是**将子类的原型指向了父类的实例**

**优点：**

* 父类新增原型方法/原型属性，子类都能访问到
* 简单容易实现

**缺点：**

* 不能实现多重继承
* 来自原型对象的所有属性被所有实例共享
* 创建子类实例时，无法向父类构造函数传参

<img src="image-20210617154553631.png" alt="image-20210617154553631" style="zoom:50%;" />

```javascript
//父类型
function Person(name, age) {
    this.name = name,
    this.age = age,
    this.play = [1, 2, 3]
    this.setName = function () { }
}
Person.prototype.setAge = function () { }
//子类型
function Student(price) {
    this.price = price
    this.setScore = function () { }
}
Student.prototype = new Person('wang',23) // 子类型的原型为父类型的一个实例对象
var s1 = new Student(15000)
var s2 = new Student(14000)
console.log(s1,s2)
```

### 借用构造函数实现继承

**原理**：在子类型构造函数中通用call()调用父类型构造函数

**特点**：

- 解决了原型链继承中子类实例共享父类引用属性的问题
- 创建子类实例时，可以向父类传递参数
- 可以实现多重继承(call多个父类对象)

**缺点**：

- 实例并不是父类的实例，只是子类的实例
- 只能继承父类的实例属性和方法，不能继承父类原型属性和方法
- 无法实现函数复用，每个子类都有父类实例函数的副本，影响性能

```javascript
function Person(name, age) {
    this.name = name,
    this.age = age,
    this.setName = function () {}
  }
  Person.prototype.setAge = function () {}
  function Student(name, age, price) {
    Person.call(this, name, age) 
    // 相当于: 
    /*
    this.Person(name, age)
    this.name = name
    this.age = age*/
    this.price = price
  }
  var s1 = new Student('Tom', 20, 15000)
```

###  原型链+借用构造函数的组合继承

**原理**：通过调用父类构造，继承父类的属性并保留传参的优点，然后通过将父类实例作为子类原型，实现函数复用。

**优点**：

- 可以继承实例属性/方法，也可以继承原型属性/方法
- 不存在引用属性共享问题
- 可传参
- 父类原型上的函数可复用

**缺点**：

- 调用了两次父类构造函数，生成了两份实例

```javascript
function Person(name, age) {
    this.name = name,
    this.age = age,
    this.setAge = function () { }
}
Person.prototype.setAge = function () {
    console.log("111")
}
function Student(name, age, price) {
    Person.call(this,name,age)
    this.price = price
    this.setScore = function () { }
}
Student.prototype = new Person()
Student.prototype.constructor = Student//组合继承也是需要修复构造函数指向的
var s1 = new Student('Tom', 20, 15000)
var s2 = new Student('Jack', 22, 14000)
console.log(s1)
console.log(s1.constructor) //Student
```

### ES6 class继承

**原理：** ES6中引入了class关键字，class可以通过extends关键字实现继承，还可以通过static关键字定义类的静态方法,这比 ES5 的通过修改原型链实现继承，要清晰和方便很多。

> 我当时第一次见的时候，还以为是java
>
> 其实我还是觉得，class写起来得劲多了，哈哈哈

**优点**：

- 语法简单易懂,操作更方便

**缺点**：

- 并不是所有的浏览器都支持class关键字


```javascript
class Person {
    //调用类的构造方法
    constructor(name, age) {
        this.name = name
        this.age = age
    }
    //定义一般的方法
    showName() {
        console.log("调用父类的方法")
        console.log(this.name, this.age);
    }
}
let p1 = new  Person('kobe', 39)
console.log(p1)
//定义一个子类
class Student extends Person {
    constructor(name, age, salary) {
        super(name, age)//通过super调用父类的构造方法
        this.salary = salary
    }
    showName() {//在子类自身定义方法
        console.log("调用子类的方法")
        console.log(this.name, this.age, this.salary);
    }
}
let s1 = new Student('wade', 38, 1000000000)
console.log(s1)
s1.showName()
```

## 最后

其实我没有在平时写的项目中，用过继承，所以不太懂具体的应用场景，希望大佬们可以指点一下。
