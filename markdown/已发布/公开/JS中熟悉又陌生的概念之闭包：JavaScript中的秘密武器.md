### 闭包：JavaScript中的秘密武器

闭包是JavaScript中一个强大的特性，它使得一个函数能够访问另一个函数的变量，即使外部函数已经执行完毕。本质上，闭包是函数记住并访问其外部作用域的能力。

>大家熟知的前端打包工具webpack中就大量运用了闭包这个功能特效，譬如打包后文件中的加载器（loader）

### 闭包的本质

闭包就像一个函数携带着它被创建时的环境，即使它被执行在完全不同的作用域中。这种特性使得闭包在JavaScript中非常有用，尤其是在创建私有变量和封装功能时。

### 闭包的应用场景

闭包的主要优势在于能够访问外部函数的变量，这使得它成为创建私有变量的理想选择，这些变量只能在闭包内部被修改和访问。

### 代码示例

#### 案例 1：基础闭包

```javascript
function createGreeting() {
  const name = "Xiaobai";
  return function() {
    console.log("Hello, " + name + "!");
  };
}

const myGreeting = createGreeting(); // 创建闭包
myGreeting(); // 输出：Hello, Xiaobai!
```

在这个例子中，`createGreeting` 函数创建并返回了一个匿名函数，这个匿名函数是闭包，因为它访问了 `createGreeting` 函数的局部变量 `name`。即使 `createGreeting` 函数已经执行完毕，返回的函数仍然可以访问 `name` 变量。

#### 案例 2：使用闭包封装私有变量

```javascript
function createCounter() {
  let count = 0; // 私有变量
  return {
    increment: function() {
      count++;
      console.log(count);
    },
    decrement: function() {
      count--;
      console.log(count);
    }
  };
}

const counter = createCounter();
counter.increment(); // 输出：1
counter.increment(); // 输出：2
counter.decrement(); // 输出：1
```

在这个例子中，`createCounter` 函数返回了一个包含两个函数 `increment` 和 `decrement` 的对象。这两个函数都是闭包，因为它们都可以访问 `createCounter` 函数中的私有变量 `count`。这样，`count` 变量就被封装在闭包内部，不会被外部直接访问。

#### 案例 3：闭包与循环

```javascript
function createCounter() {
  let count = 0;
  return function() {
    count++;
    console.log(count);
  };
}

for (let i = 0; i < 3; i++) {
  (function(index) {
    createCounter()(); // 每次循环都创建了一个新的闭包
  })(i);
}

// 输出：
// 1
// 2
// 3
```

这个例子中，我们使用立即执行函数表达式（IIFE）为每次迭代创建一个新的作用域，这样每次循环都会创建一个新的 `count` 变量，因此每次调用 `createCounter` 都会输出不同的数字。

闭包是JavaScript中一个非常强大的工具，它允许我们创建私有变量和封装复杂的逻辑。理解闭包可以帮助你编写更加模块化和可重用的代码。

---

## Cover 图

![cover_1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/08/08fad485e985ae3ba61adca07c4e4eefadcf017af6c28a2d2253796a9f570a69.jpeg)
