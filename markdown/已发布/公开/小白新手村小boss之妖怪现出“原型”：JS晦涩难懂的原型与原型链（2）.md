# JavaScript中的原型与原型链（下篇）

## 深入原型链的工作原理

在上一篇文章中，我们了解了原型和原型链的基本概念以及如何创建对象。这篇文章我们将深入探讨原型链的工作原理，以及如何利用原型链实现继承和属性查找。

### 原型链的属性查找

当我们尝试访问一个对象的属性时，JavaScript引擎会首先在该对象上查找该属性。如果找不到，它会沿着原型链向上查找，直到找到该属性或到达原型链的末端（`null`）。

### 代码案例：原型链的属性查找

```javascript
const proto = {
  greet() {
    console.log('Hello from prototype!');
  }
};

const obj = Object.create(proto);
obj.greet(); // 输出：Hello from prototype!

console.log(obj.hasOwnProperty('greet')); // 输出：false
console.log(obj.greet === proto.greet); // 输出：true
```

在这个例子中，`obj`对象没有自己的`greet`方法，但是它可以通过原型链访问`proto`对象上的`greet`方法。`hasOwnProperty`方法用于检查对象是否具有指定的自有属性，不包括原型链上的属性。

### 原型链与继承

原型链是JavaScript实现继承的主要方式。通过设置一个对象的原型为另一个对象，可以实现属性和方法的继承。

### 代码案例：原型链实现继承

```javascript
function Animal(species) {
  this.species = species;
}

Animal.prototype.getSpecies = function() {
  return this.species;
};

function Dog(name) {
  Animal.call(this, 'dog'); // 继承Animal的species属性
  this.name = name;
}

// 设置Dog的原型为Animal的实例
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog; // 修复constructor属性

Dog.prototype.bark = function() {
  console.log(`${this.name} barks!`);
};

const dog = new Dog('Buddy');
console.log(dog.getSpecies()); // 输出：dog
dog.bark(); // 输出：Buddy barks!
```

在这个例子中，我们通过设置`Dog.prototype`为`Animal.prototype`的实例来实现继承。这样，`Dog`的实例就可以访问`Animal.prototype`上的方法。同时，我们修复了`constructor`属性，并添加了`bark`方法。

### 原型链的优缺点

#### 优点

1. **节省内存**：多个对象可以共享同一个原型，而不是每个对象都复制一份方法。
2. **动态性**：可以在运行时修改对象的原型，改变对象的行为。

#### 缺点

1. **性能问题**：原型链上较深层的属性查找可能会影响性能。
2. **属性遮蔽**：对象的自有属性会遮蔽原型链上的同名属性。

### 代码案例：属性遮蔽

```javascript
const proto = {
  greet() {
    console.log('Hello from prototype!');
  }
};

const obj = Object.create(proto);
obj.greet = function() {
  console.log('Hello from own property!');
};

obj.greet(); // 输出：Hello from own property!
```

在这个例子中，`obj`对象有自己的`greet`方法，它遮蔽了原型链上的`greet`方法。

### 总结

通过这篇文章，我们深入探讨了原型链的工作原理和如何利用原型链实现继承。理解原型链对于编写高效、可维护的JavaScript代码至关重要。在实际开发中，合理利用原型链可以提高代码的复用性和灵活性。

---

引用来源：
- [继承与原型链 - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)

---

## Cover 图

![cover_1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/7c/7c83fb0800dc5ad2cc6d7197c487fc06ea36c8e4b5e72030100d9365dbf732b4.jpeg)
