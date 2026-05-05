**懒加载（Lazy Loading）** 是基于 `Intersection Observer` 实现的，它的核心思想是在**元素进入视口时才进行加载**，例如加载图片、视频资源或数据内容，以优化页面性能。

### 懒加载的实现思路
懒加载的主要目的是减少初始页面的加载量，将不在视口内的内容推迟加载，只有当用户滚动到这些内容附近时才触发加载行为。这样可以：
- 提高页面的加载速度；
- 降低资源消耗，特别是在移动设备上；
- 减少不必要的网络请求。

### 懒加载图片示例
以下代码展示了如何利用 `Intersection Observer` 实现图片懒加载。当图片元素即将进入视口时，才加载真正的图像资源。

#### HTML
```html
<!-- 图片元素，使用 data-src 代替 src -->
<img class="lazyload" data-src="actual-image.jpg" alt="Lazy Loaded Image">
```

#### JavaScript 实现懒加载
```javascript
// 获取所有懒加载图片
const lazyImages = document.querySelectorAll(".lazyload");

// 创建 Intersection Observer 实例
const lazyLoadObserver = new IntersectionObserver((entries, observer) => {
  entries.forEach(entry => {
    // 如果图片进入视口
    if (entry.isIntersecting) {
      const img = entry.target;
      // 设置图片 src 为 data-src 的内容，从而加载图片
      img.src = img.dataset.src;
      img.classList.remove("lazyload");
      // 停止观察已加载的图片
      observer.unobserve(img);
    }
  });
});

// 观察每个懒加载图片
lazyImages.forEach(img => lazyLoadObserver.observe(img));
```

### 代码解释
- **`data-src` 属性**：图片元素的 `src` 属性在页面加载时是空的。我们将真实的图片路径存放在 `data-src` 中。
- **Intersection Observer**：观察图片元素，判断它是否进入视口。
- **图片加载触发**：当图片进入视口时，将 `data-src` 值赋给 `src`，从而开始加载真实的图片资源。加载后，移除该图片的懒加载观察。

### 懒加载的应用场景
- **图片懒加载**：常用于电商、社交平台的图片墙。
- **视频懒加载**：在用户滚动到视频前，加载视频缩略图或占位符，当接近时再加载视频资源。
- **无限滚动**：在社交媒体或内容聚合平台上，只有在用户接近底部时才加载新内容。

懒加载在现代前端优化中至关重要，通过 `Intersection Observer` 简化了滚动检测逻辑，同时提升了页面性能，非常适合大量资源加载的场景。

---

## Cover 图

![cover_1](Intersection Observer 实现图片懒加载.assets/677bbb2f33d34cc2.png)
