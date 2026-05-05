![在这里插入图片描述](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/a3/a3c713dea90ddbd247dee83e6b48cf5e56627add248c168e72758004bf113300.png)

```css
html, body, #page {
    width: 100%;
    height: 100%;
    background-color: #000;
}
```
为什么会有滚动条？


```css
html, #page {
    width: 100%;
    height: 100%;
    background-color: #000;
}
```
这个为什么不会有？
___

在 CSS 中，滚动条的出现通常是由元素尺寸超出视口（viewport）引起的。以下是两种代码行为的详细分析：

### 第一段代码（出现滚动条）
```css
html, body, #page {
    width: 100%;
    height: 100%;
    background-color: #000;
}
```
**原因：**  
1. `<body>` 元素默认带有 `margin`（通常为 `8px`，由浏览器默认样式设置）。  
2. 当 `body` 的高度设为 `100%` 时，它等于视口高度。  
3. 但加上默认的 `margin` 后，实际占用空间 = `100% + 上下 margin`（例如 `100% + 16px`）。  
4. 这导致总高度**超出视口**，触发垂直滚动条。

---

### 第二段代码（无滚动条）
```css
html, #page {
    width: 100%;
    height: 100%;
    background-color: #000;
}
```
**原因：**  
1. 此代码**未设置 `body` 的高度**，因此 `body` 保持默认 `height: auto`（由内容决定高度）。  
2. `#page` 作为 `body` 的子元素，其 `height: 100%` 会继承自父级：  
   - 由于 `body` 未明确高度，`#page` 的 `100%` 实际**无效**（高度为 `0`）。  
3. 此时 `body` 的实际高度仅由默认 `margin` 撑开（约 `16px`），但**未超出视口**，故无滚动条。

---

### 关键差异
| 代码          | `body` 高度    | 效果                          |
|---------------|---------------|------------------------------|
| 第一段（含 `body`） | `100%` + 默认 `margin` | 超出视口 → **出现滚动条**      |
| 第二段（不含 `body`）| `auto`（高度为 `0` + `margin`） | 未超出视口 → **无滚动条**      |

---

### 解决方案（消除滚动条）
若需全屏且无滚动条，**重置 `body` 的默认 `margin`**：
```css
html, body, #page {
    width: 100%;
    height: 100%;
    margin: 0; /* 关键：清除默认 margin */
    background-color: #000;
}
```
这样 `body` 的 `100%` 高度会严格等于视口高度，无额外空间触发滚动条。

---

## Cover 图

![cover_1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/de/de66ff5950c97979c182dc639713ded60a3d895e2d271abba1b99ff8397e81b4.png)
