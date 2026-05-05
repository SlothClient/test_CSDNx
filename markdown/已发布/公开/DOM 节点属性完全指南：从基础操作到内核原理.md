@[toc]
# DOM 节点属性完全指南：从基础操作到内核原理

## 一、引言：万物皆节点

在深入属性之前，我们必须理解 DOM（文档对象模型）的核心基础：**一切皆为节点**。浏览器将 HTML 文档解析为一个由各种节点组成的树形结构。常见的节点类型包括：

1.  **元素节点** (Element Node)：如 `<div>`、`<p>`、`<span>`，构成了页面的骨架。
2.  **文本节点** (Text Node)：包含在元素内的文本内容，是元素的子节点。
3.  **属性节点** (Attribute Node)：元素的特性，如 `id`、`class`。*(注意：在现代 DOM 操作中，属性节点较少直接通过节点 API 操作)*
4.  **注释节点** (Comment Node)：HTML 中的注释 `<!-- 注释 -->`。

JavaScript 为我们提供了丰富的属性来操作这些节点。本文将系统性地介绍这些属性，并深入剖析易混淆的 `nodeValue`、`data`、`innerHTML` 和 `textContent`。

---

## 二、节点基础属性

这些属性定义在 `Node` 接口上，几乎所有类型的节点都拥有它们。

### 1. `nodeName` 与 `tagName`
*   **`nodeName`**：所有节点都拥有。对于**元素节点**，返回**大写**的标签名（如 `"DIV"`）；对于文本节点，返回 `"#text"`；对于注释节点，返回 `"#comment"`。
*   **`tagName`**：**仅元素节点**拥有。返回元素的**大写**标签名。
*   **注意**：`tagName` 是 `Element` 接口的属性，所以非元素节点没有此属性。

```javascript
document.body.nodeName; // "BODY"
document.body.tagName;  // "BODY"

const textNode = document.createTextNode('hello');
textNode.nodeName; // "#text"
textNode.tagName;  // undefined
```

### 2. `nodeType`
*   只读属性，返回一个代表**节点类型**的数字常量。
*   这是判断节点类型最可靠的方法。
*   常用值：
    *   `Node.ELEMENT_NODE` (1) - 元素节点
    *   `Node.TEXT_NODE` (3) - 文本节点
    *   `Node.COMMENT_NODE` (8) - 注释节点

```javascript
if (someNode.nodeType === Node.ELEMENT_NODE) {
  // 这是一个元素节点
}
if (someNode.nodeType === 1) { // 直接使用数字也可，但常量更清晰
  // 这也是一个元素节点
}
```

### 3. `childNodes` 与 `children`
*   **`childNodes`**：返回一个包含所有**子节点**的类数组对象（NodeList），包括元素节点、文本节点、注释节点。
*   **`children`**：返回一个仅包含**元素子节点**的类数组对象（HTMLCollection）。**这是与 `childNodes` 最关键的区别**。

```html
<div id="parent">
  我是文本节点
  <p>我是元素节点</p>
  <!-- 我是注释节点 -->
</div>
```

```javascript
const parent = document.getElementById('parent');
console.log(parent.childNodes); // NodeList(5) [text, p, text, comment, text]
console.log(parent.children);   // HTMLCollection [p]
```

---

## 三、内容操作属性深度对比

这是最容易混淆的一组属性，理解了它们，就掌握了 DOM 内容操作的核心。

### 1. `nodeValue` 与 `data` (孪生属性)

*   **适用对象**：**文本节点**和**注释节点**。
*   **对于元素节点**：它们的值永远是 `null`。对元素节点设置它们不会有任何效果。
*   **功能**：获取或设置**文本/注释节点**的原始文本内容。
*   **关系**：对于文本和注释节点，`nodeValue` 和 `data` 的值完全相同。可以认为 `data` 是 `CharacterData` 接口（Text 和 Comment 的基类）的专门属性，而 `nodeValue` 是更通用的 Node 接口的属性。

```html
<p id="myPara">Hello</p>
```

```javascript
const para = document.getElementById('myPara');
const textNode = para.firstChild; // 获取 <p> 下的文本节点 "Hello"

console.log(textNode.nodeValue); // 'Hello'
console.log(textNode.data);      // 'Hello'

// 修改文本内容
textNode.data = 'Hi'; // 等同于 textNode.nodeValue = 'Hi';
```

### 2. `textContent` (纯文本之王)

*   **适用对象**：**元素节点**。
*   **获取**：返回该元素**及其所有后代**的文本内容拼接而成的字符串。它会**忽略所有内部的 HTML 标签**，只提取纯文本，包括 `<script>` 和 `<style>` 标签内的内容以及 `display: none` 隐藏的元素文本。
*   **设置**：将给定的字符串**作为纯文本插入**。字符串中的任何 HTML 标签都会被转义为文本实体（如 `<` 变为 `&lt;`），而不会被解析为 DOM 结构。
*   **优点**：**性能高**（无需解析 HTML），**安全性高**（天然防御 XSS 攻击）。

```html
<div id="example">
  Hello
  <span style="display:none">Secret Text</span>
  <!-- A comment -->
  <p>World</p>
</div>
```

