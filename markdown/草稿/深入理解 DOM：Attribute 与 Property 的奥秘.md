# 深入理解 DOM：Attribute 与 Property 的奥秘

在前端开发中，我们经常需要与 HTML 元素（Element）打交道，操作它们的特性，例如修改一个输入的值、隐藏一个div或者勾选一个复选框。在这个过程中，我们经常会使用到两类方法例如`elt.getAttribute([specific_attr])` & `elt.[specific_prop]`，涉及到两个概念：**attribute** 和 **property**。它们中文都可以翻译为“属性”，但在 DOM 的世界里，它们代表的是两个截然不同的东西。混淆它们是许多难以排查的 Bug 的根源。

本文将深入探讨 attribute 和 property 的区别与联系，并以 `hidden` 和 `checked` 这两个典型的属性为例，帮助你彻底理解它们。

## 一、核心概念：Attribute 与 Property

### 1. Attribute (HTML 属性)

*   **是什么**：Attribute 是 **写在 HTML 代码中** 的键值对。它是 HTML 文本的一部分。
*   **位置**：存在于 HTML 源码中。
*   **类型**：值**总是字符串**。即使你写的是 `checked="true"`，它的值也是字符串 `"true"`。
*   **操作方式**：
    *   **获取**：`element.getAttribute(‘attrName’)`
    *   **设置**：`element.setAttribute(‘attrName’, ‘value’)`
    *   **删除**：`element.removeAttribute(‘attrName’)`
*   **大小写**：**不敏感**。HTML 规范中约定通常使用小写。

**示例**：
```html
<input id="myInput" type="checkbox" checked="checked" data-custom="someValue">
```
这里的 `id`, `type`, `checked`, `data-custom` 都是 attribute。

### 2. Property (DOM 属性)

*   **是什么**：Property 是 DOM 对象（在内存中）的**属性**。当浏览器解析 HTML 后，会创建相应的 DOM 对象节点。HTML 的标准 attribute 会被同步到这些对象的 properties 上。
*   **位置**：存在于内存中的 JavaScript 对象里。
*   **类型**：值可以是**任何 JavaScript 类型**（字符串、布尔值、数字、对象等）。
*   **操作方式**：像操作普通 JavaScript 对象属性一样，使用**点运算符**或**方括号**。
    *   **获取**：`element.propertyName`
    *   **设置**：`element.propertyName = value`
*   **特点**：原理上，property 的值**当前状态**的反映，而 attribute 是**初始默认值**的反映，一般是不同的。
* **实际上，property和attribute，除了没用`data-`进行命名的自定义属性基本都相同，具体是否相同取决于浏览器的同步机制。**

**示例**：
```javascript
const checkbox = document.getElementById(‘myInput’);
console.log(checkbox.checked); // 输出：true (布尔值)
console.log(checkbox.id); // 输出：'myInput' (字符串)
console.log(checkbox.dataCustom); // undefined，非标准 attribute 不会自动同步
console.log(checkbox.dataset.custom, checkbox.getAttribute(‘data-custom’)); // 输出：'someValue'，需要用 dataset或getAttribute
```

---

## 二、关键区别与联系总结

| 特性 | Attribute | Property |
| :--- | :--- | :--- |
| **所处位置** | HTML **文档**中 | DOM **对象**中 |
| **值的类型** | **总是字符串** | **可以是任意类型**（布尔、数字、对象等） |
| **操作方式** | `get/setAttribute()` | **点符号**（.） |
| **行为代表** | **初始的、默认的**值 | **当前的、动态的**状态值 |
| **自定义数据** | 可以用（如 `data-*`） | 默认不会自动创建 property |

**联系**：**标准化的 attribute**（如 `id`, `class`, `type`, `value`, `checked` 等）在元素被浏览器解析后，会被自动同步到其对应的 DOM 对象的 property 上。这可以看作是一个**“初始化”**的过程。

---

## 三、案例分析：`hidden` 与 `checked`

### 1. `hidden` 属性

`hidden` 是一个布尔属性（Boolean attribute），它的存在即表示真，移除则表示假。

*   **Attribute**:
    ```html
    <div id="myDiv" hidden>这个内容是隐藏的</div>
    ```
    `myDiv.getAttribute(‘hidden’)` 会获取到**空字符串**（`“”`）—— 这是布尔 attribute 的特点，只要存在，`getAttribute` 返回的就是空字符串。如果不存在，则返回 `null`。

