# 使用HTML、CSS和JavaScript创建动态随机验证码页面

在这篇博客中，我们将探讨一个简单的网页应用程序，该应用程序使用HTML、CSS和JavaScript生成并显示随机验证码。我们将深入了解代码中涉及的各个知识点，包括CSS变量、JavaScript随机数生成、DOM操作以及CSS混合模式。

## HTML结构

首先，我们来看一下HTML的基本结构：

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>random综合应用案例</title>
    <style>
        /* CSS样式将在后面详细介绍 */
    </style>
</head>
<body>
    <div id="showCard"></div>
    <script>
        // JavaScript代码将在后面详细介绍
    </script>
</body>
</html>
```

这个HTML文件定义了一个简单的网页结构，包含一个`<div>`元素用于显示内容，以及一个`<script>`标签用于嵌入JavaScript代码。

## CSS样式

### CSS变量

在CSS中，我们使用了CSS变量来定义一些常用的样式属性：

```css
:root {
    --card-width: 50vw;
    --card-height: 20vh;
}
```

CSS变量（也称为自定义属性）允许我们在CSS中定义可重用的值。这里，我们定义了卡片的宽度和高度，使用`vw`和`vh`单位来实现响应式设计。

### 布局和样式

```css
		* {
            margin: 0;
            padding: 0;
        }
        
        body {
            width: 100vw;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        #showCard {
            min-width: var(--card-width);
            max-width: fit-content;
            min-height: var(--card-height);
            max-height: fit-content;
            background-color: rgba(255, 255, 255, 0);
            border-radius: 50px;
            box-shadow: inset 0px 0px 10px #000;
            /* line-height设置为showCard的高度，响应式，随着showCard高度的变化而变化，如showCard变为30%，则跟随变化 */
            /* line-height: var(--card-height); */
            padding: 0 20px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
```

我们使用Flexbox来居中对齐`<div>`元素，并为其设置了圆角和阴影效果。`background-color`使用了`rgba`格式，允许我们设置透明度。

### 混合模式

```css
		h1 {
            /* 文字与背景形成对比色 */
            color: white;
            mix-blend-mode: difference;
            font-family: "幼圆","苹方";
            width: fit-content;
            /* margin: 0 auto; */
            /* 自动换行，行距为0 */
            text-wrap: auto;
            line-height: 1;
        }
```

`mix-blend-mode: difference;`用于定义元素内容与背景的混合模式。`difference`模式会根据背景色和前景色的差异来显示颜色，这在某些设计中可以产生有趣的视觉效果。`line-height:1`设置换行后行距为0。

### 选中效果

```css
h1::selection {
    color: black;
    background-color: yellow;
}
```

我们使用`::selection`伪元素来定义文本被选中时的样式。在这里，我们设置了选中文字的颜色为黑色，背景为黄色。（由于上文中的`mix-blend-mode: difference`效果存在差异，可自行调整）

## JavaScript功能

### 随机数生成

```javascript
const rdmNum = (min, max) => {
    return Math.floor(Math.random() * (max - min + 1)) + min;
}
```

这个函数用于生成一个在`min`和`max`之间的随机整数。`Math.random()`生成一个0到1之间的随机数，`Math.floor()`用于向下取整。

### 随机颜色生成

```javascript
const rdmBgColor = (opacity) => {
    return `rgba(${rdmNum(0,255)},${rdmNum(0,255)},${rdmNum(0,255)},${opacity})`;
}
```

这个函数生成一个随机的RGBA颜色值，允许我们为页面背景设置随机颜色。

### 随机验证码生成

```javascript
const rdmPassCode = (len, pool) => {
    let code = "";
    for(let i = 0; i < len; i++) {
        code += pool[rdmNum(0, pool.length - 1)];
    }
    return code;
}
```

这个函数生成一个随机验证码，`len`参数指定验证码的长度，`pool`参数指定可用字符的集合。

### DOM操作

```javascript
document.body.style.backgroundColor = rdmBgColor(0.5);
const pool = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
const h1 = document.createElement("h1");
h1.innerHTML = `您此次的随机验证码为：${rdmPassCode(6, pool)}`;
document.getElementById("showCard").appendChild(h1);
```

我们使用JavaScript动态设置页面背景颜色，并创建一个`<h1>`元素来显示随机验证码。`document.createElement()`用于创建新的DOM元素，`appendChild()`用于将其添加到页面中。

## 结论

通过结合使用HTML、CSS和JavaScript，我们创建了一个简单但功能丰富的网页应用程序。这个示例展示了如何使用CSS变量实现响应式设计，如何使用JavaScript生成随机数和验证码，以及如何通过DOM操作动态更新网页内容。希望这篇博客能帮助你更好地理解这些技术的应用！

---

## Cover 图

![cover_1](html css js关于random函数的动态随机验证码案例（待调整）.assets/ff1e3c46d3a88826.png)
