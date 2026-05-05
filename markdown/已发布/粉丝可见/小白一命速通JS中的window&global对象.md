> 笔者注意到JS中的`window`对象与`global`对象经常被混淆，尽管它们在相当一部分使用情况下可以等同，但是本质上仍然存在很多不同，下面是对于两者的详细拆解

---

### **1. `window` 对象**
- **定义**：`window` 对象表示 **浏览器环境**中的全局上下文。
- **作用域**：它是浏览器中运行的任何 JavaScript 代码的顶级对象。
- **关键特性**：
  - 包含所有通过 `var` 声明的 **全局变量和函数**（在非模块脚本中）。
  - 表示浏览器的 **窗口** 或 **框架**，代码运行在其中。
  - 包含浏览器特定的属性和方法，如：
    - `window.location`（当前 URL）
    - `window.document`（页面的 DOM 树）
    - `window.alert()`（弹出框）
  - 隐式引用/自动挂载：通过 `var` 声明或未使用 `let`/`const`/`var` 创建的全局变量成为 `window` 的属性。 ![隐式引用](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/21/21eee1f89656d2b02d8c1456aeb266934730bd72d9aa5c7bd4580cfc4d960397.png)


#### **示例**：
```javascript
console.log(window.location.href); // 打印当前页面的 URL
window.alert("Hello!"); // 显示一个警告对话框
```

---

### **2. `global`对象**
- **定义**：`global`对象是所有 JavaScript 环境中（**浏览器、Node.js 或其他**）可用的顶级对象。
- **作用域**：它作为容器，包含所有全局可访问的实体。
- **关键特性**：
  - 它根据 **执行环境** 的不同而有所不同：
    - 在浏览器中，`global`对象是 `window`。
    - 在 Node.js 中，`global`对象是 `global`。
    - 在 Web Workers 中，`global`对象是 `self`。
  - 包含全局内置对象，如 `Math`、`Date`、`JSON` 等。

#### **Node.js 示例**：
```javascript
console.log(global.setTimeout); // 通过 global 访问 setTimeout
```

#### **浏览器示例**：
```javascript
console.log(window === global); // 在浏览器中为 true
```

---

### **3. 关键区别**

| **方面**              | **`window` 对象**                                  | **`global`对象**                                              |
|------------------------|-----------------------------------------------------|----------------------------------------------------------|
| **作用域**             | 仅限于浏览器                                    | 通用（浏览器、Node.js 等）                                |
| **环境**               | 仅在浏览器中可用                                | 取决于环境（`window`、`global` 或 `self`）               |
| **内容**               | 包含浏览器特定的 API 和方法                        | 包含标准的全局对象（`Math`、`JSON` 等）                  |
| **全局变量**           | 作为 `window` 的属性可访问                        | 作为全局对象的属性可访问                                  |
| **Node.js 支持**       | 不可用                                          | 使用 `global` 代替                                      |

---

### **4. 使用 `globalThis` 实现通用访问**
- ES2020 引入了 `globalThis`，提供了一种不依赖于环境的通用方式来访问全局对象：
  - 在浏览器中，`globalThis` 等同于 `window`。
  - 在 Node.js 中，`globalThis` 等同于 `global`。
  - 在 Web Workers 中，`globalThis` 等同于 `self`。

#### **示例**：
```javascript
// 在不同环境中都有效
console.log(globalThis.setTimeout === setTimeout); // true
```

---

### **5. 重叠与差异**
#### **重叠**
在浏览器中：
```javascript
var x = 10; // 声明一个全局变量
console.log(window.x); // 10（与 global.x 相同）
```

#### **差异**
```javascript
// Node.js 示例
global.x = 10; // 在 global 中可用
console.log(global.x); // 10
console.log(window.x); // 错误：window 未定义
```

---

### **结论**
- 当处理与浏览器相关的功能（如 DOM 操作、位置、警告等）时，使用 **`window` 对象**。
- 使用 **全局对象**（或 `globalThis` 以确保跨环境兼容性）编写与环境无关的代码。

---

## Cover 图

![cover_1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/21/21eee1f89656d2b02d8c1456aeb266934730bd72d9aa5c7bd4580cfc4d960397.png)