*   **Property**:
    DOM 对象上的 `hidden` property 被映射为**布尔类型**。
    ```javascript
    const div = document.getElementById(‘myDiv’);
    console.log(div.hidden); // 输出：true (布尔值)

    // 通过 property 操作（推荐）
    div.hidden = false; // 使元素显示
    div.hidden = true; // 使元素隐藏

    // 通过 attribute 操作
    div.setAttribute(‘hidden’, ‘’); // 使元素隐藏
    div.removeAttribute(‘hidden’); // 使元素显示
    ```

对于 `hidden`，**强烈推荐使用 property 方式来操作**（`element.hidden = true/false`），因为它更直观、性能更好，且符合 JavaScript 的语法习惯。

### 2. `checked` 属性

`checked` 是另一个典型的布尔 attribute，主要用于复选框（`<input type=“checkbox”>`）和单选框（`<input type=“radio”>`）。它的行为是理解两者差异的绝佳例子。

*   **Attribute**:
    `checked` attribute 定义的是**默认的选中状态**。
    ```html
    <input type="checkbox" id="myCheckbox" checked>
    ```
    `myCheckbox.getAttribute(‘checked’)` 初始会返回字符串 `“”`。

*   **Property**:
    `checked` property 反映的是复选框**当前实时的、动态的选中状态**。
    ```javascript
    const cb = document.getElementById(‘myCheckbox’);

    // 初始状态
    console.log(cb.getAttribute(‘checked’)); // 输出: “” (字符串，表示初始已设置)
    console.log(cb.checked); // 输出: true (布尔值，表示当前已选中)

    // 用户在页面上点击复选框，取消了勾选
    console.log(cb.getAttribute(‘checked’)); // 输出: “” (字符串，初始值未变！)
    console.log(cb.checked); // 输出: false (布尔值，实时反映了当前未选中状态)

    // 通过 JavaScript 修改
    cb.checked = true; // 通过 property 勾选复选框

    console.log(cb.getAttribute(‘checked’)); // 输出: “” (attribute 依然不变)
    console.log(cb.checked); // 输出: true (property 已变化)
    ```

**关键点**：
*   `getAttribute(‘checked’)` 只告诉你一件事：这个复选框在 HTML 里**初始状态**是否被设置为“已勾选”。它之后永远不会变。
*   `checkbox.checked` 告诉你这个复选框**此时此刻**是否被勾选。它是动态变化的。

**最佳实践**：在处理表单控件（如复选框、单选框、输入框）的**当前值或状态时，永远使用 property**（`.checked`, `.value`, `.selected` 等），因为它反映的是用户的实时操作。你几乎只在需要获取初始默认值时才使用 `getAttribute(‘checked’)`。

---

## 四、总结

1.  **理解本质**：Attribute 是 HTML 的“源代码”，Property 是 JavaScript 内存中对象的“当前状态”。
2.  **操作选择**：
    *   **对于标准属性**（如 `id`, `class`, `href`, `value`, `checked`）的**当前值**，优先使用 **property** 方式（`element.xxx`）来读写。它更快、类型正确、且反映当前状态。
    *   当你需要**获取或设置一个自定义的 attribute**（例如 `data-*`）时，必须使用 `getAttribute()` 和 `setAttribute()` 方法。
    *   当你需要**获取一个标准属性的初始默认值**（而非当前状态）时，使用 `getAttribute()`。
3.  **特例注意**：`input` 元素的 `value` attribute 定义的是初始值，而 `value` property 是当前值。修改 property 不会更新 attribute，这与其他属性类似。
    ```html
    <input id="user" value="默认名">
    ```
    ```javascript
    const input = document.getElementById(‘user’);
    console.log(input.getAttribute(‘value’)); // ‘默认名’
    console.log(input.value); // ‘默认名’

    input.value = ‘新名字’;
    console.log(input.getAttribute(‘value’)); // ‘默认名’ (未变)
    console.log(input.value); // ‘新名字’ (已变)
    ```
4.  **自定义数据**：对于自定义数据，请使用 `data-*` attribute，并通过 DOM 对象的 `dataset` property 来访问，这是连接自定义 attribute 和 property 的桥梁。

希望这篇博客能帮助你彻底厘清 attribute 和 property 的区别，让你在未来的 DOM 操作中更加得心应手！
