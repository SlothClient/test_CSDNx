# JavaScript中的原型与原型链（上篇）

## 初识原型与原型链

在JavaScript中，原型（prototype）和原型链（prototype chain）是理解继承和对象属性查找的核心概念。这篇文章将从基础开始，逐步深入，帮助你掌握JavaScript中的原型与原型链。

### 什么是原型？

在JavaScript中，每个对象都有一个特殊的隐藏属性`[[Prototype]]`，它指向另一个对象，这个对象就是原对象的原型。原型对象上定义的属性和方法可以被原对象继承。

### 什么是原型链？

原型链是指当试图访问一个对象的属性时，如果该对象本身没有这个属性，JavaScript引擎会沿着该对象的`[[Prototype]]`向上查找，直到找到属性或到达原型链的末端（`null`）。

## 原型的创建与使用

### 1. 使用构造函数创建对象

JavaScript中的对象可以通过构造函数来创建。构造函数是一种特殊的函数，用于创建和初始化对象。

```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.greet = function() {
  console.log(`Hello, my name is ${this.name}!`);
};

const person = new Person('Xiaobai');
person.greet(); // 输出：Hello, my name is Xiaobai!
```

在这个例子中，`Person`是一个构造函数，它创建了一个具有`name`属性的对象。`Person.prototype.greet`是一个在原型上定义的方法，所有`Person`的实例都可以访问这个方法。

### 2. 使用Object.create()创建对象

`Object.create()`方法可以用来创建一个新对象，它的原型是传入的第一个参数。

```javascript
const proto = {
  greet() {
    console.log(`Hello from prototype!`);
  }
};

const obj = Object.create(proto);
obj.greet(); // 输出：Hello from prototype!
```

在这个例子中，我们创建了一个原型对象`proto`，并定义了一个`greet`方法。然后使用`Object.create(proto)`创建了一个新的对象`obj`，它的原型是`proto`，因此`obj`可以访问`greet`方法。

### 3. 使用字面量创建对象

对象字面量是创建对象的另一种方式，这种方式创建的对象自动将`Object.prototype`作为其原型。

```javascript
const obj = {
  name: 'Xiaobai',
  greet() {
    console.log(`Hello, my name is ${this.name}!`);
  }
};

obj.greet(); // 输出：Hello, my name is Xiaobai!
```

在这个例子中，`obj`是一个对象字面量，它有一个`name`属性和一个`greet`方法。由于`obj`没有显式指定原型，它的原型默认是`Object.prototype`。

## 总结

本篇文章介绍了JavaScript中的原型和原型链的基本概念，以及如何使用构造函数、`Object.create()`和对象字面量来创建对象。在下一篇文章中，我们将深入探讨原型链的工作原理，以及如何使用原型链来实现继承和属性查找。敬请期待！

---

引用来源：
- [继承与原型链 - JavaScript | MDN]

---

## Cover 图

![cover_1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/92/92616f67319c1d5285748ae437742e38c85aa6029acea84969d1ad7d22e93cee.jpeg)
