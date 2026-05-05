# CSS 变量（自定义属性）详解

CSS 变量（也称为 CSS 自定义属性）是 CSS 的一项强大功能，它允许您在样式表中定义可重用的值，并在整个文档中引用这些值。

## 基本用法

### 定义变量

```css
:root {
  --primary-color: #4285f4;
  --secondary-color: #34a853;
  --max-width: 1200px;
  --base-font-size: 16px;
}
```

- 变量名以 `--` 开头（如 `--primary-color`）
- 通常在 `:root` 伪类中定义全局变量（相当于 HTML 元素）
- 也可以在特定选择器中定义局部变量

### 使用变量

```css
.header {
  background-color: var(--primary-color);
  max-width: var(--max-width);
}

.button {
  background-color: var(--primary-color);
  font-size: calc(var(--base-font-size) * 1.2);
}
```

## 变量作用域

CSS 变量有作用域的概念：

```css
/* 全局变量 */
:root {
  --global-var: red;
}

/* 局部变量 - 只在 .container 及其子元素中有效 */
.container {
  --local-var: blue;
}

/* 可以覆盖全局变量 */
.special-container {
  --global-var: green;
}
```

## 变量继承

CSS 变量遵循 DOM 继承规则：

```html
<div class="parent">
  <div class="child"></div>
</div>
```

```css
.parent {
  --parent-var: 20px;
}

.child {
  /* 可以访问 --parent-var */
  padding: var(--parent-var);
}
```

## 默认值

当变量未定义时，可以设置回退值：

```css
.element {
  color: var(--undefined-var, black); /* 如果 --undefined-var 未定义，使用 black */
  margin: var(--margin, 10px 20px); /* 复合值也可以 */
}
```

## JavaScript 交互

可以通过 JavaScript 动态修改变量：

```javascript
// 获取根元素
const root = document.documentElement;

// 设置变量
root.style.setProperty('--primary-color', '#ff0000');

// 读取变量
const color = getComputedStyle(root).getPropertyValue('--primary-color');

// 移除变量
root.style.removeProperty('--primary-color');
```

## 实际应用示例

```css
:root {
  --primary: #6200ee;
  --primary-light: #9e47ff;
  --primary-dark: #0400ba;
  --secondary: #03dac6;
  --error: #b00020;
  --surface: #ffffff;
  --background: #f5f5f5;
  --on-primary: #ffffff;
  --on-secondary: #000000;
  --on-error: #ffffff;
  --on-surface: #000000;
  --spacing-unit: 8px;
  --border-radius: 4px;
  --box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.button {
  padding: calc(var(--spacing-unit) * 2) calc(var(--spacing-unit) * 4);
  border-radius: var(--border-radius);
  background-color: var(--primary);
  color: var(--on-primary);
  box-shadow: var(--box-shadow);
  transition: background-color 0.3s;
}

.button:hover {
  background-color: var(--primary-light);
}

.card {
  background-color: var(--surface);
  border-radius: var(--border-radius);
  box-shadow: var(--box-shadow);
  padding: calc(var(--spacing-unit) * 3);
}
```

## 注意事项

1. **浏览器支持**：现代浏览器都支持 CSS 变量（IE11 及以下不支持）
2. **大小写敏感**：`--color` 和 `--Color` 是不同的变量
3. **值计算**：变量值在计算时解析，定义时不解析
4. **无效值**：如果变量值无效，浏览器会使用继承值或初始值
5. **性能**：过度使用复杂变量可能影响性能

CSS 变量特别适合主题切换、响应式设计和设计系统实现，大大提高了 CSS 的可维护性和灵活性。
___
# JavaScript 操作 CSS 变量（自定义属性）的完整指南

CSS 变量（自定义属性）可以通过 JavaScript 动态操作，这为创建动态主题、实时样式调整和交互效果提供了强大支持。以下是详细的使用方法：

## 1. 设置 CSS 变量

### 在根元素上设置全局变量

