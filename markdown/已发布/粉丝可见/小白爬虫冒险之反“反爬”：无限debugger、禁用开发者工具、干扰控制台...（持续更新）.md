
# 背景浅谈

> 小白踏足JS逆向领域也有一年了，对于逆向这个需求呢主要要求就是让我们去破解**“反爬机制”**，即反“反爬”，脚本处理层面一般都是`decipher`网站对`request`设置的`cipher`，比如破解一个`DES/AES`加密拿到`key`。这篇文章先不去谈这类已经进入JS分析阶段的问题，而是往前推到我们的第一步——**调试**，这也是我们后续分析的前提，学习逆向的大侠们如果自己去找不同的网站练习很容易发现其实很多网站恰恰喜欢在这第一步就设置反爬策略，也就是我这篇博客接下来要谈到的类似于无限debugger的小问题。~~（毕竟网站也不想被逆向“菜狗”一直请求，所以先把一部分“菜狗”拦下来不让你去分析）~~
>
> 大年初一，我先写一部分短时间能想到的，内容可能也会相对粗糙，后期会加以润色，遇到其他的会继续在此更新。

# 问题及应对策略

## 1.无限debugger

### 无限debugger产生原因

防止爬虫人员调试网站、抓包等行为，恶心你，层层下陷的`debugger`仿佛“沼泽陷阱”

### 无限debugger原理

使用`debugger`关键字与`setInterval()`或者`setTimeout()`配合使用造成无限创建虚拟机debugger

#### `setInterval()`

点不完的定时器

#### `setTimeout()`

配合`setInterval()`组成递归从而造成无限内陷无法自拔直至程序崩溃

### 无限debugger破解思路

  1. 断点设置一律不在此处断住

  2. 断点设置条件置为`false`

  3. 被第一个`debugger`断住后利用请求堆栈向上溯源利用**无限debugger原理**定位调用入口，破解调用入口

  4. **脚本注入**：  

     ```javascript
     // 重写 debugger 函数
     window.eval = (code) => { 
       if (!code.includes("debugger")) eval(code); 
     };
     ```

## 2.禁用开发者工具

当你打开一个网站点击F12准备“大干一场”的时候突然发现网站不允许调试，倘若你刚好是一名“菜鸡”，那你不就炸了吗？

网站禁用开发者工具的实现通常依赖于检测用户是否打开了开发者工具，并通过技术手段进行干扰。以下是其底层原理及应对策略的分析：

---

### **一、禁用开发者工具的底层原理**

1. **窗口尺寸检测**

   - **原理**：开发者工具打开后，浏览器窗口的尺寸或布局可能发生变化（如窗口分栏）。网站通过监听 `window.resize` 事件或对比 `window.outerWidth/innerWidth` 的差值来判断。
   - **局限性**：响应式设计的网站可能误判，且用户可通过取消开发者工具独立窗口规避。

2. **控制台属性检测**

   - **原理**：通过检查 `console` 对象或 `debugger` 关键字的状态。例如：

     ```javascript
     setInterval(() => {
       if (console.firebug || /./.constructor.prototype.toString = () => {}) {
         alert("开发者工具已打开！");
         window.location.href = "about:blank"; // 强制跳转
       }
     }, 1000);
     ```

   - **局限性**：现代浏览器已修复大部分漏洞，且用户可通过禁用控制台日志输出绕过。

3. **键盘事件监听**

   - **原理**：监听 `F12`、`Ctrl+Shift+I`、`Ctrl+Shift+J` 等快捷键的按下事件，阻止默认行为：

     ```javascript
     document.addEventListener('keydown', (e) => {
       if (e.keyCode === 123 || (e.ctrlKey && e.shiftKey && e.keyCode === 73)) {
         e.preventDefault();
         window.location.href = "about:blank";
       }
     });
     ```

   - **局限性**：无法阻止通过浏览器菜单手动打开开发者工具。

---

### **二、应对策略**

有一种很简单的绕过方式就是提前将开发者工具的窗口设置为独立窗口

#### **1. 底层原理分析**

**开发者工具独立窗口的作用**：  
当开发者工具以独立窗口（非停靠模式）打开时，主浏览器窗口的布局和尺寸不会发生变化。这直接影响网站通过 **窗口尺寸变化** 或 **布局偏移** 来检测开发者工具的机制。

---

#### **2. 针对窗口尺寸检测的绕过**

**原检测逻辑**：  
网站通过监听 `resize` 事件或比较 `window.outerWidth` 和 `window.innerWidth` 的差值，判断开发者工具是否打开（停靠模式会改变主窗口尺寸）。

**代码映射**：  

```javascript
// 原代码中的窗口监听
window.addEventListener("resize", e); // 监听窗口变化触发检测
var c = setInterval(e, 500);          // 定时检测
```

**独立窗口的绕过效果**：  

- 主窗口尺寸不变，`resize` 事件不会被触发。  
- 定时执行的 `e()` 函数仍会运行，但若其逻辑依赖窗口尺寸，则无法检测到工具开启。  

**局限性**：  
若 `e()` 函数包含其他检测逻辑（如控制台属性劫持），独立窗口无法绕过这些检测。

---

#### **3. 有效性**

