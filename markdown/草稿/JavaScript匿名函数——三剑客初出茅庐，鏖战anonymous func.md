JavaScript中的匿名函数是一种没有名称的函数，它们通常用于那些只需要使用一次的函数。匿名函数可以被赋值给变量，也可以作为参数传递给其他函数，或者直接执行。下面我将详细介绍匿名函数的几种使用场景，并提供相应的代码案例。

1. 匿名函数变量赋值

匿名函数可以被赋值给一个变量，这样你就可以在代码的其他部分引用这个函数。

var myFunc = function() {

  console.log('这是一个匿名函数');

};

myFunc(); // 输出：这是一个匿名函数

2. 匿名函数自执行

自执行匿名函数（Immediately Invoked Function Expression，IIFE）是一种在定义后立即执行的函数。它们通常用于创建局部作用域，避免污染全局命名空间。

(function() {

  console.log('这是一个自执行的匿名函数');

})();

// 输出：这是一个自执行的匿名函数

3. 匿名函数作为参数

匿名函数可以作为参数传递给其他函数，这使得它们在处理回调和事件处理器时非常有用。

function executeFunction(fn) {

  fn();

}

executeFunction(function() {

  console.log('这是一个作为参数传递的匿名函数');

});

// 输出：这是一个作为参数传递的匿名函数

4. 匿名函数作为高阶函数使用

高阶函数是指接受函数作为参数或返回函数的函数。匿名函数在这里可以作为这些高阶函数的参数或返回值。

function createGreeting(name) {

  return function() {

    console.log('Hello, ' + name + '!');

  };

}

var greetKimi = createGreeting('Kimi');

greetKimi(); // 输出：Hello, Kimi!

// 作为参数传递

function repeatAction(fn, times) {

  for (var i = 0; i < times; i++) {

    fn();

  }

}

repeatAction(function() {

  console.log('重复执行的匿名函数');

}, 3);

// 输出：

// 重复执行的匿名函数

// 重复执行的匿名函数

// 重复执行的匿名函数

5. 使用箭头函数的匿名函数

ES6 引入了箭头函数，它提供了一种更简洁的写法来定义匿名函数。

let add = (a, b) => a + b;

console.log(add(2, 3)); // 输出：5

// 自执行箭头函数

(() => {

  console.log('这是一个自执行的箭头函数');

})();

// 作为参数传递的箭头函数

executeFunction(() => {

  console.log('这是一个作为参数传递的箭头函数');

});

// 作为高阶函数使用的箭头函数

repeatAction(() => {

  console.log('这是一个重复执行的箭头函数');

}, 3);

// 输出：

// 这是一个重复执行的箭头函数

// 这是一个重复执行的箭头函数

// 这是一个重复执行的箭头函数

以上就是JavaScript中匿名函数的几种常见用法，它们在不同的场景下提供了极大的灵活性和便利性。

---

## Cover 图

![cover_1](JavaScript匿名函数——三剑客初出茅庐，鏖战anonymous func.assets/b9ec5484b4405785.png)