```javascript
// 获取根元素
const root = document.documentElement;

// 设置变量
root.style.setProperty('--primary-color', '#4285f4');
root.style.setProperty('--header-height', '80px');
```

### 在特定元素上设置局部变量

```javascript
const element = document.querySelector('.my-element');
element.style.setProperty('--element-bg', '#ff0000');
```

## 2. 获取 CSS 变量值

### 获取全局变量值

```javascript
const rootStyles = getComputedStyle(document.documentElement);
const primaryColor = rootStyles.getPropertyValue('--primary-color').trim();
console.log(primaryColor); // 输出: "#4285f4"
```

### 获取特定元素的变量值

```javascript
const element = document.querySelector('.my-element');
const elementStyles = getComputedStyle(element);
const bgColor = elementStyles.getPropertyValue('--element-bg').trim();
```

## 3. 删除 CSS 变量

```javascript
// 从根元素删除
document.documentElement.style.removeProperty('--primary-color');

// 从特定元素删除
document.querySelector('.my-element').style.removeProperty('--element-bg');
```

## 4. 实际应用示例

### 动态主题切换

```html
<button id="themeToggle">切换主题</button>
```

```css
:root {
  --bg-color: #ffffff;
  --text-color: #333333;
  --primary: #4285f4;
}

.dark-theme {
  --bg-color: #222222;
  --text-color: #ffffff;
  --primary: #34a853;
}

body {
  background-color: var(--bg-color);
  color: var(--text-color);
  transition: all 0.3s ease;
}
```

```javascript
const themeToggle = document.getElementById('themeToggle');
themeToggle.addEventListener('click', () => {
  document.body.classList.toggle('dark-theme');
});
```

### 实时调整元素大小

```html
<input type="range" id="sizeSlider" min="10" max="100" value="50">
<div class="resizable-box"></div>
```

```css
.resizable-box {
  width: var(--box-size, 50px);
  height: var(--box-size, 50px);
  background-color: var(--primary);
  transition: width 0.2s, height 0.2s;
}
```

```javascript
const sizeSlider = document.getElementById('sizeSlider');
const box = document.querySelector('.resizable-box');

sizeSlider.addEventListener('input', (e) => {
  box.style.setProperty('--box-size', `${e.target.value}px`);
});
```

## 5. 高级用法

### 操作多个变量

```javascript
function setTheme(primary, secondary, text) {
  const root = document.documentElement;
  root.style.setProperty('--primary', primary);
  root.style.setProperty('--secondary', secondary);
  root.style.setProperty('--text-color', text);
}

// 使用
setTheme('#4285f4', '#34a853', '#333333');
```

### 响应式变量调整

```javascript
function updateSpacing() {
  const spacing = window.innerWidth < 768 ? '8px' : '16px';
  document.documentElement.style.setProperty('--spacing-unit', spacing);
}

// 初始设置
updateSpacing();

// 窗口大小变化时更新
window.addEventListener('resize', updateSpacing);
```

## 6. 注意事项

1. **浏览器兼容性**：现代浏览器都支持，但IE11及以下不支持
2. **性能考虑**：频繁修改变量可能影响性能，考虑节流
3. **CSS回退**：为不支持CSS变量的浏览器提供回退方案
   ```css
   .element {
     color: #333; /* 回退值 */
     color: var(--text-color, #333);
   }
   ```
4. **变量命名**：使用有意义的名称，保持一致性
5. **作用域**：注意变量的作用域（全局:root vs 局部元素）

## 7. 与CSS预处理器变量区别

| 特性 | CSS变量 | SASS/LESS变量 |
|------|---------|---------------|
| 作用域 | 运行时，可继承 | 编译时，不可继承 |
| 可修改性 | 可通过JS动态修改 | 编译后固定 |
| 浏览器支持 | 现代浏览器 | 所有浏览器(编译后) |
| 单位处理 | 需要JS处理 | 预处理器可计算 |

通过JavaScript操作CSS变量，您可以创建高度动态和交互式的Web应用，而无需频繁操作DOM或重写样式规则。
