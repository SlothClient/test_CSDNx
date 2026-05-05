### Var与Let：JavaScript变量声明的演变

#### Var：JavaScript的传统变量声明
`var` 关键字是 JavaScript 最初的变量声明方式，允许声明变量并为其赋值。`var` 声明的变量具有函数作用域，如果在函数外部声明，则成为全局变量。

#### Let：ECMAScript 2015引入的块级作用域变量
`let` 是 ES6 引入的，用于声明块级作用域变量。这意味着由 `let` 声明的变量仅在其声明的代码块内可见。

### Var与Let的关键差异

#### 1. 作用域
- **Var**：局限于函数或全局作用域。
- **Let**：局限于块级作用域，如 `if` 语句、`for` 循环等。

#### 2. 变量提升
- **Var**：存在变量提升，允许在声明前使用变量，初始值为 `undefined`。
- **Let**：不存在变量提升，提前使用会导致错误。

#### 3. 重复声明
- **Var**：在同一作用域内可以重复声明变量。
- **Let**：在同一作用域内重复声明会导致错误。

### 代码示例

#### Var的使用
```javascript
var x = 10;

if (true) {
  var x = 20; // 覆盖之前的变量值
  console.log(x); // 输出：20
}

console.log(x); // 输出：20
```

#### Let的使用
```javascript
let y = 10;

if (true) {
  let y = 20; // 块级作用域，新变量
  console.log(y); // 输出：20
}

console.log(y); // 输出：10
```

#### 变量提升
```javascript
console.log(x); // 输出：undefined，变量提升
var x = 10;

console.log(y); // 抛出错误：y is not defined，无变量提升
let y = 10;
```

#### 重复声明
```javascript
var a = 10;
var a = 20; // 允许重复声明，值更新
console.log(a); // 输出：20

let b = 10;
let b = 20; // 禁止重复声明，抛出错误
```

### 作用域的扩展讨论

#### 1. 函数作用域
函数作用域限制变量只能在函数内部访问。

```javascript
function myFunction() {
  var z = 30;
  console.log(z); // 输出：30
}
myFunction();
console.log(z); // 抛出错误：z is not defined
```

#### 2. 全局作用域
全局作用域允许变量在整个代码范围内访问。

```javascript
var globalVar = 100;
console.log(globalVar); // 输出：100
```

#### 3. 块级作用域
块级作用域限制变量只能在代码块内访问。

```javascript
if (true) {
  let blockVar = 40;
  console.log(blockVar); // 输出：40
}
console.log(blockVar); // 抛出错误：blockVar is not defined
```

#### 4. 词法作用域
词法作用域由函数创建时的上下文决定，与变量赋值无关。

```javascript
function scopeTest() {
  var scope = "function scope";
  console.log(scope); // 输出：function scope
}
scopeTest();

var scope = "global scope";
scopeTest();
console.log(scope); // 输出：global scope
```

理解 `var` 和 `let` 的差异对于控制 JavaScript 代码中的变量可见性和生命周期至关重要。使用 `let` 和 `const` 可以编写更清晰、更易于维护的代码。

---

## Cover 图

![cover_1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/58/58655a1ccb87043e86a0053e9e1240b55196bdbdd98459725bf8b75c2b09c593.jpeg)
