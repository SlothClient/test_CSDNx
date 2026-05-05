# DOM导航属性、元素与结点

这是一份速查+避坑指南，帮你弄清“元素(Element)”与“结点(Node)”的区别，以及如何正确导航父/子/兄弟节点。

## 一、元素 vs 结点

- 结点 Node：DOM中的一切都是结点。常见类型
  - 元素结点：Node.ELEMENT_NODE = 1，如 div、span
  - 文本结点：Node.TEXT_NODE = 3，包含空白与换行
  - 注释结点：Node.COMMENT_NODE = 8
  - 文档结点：Node.DOCUMENT_NODE = 9（document）
  - 文档片段：Node.DOCUMENT_FRAGMENT_NODE = 11（DocumentFragment）
- 元素 Element：结点的子集，只有标签本身。元素有专属的“Element系”导航属性，能自动跳过文本结点（换行空白）。

常用只读标识
- nodeType（数字）、nodeName（如 DIV、#text）、tagName（仅元素有）
- nodeValue（文本/注释有效）、textContent/innerText/innerHTML

## 二、导航属性对照（元素系优先）

- 父级
  - parentNode：任意结点的父结点
  - parentElement：父级若不是元素则为 null（如 document 的 parentElement 为 null）
- 子级
  - childNodes：NodeList，含文本结点
  - children：HTMLCollection，仅元素，活的集合
  - firstChild/lastChild：可能拿到文本
  - firstElementChild/lastElementChild：只拿元素
- 兄弟
  - previousSibling/nextSibling：可能是文本
  - previousElementSibling/nextElementSibling：只元素
- 其他
  - contains(node)、closest(selector)、matches(selector)

建议
- 导航优先使用 Element 系属性（children、firstElementChild、nextElementSibling...），避免被空白文本结点绊倒。

## 三、静态 vs 活集合

- NodeList
  - querySelectorAll 返回“静态”NodeList（后续 DOM 变化不自动更新）
  - childNodes 返回“活”的 NodeList（会随 DOM 变化）
- HTMLCollection
  - 如 children、getElementsByClassName 返回“活集合”

将集合转为数组便于操作
````js
const arr1 = Array.from(element.children);      // HTMLCollection → Array
const arr2 = [...element.querySelectorAll('li')]; // NodeList(静态) → Array
````

## 四、实用示例与常见坑

示例 HTML
````html
<ul id="list">
  <li>JavaScript</li>
  <li>DOM</li>
  <!-- comment -->
  <li><span>CSS</span></li>
</ul>
````

1) 正确遍历子元素（跳过空白文本）
````js
const ul = document.getElementById('list');
for (const li of ul.children) {
  console.log(li.tagName, li.textContent.trim());
}
````

2) 错误遍历（会拿到文本结点）
````js
for (const node of ul.childNodes) {
  // 可能是 #text / #comment，访问 node.tagName 会是 undefined
}
````

3) 安全获取兄弟元素
````js
const second = ul.children[1];
const prev = second.previousElementSibling; // 第一个 li
const next = second.nextElementSibling;     // 第三个 li
````

4) 过滤成元素数组
````js
const onlyElements = [...ul.childNodes].filter(n => n.nodeType === Node.ELEMENT_NODE);
````

5) 最近匹配祖先
````js
const span = ul.querySelector('span');
const li = span.closest('li'); // 找到最近的 li 祖先
````

6) 文本处理与归一化
````js
// 把相邻文本结点合并，便于处理
ul.normalize();
````

## 五、选择器与导航的组合拳

- 定位：document.querySelector / querySelectorAll
- 局部范围：container.querySelectorAll
- 导航：基于 Element 系属性做父/子/兄弟移动
- 判断：element.matches('.active') / element.contains(child)

示例：取所有直接子 li 的下一个兄弟 li 的文本
````js
const texts = [...ul.children]
  .map(li => li.nextElementSibling?.textContent.trim())
  .filter(Boolean);
console.log(texts);
````

## 六、速记要点

- 需要“只要元素”用 Element 系：children、firstElementChild、nextElementSibling
- 可能拿到文本结点的：childNodes、firstChild、nextSibling
- NodeList(静态/活)与HTMLCollection(活)要分清，操作前转数组更稳
- 处理文本用 textContent，读写结构用 innerHTML，调试看 nodeType/nodeName