| **检测类型**      | **独立窗口绕过效果**       | **需额外应对措施**                   |
| ----------------- | -------------------------- | ------------------------------------ |
| 窗口尺寸/布局变化 | **有效**                   | 无需额外操作                         |
| 控制台属性劫持    | **无效**                   | 禁用 `__defineGetter__` 或静默控制台 |
| 定时轮询检测      | **部分有效**               | 清除定时器 (`clearInterval`)         |
| Firebug 对象检测  | **有效**（Firebug 已淘汰） | 无需操作                             |

## 3.干扰控制台

---

### **网站干扰控制台的底层实现原理及应对策略**

网站干扰控制台的目的是阻止用户通过开发者工具（如控制台）调试、分析或修改页面逻辑。以下是常见的干扰手段及其应对方法：

---

### **一、底层实现原理**

#### **1. 禁用控制台方法（Console Methods）**  

**原理**：  
通过重写 `console.log`、`console.error` 等方法，使其无法输出内容或抛出错误。  

```javascript
// 示例：禁用 console.log
console.log = function() {}; 
// 或抛出错误
console.log = function() { throw new Error("Console is disabled"); };
```

**效果**：  
用户在控制台执行 `console.log` 时无输出或直接报错。

---

#### **2. 控制台打开检测与警告**  

**原理**：  
通过对比窗口尺寸、计算代码执行时间差，或劫持 `console` 对象，检测控制台是否打开。  

```javascript
// 示例：通过代码执行时间差检测
const start = Date.now();
console.log("检测控制台");
const delay = Date.now() - start;
if (delay > 50) { 
  alert("控制台已打开！");
  window.location.href = "about:blank"; 
}
```

**效果**：  
用户打开控制台时，页面跳转或弹出警告。

---

#### **3. 控制台输出劫持**  

**原理**：  
劫持 `console` 方法，修改输出内容或频率。  

```javascript
// 示例：劫持 console.log 输出乱码
const originalLog = console.log;
console.log = function(...args) {
  originalLog.call(console, "干扰输出: " + Math.random().toString(36));
};
```

**效果**：  
用户看到的控制台输出被篡改，无法获取真实信息。

---

#### **4. 内存耗尽攻击**  

**原理**：  
通过高频输出大量内容或死循环，导致控制台卡死或浏览器崩溃。  

```javascript
// 示例：每秒输出 10 万条日志
setInterval(() => {
  for (let i = 0; i < 1e5; i++) console.log("垃圾数据");
}, 1000);
```

**效果**：  
控制台因处理海量日志而失去响应。

---

### **二、应对策略**

#### **1. 恢复原生 Console 方法**  

**方法**：  
在控制台中重置 `console` 对象，或使用浏览器插件提前注入修复脚本。  

```javascript
// 在控制台执行以下代码恢复 console.log
delete console.log; // 仅对部分重写有效
// 或直接从 iframe 中获取原生 console
const iframe = document.createElement('iframe');
document.body.appendChild(iframe);
console.log = iframe.contentWindow.console.log;
```

**适用场景**：  
针对 `console` 方法被重写或禁用的情况。


---

#### **2. 屏蔽控制台检测逻辑**  

**方法**：  
使用浏览器插件（如 某猴）在页面加载前注入代码，覆盖检测逻辑。  

```javascript
// ==UserScript==
// @run-at       document-start
// 禁用控制台检测
Object.defineProperty(window, 'console', {
  value: window.console,
  writable: false,
  configurable: false
});
// 覆盖定时器函数
window.setInterval = function() {}; // 禁用所有定时器
```

**适用场景**：  
针对基于定时器或 `console` 劫持的检测。

---

#### **3. 使用无头浏览器或代理拦截**  

**方法**：  
通过无头浏览器（如 Puppeteer）或本地代理（如 Charles）直接修改网页内容。  

- **Puppeteer 示例**：  

  ```javascript
  const puppeteer = require('puppeteer');
  (async () => {
    const browser = await puppeteer.launch();
    const page = await browser.newPage();
    await page.setRequestInterception(true);
    // 拦截并删除干扰脚本
    page.on('request', (req) => {
      if (req.url().includes('anti-console.js')) req.abort();
      else req.continue();
    });
    await page.goto('https://target-site.com');
  })();
  ```

  **适用场景**：  
  自动化绕过所有前端干扰逻辑。

---

#### **4. 禁用 JavaScript 执行**  

**方法**：  
通过浏览器设置或插件（如 NoScript）直接禁用页面 JavaScript。  

- **操作路径**：  
  Chrome → 设置 → 隐私与安全 → 网站设置 → JavaScript → 禁用。  
  **缺点**：  
  可能导致页面功能完全失效。

---

### **三、总结：干扰手段与应对对照表**

| **干扰手段**      | **底层原理**                  | **应对策略**                              |
| ----------------- | ----------------------------- | ----------------------------------------- |
| 禁用 Console 方法 | 重写 `console.log` 等原生方法 | 恢复 `console` 或通过 iframe 获取原生方法 |
| 控制台打开检测    | 窗口尺寸/代码执行时间差检测   | 覆盖检测逻辑或使用无头浏览器              |
| 控制台输出劫持    | 篡改 `console` 输出内容       | 重置 `console` 或拦截日志输出             |
| 内存耗尽攻击      | 高频输出海量日志              | 禁用控制台日志或过滤高频输出              |
