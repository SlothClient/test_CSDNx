@[TOC]
![在这里插入图片描述](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/0e/0ece023a5b69d21af58742cfb9437acceeff186dba85994f950855794d148070.png)

## JS中普通函数和箭头函数的区别

在现代JavaScript开发中，函数是最基础也是最重要的构造。随着ES6的普及，箭头函数（Arrow Function）被广泛使用，但普通函数（Function Declaration / Function Expression）仍然不可或缺。本文将通过对比示例，全面解析两者的区别，并帮你理解在不同场景下如何选择使用。

### 1. 语法差异

普通函数的声明方式有两种：

```js
// 函数声明
function add(a, b) {
  return a + b;
}

// 函数表达式
const multiply = function(a, b) {
  return a * b;
};
```

箭头函数的写法更加简洁：

```js
const add = (a, b) => a + b;
const multiply = (a, b) => {
  return a * b;
};
```

**总结**：

- 箭头函数可以省略 `function` 关键字。
- 单行表达式可以省略大括号和 `return`。
- 参数只有一个时可以省略括号，如：`x => x * 2`。

### 2. `this` 指向

这是普通函数与箭头函数最本质的区别。

#### 普通函数的 `this`

普通函数的 `this` 指向调用它的对象，调用方式不同，`this` 也不同：

```js
const obj = {
  name: "Alice",
  greet: function() {
    console.log(this.name);
  }
};

obj.greet(); // Alice

const greetFunc = obj.greet;
greetFunc(); // undefined 或 window / globalThis
```

#### 箭头函数的 `this`

箭头函数没有自己的 `this`，它会捕获**定义时的外层 `this`**：

```js
const obj = {
  name: "Alice",
  greet: () => {
    console.log(this.name);
  }
};

obj.greet(); // undefined
```

> 注意：箭头函数通常不适合作为对象方法，因为它的 `this` 并不指向对象本身。

### 3. `arguments` 对象

普通函数可以访问内置的 `arguments` 对象：

```js
function sum() {
  let total = 0;
  for (let i = 0; i < arguments.length; i++) {
    total += arguments[i];
  }
  return total;
}

sum(1, 2, 3); // 6
```

箭头函数没有自己的 `arguments`，需要使用 **剩余参数**：

```js
const sum = (...args) => args.reduce((a, b) => a + b, 0);
sum(1, 2, 3); // 6
```

### 4. 可作为构造函数

普通函数可以用 `new` 创建实例：

```js
function Person(name) {
  this.name = name;
}
const p = new Person("Alice");
console.log(p.name); // Alice
```

箭头函数**不能**作为构造函数：

```js
const Person = (name) => { this.name = name; };
new Person("Alice"); // TypeError: Person is not a constructor
```

### 5. `prototype` 属性

普通函数有 `prototype`，箭头函数没有：

```js
function Foo() {}
console.log(Foo.prototype); // Foo {}

const Bar = () => {};
console.log(Bar.prototype); // undefined
```

这意味着箭头函数不能用来定义类或传统的原型方法。

### 6. 什么时候用箭头函数

- 回调函数：如 `map`、`filter`、`forEach` 等
- 不需要 `this` 或 `arguments` 的函数
- 保持函数体简洁，减少冗余

**示例**：

```js
const nums = [1, 2, 3];
const squares = nums.map(n => n * n);
console.log(squares); // [1, 4, 9]
```

### 7. 总结对比表

| 特性        | 普通函数                             | 箭头函数                       |
| ----------- | ------------------------------------ | ------------------------------ |
| 语法        | `function` 声明或表达式              | 简洁 `()=>{}`                  |
| `this`      | 调用时决定                           | 继承外层 `this`                |
| `arguments` | 有                                   | 无，需要 `...args`             |
| 构造函数    | 可使用 `new`                         | 不可使用 `new`                 |
| `prototype` | 有                                   | 无                             |
| 适用场景    | 对象方法、构造函数、需要 `arguments` | 简短回调函数，保持 `this` 绑定 |

### 8. 思考题

1. 为什么在事件监听中，箭头函数可以避免 `this` 指向 `window`？

   答：普通函数是动态绑定，使用的是动态this；箭头函数是词法绑定，使用的是词法this，继承定义时外层作用域的this，可以达到保持上下文的效果。

2. 如何在箭头函数中访问外层函数的 `arguments`？

   答：构造闭包使用外层的`arguments`或使用剩余参数`...args`。

3. 什么时候仍然必须使用普通函数而不能用箭头函数？

   答：需要使用构造函数、动态this以及arguments参数等情况下。

---

## Cover 图

![cover_1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/04/04e8e8a6084f0840a9c7d0fec7ff4b688cf170c2fce205f3d228d361c3e19846.png)
