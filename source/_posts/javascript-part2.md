---
title: JavaScript高级篇之part2
date: 2016-05-18 18:29:33
tags: javascript
categories: javascript
---

javascript 高级与面向对象笔记整理，接 part1 篇！！

<!-- more -->

## 面向对象编程举例

### 1.面向过程的思维方式

代码重复性太高，几乎没有复用性。

### 2.使用函数进行封装

提升的代码的复用性
全局变量污染
结构混乱，后期维护不便

### 3.使用对象进行封装

使用对象进行封装，在外界之暴露一个对象名，不会造成全局污染
在对象内部使用对象的属性，进行模块的划分，让代码的结构更加的清晰，便于维护

## 创建对象的三种方式

### 字面量创建对象

```js
var obj = {
  key: value,
  key: value
}
```

只能创建一次对象，复用性太差

### 内置构造函数创建对象

```js
var obj = new Object()
```

每次创建出来的对象都是空对象，需要手动的去添加成员

### 自定义构造函数创建对象

自己创建的构造函数就是自定构造函数

## 自定义构造函数

### 构造函数的特点

- 函数的首字母大写
- 一般和 `new` 关键字配合使用
- 没有 `return` 语句，返回值默认为创建出来的对象
- 手动添加 `return` 语句的时候
- 如果 `return` 的是基本类型的数据，则不会对默认的返回有任何的影响
- 如果 `return` 的是对象类型的数据，则会替换掉默认的返回值

### 构造函数的执行步骤

- 使用 `new` 关键字创建对象
- 调用构造函数，将 `this` 赋值为 `new` 关键字创建出来的对象
- 在构造函数中，使用 `this` 为新创建的对象新增成员
- 默认返回新创建的这个对象

## 面向对象的三大特性

### 封装

将数据和方法进行封装，对外界只提供指定的接口，外部使用只要调用相应的接口，而不需要关心内部的具体实现

### 继承

一个对象没有某些属性和方法，另一个对象有，拿过来使用，就是继承

### 多态

JS 中没有多态
父类的指针指向子类的对象

## 原型

### 构造函数存在的问题

如果将方法的定义写在构造函数中，每次创建对象的时候，都会重新的创建一个新的方法，每个对象独占一个方法，但是所有对象的该方法都是一样的，会造成资源浪费

- 在外部声明函数，每次创建对象的时候，将外部的函数引用赋值给当前对象的方法，这样就能保证所有的对象都指向构造函数外部的这个函数
- 使用原型

### 原型是什么

在构造函数创建出来的时候，系统会默认的为构造函数创建并关联一个空对象，这个的对象就是原型

### 原型的作用

所用通过构造函数创建出来的对象，都能访问原型中的成员，也就是说原型中的成员被所有的对象共享

### 如何访问原型对象

- 构造函数 `.prototype`
- 对象 `.__proto__` (不推荐使用，因为有兼容性问题，调试的时候可以使用)

### 原型的使用方式

- 利用对象的动态特性为原型添加成员
- 直接替换原型对象

### 原型的使用注意事项

- 一般情况只将方法放在原型中，属性放在对象中
- 对象在获取属性的时候，会现在自身查找，如果找到了直接使用，如果没有找到，就去原型中查找，如果找到了就使用
- 对象在设置属性的时候，不会去原型中查找了，只在自身进行查找，如果找到了，就修改，如果没有找到，就新增
- 在替换原型对象的时候，需要注意：替换之前创建的对象和替换之后创建的对象的原型不一致

## 继承的实现方式

### 混入式继承（ `mix-in` ）

```js
var obj = {}
var obj1 = { name: 'adsf', age: 18 }
for (var k in obj1) {
  obj[k] = obj1[k]
}
```

### 原型继承

#### 1.使用混入的方式为原型对象添加成员、

```js
var human = { name: '', age: 18 }
function Person() {}

for (var k in human) {
  Person.prototype[k] = human[k]
}
```

#### 2.直接修改原型对象

```js
function Person() {}
Person.prototype.name = ''
Person.prototype.age = 18
```

#### 3.替换原型对象

```js
var human = { name: '', age: 18 }
function Person() {}
Person.prototype = human
```

### 经典继承

```js
var obj = Object.create(obj1)
//创建出来一个新的对象obj继承自obj1
//原理就是把obj1设置为obj的原型
```

#### 经典继承的兼容性问题

```js
function myCreate(obj) {
  //判断浏览器有没有Object.create方法
  if (Object.create) {
    //如果有，直接调用
    return Object.create(obj)
  } else {
    function F() {}
    F.prototype = obj
    return new F()
  }
}
```

##### 为什么不能修改原生对象?

因为原生对象是公用的，在多人开发的时候，可能会出现冲突，你修改了，他也修改了，谁的生效呢？