```javascript
const el = document.getElementById('example');
console.log(el.textContent);
// 输出（可能包含换行和空格）: "\n  Hello\n  Secret Text\n  \n  World\n"

// 设置 textContent
el.textContent = '<strong>New</strong> Content';
// 结果：页面上显示文字 "<strong>New</strong> Content"
```

### 3. `innerHTML` (HTML 解析器)

*   **适用对象**：**元素节点**。
*   **获取**：返回该元素所有子节点的 **HTML 序列化字符串**，包括标签、文本和注释。它是“所见即所得”的 HTML 代码。
*   **设置**：将给定的字符串**作为 HTML 代码交给浏览器解析**，并用解析得到的新 DOM 树替换元素原有的所有子节点。
*   **优点**：功能强大，可动态创建复杂 DOM 结构。
*   **缺点**：
    *   **极高的安全风险 (XSS)**：直接插入用户输入会导致脚本执行。
    *   **性能开销**：触发 HTML 解析器，比 `textContent` 慢。
    *   **破坏现有引用**：清空所有子节点，可能导致内存泄漏和事件监听器失效。

```html
<div id="innerHTMLDemo">Hello <em>World</em></div>
```

```javascript
const demoEl = document.getElementById('innerHTMLDemo');
console.log(demoEl.innerHTML); // "Hello <em>World</em>"

// 设置 innerHTML - 会解析HTML标签
demoEl.innerHTML = '<strong>New</strong> Content'; // 页面会显示加粗的"New"
// ⚠️ 危险！
demoEl.innerHTML = '<img src=x onerror="alert(\'Hacked!\')">'; // XSS攻击！
```

### 4. `outerHTML`
*   **适用对象**：**元素节点**。
*   **功能**：代表了**包括元素自身及其所有子节点**的 HTML 代码。
*   **设置**：用给定的 HTML 字符串**替换掉整个元素本身**。

```html
<div id="outerDemo"><p>Inner</p></div>
```

```javascript
const el = document.getElementById('outerDemo');
console.log(el.outerHTML); // <div id="outerDemo"><p>Inner</p></div>

el.outerHTML = '<span>Replaced</span>';
// 现在页面上原来的 <div> 消失了，变成了 <span>Replaced</span>
```

### 终极对比表格

| 属性 | 适用节点 | 功能 | 解析 HTML？ | 安全性 | 性能 | 用途 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **`nodeValue`/`data`** | 文本、注释节点 | 操作节点本身的原始文本 | 不解析 | 安全 | 高 | 直接操作特定文本节点 |
| **`textContent`** | **元素节点** | 获取/设置元素及其后代的**所有纯文本** | **不解析**（转义） | **高** | **高** | **安全地插入或获取纯文本内容** |
| **`innerHTML`** | **元素节点** | 获取/设置元素内部的**HTML代码** | **解析** | **低**（XSS风险） | 中 | **动态创建和插入复杂的 DOM 结构** |
| **`outerHTML`** | **元素节点** | 获取/设置**包括自身**的HTML代码 | **解析** | **低**（XSS风险） | 中 | **替换整个元素** |

---

## 四、其他常用属性

### 1. `hidden` (全局属性)
*   一个布尔属性，表示元素是否隐藏。
*   设置 `element.hidden = true` 等同于添加 `style="display: none"`。

### 2. `value`
*   主要用于表单控件（`<input>`, `<select>`, `<textarea>`）。
*   表示表单控件的**当前值**。注意与 `defaultValue`（初始值）区分。

### 3. `style`
*   用于**读取和写入元素的内联样式**。
*   `element.style.color = 'red';`
*   注意：它只获取**内联样式**（写在 `style` attribute 里的），而不是通过 CSS 样式表计算的最终样式。

### 4. `classList` 与 `className`
*   **`className`**：获取或设置元素的 `class` attribute 的字符串值（例如：`"btn btn-primary"`）。
*   **`classList`**：返回一个元素的 `class` 属性的实时 `DOMTokenList` 集合，提供了一系列好用的方法：
    *   `add(className)`
    *   `remove(className)`
    *   `toggle(className)`
    *   `contains(className)`
*   **现代开发中，强烈推荐使用 `classList`**，因为它更清晰、更易于操作。

```javascript
const div = document.querySelector('div');
div.classList.add('active'); // 添加类
div.classList.remove('old'); // 移除类
div.classList.toggle('hidden'); // 切换类
```

---

## 五、总结与最佳实践

1.  **判断节点类型**：使用 `nodeType`。
2.  **遍历子节点**：
    *   想要所有类型的子节点 → `childNodes`
    *   只想要元素子节点 → `children`
3.  **操作内容**：
    *   **只想处理纯文本** → **`textContent`** (安全、高效)
    *   **需要处理 HTML 结构** → **`innerHTML`/**`outerHTML`** (务必警惕 XSS！)
    *   **操作特定文本节点** → `nodeValue`/`data`
4.  **操作类名**：优先使用 **`classList`** 而非 `className`。
5.  **操作样式**：使用 `style` 对象修改内联样式。
6.  **安全第一**：永远不要相信用户的输入，如果要将用户输入渲染到页面上，优先使用 `textContent`，如果必须使用 `innerHTML`，必须在服务器和客户端对内容进行严格的转义和过滤。

希望这篇全面的指南能帮助你彻底理解 DOM 节点的属性，并在实践中游刃有余。
