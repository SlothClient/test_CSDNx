# 图片铺满容器的 CSS 实现方法

在网页设计中，让图片铺满容器有多种方式，具体取决于您的需求（是否裁剪、保持比例等）。以下是各种常见场景的解决方案：

## 1. 背景图片铺满容器（不裁剪）

```css
.container {
  width: 100%;
  height: 400px;
  background-image: url('your-image.jpg');
  background-size: contain; /* 完整显示图片，不裁剪 */
  background-repeat: no-repeat;
  background-position: center center;
}
```

## 2. 背景图片铺满容器（裁剪多余部分）

```css
.container {
  width: 100%;
  height: 400px;
  background-image: url('your-image.jpg');
  background-size: cover; /* 铺满容器，裁剪多余部分 */
  background-position: center center;
}
```

## 3. `<img>` 标签图片铺满容器（保持比例）

```css
.container {
  width: 100%;
  height: 400px;
  overflow: hidden; /* 隐藏超出部分 */
  display: flex;
  justify-content: center;
  align-items: center;
}

.container img {
  width: 100%;
  height: 100%;
  object-fit: cover; /* 或 contain，根据需求选择 */
}
```

## 4. 图片铺满并居中（多种方法）

### 方法1：使用 `object-fit`

```css
img {
  width: 100%;
  height: 100%;
  object-fit: cover; /* 填充容器，裁剪多余部分 */
  object-position: center; /* 图片居中 */
}
```

### 方法2：使用背景图

```css
div {
  background: url('image.jpg') center/cover no-repeat;
  width: 100%;
  height: 400px;
}
```

## 5. 响应式图片铺满

```css
.hero-image {
  width: 100%;
  height: 100vh; /* 视口高度 */
  position: relative;
  overflow: hidden;
}

.hero-image img {
  position: absolute;
  min-width: 100%;
  min-height: 100%;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  object-fit: cover;
}
```

## 6. 图片平铺（重复铺满）

```css
.pattern-bg {
  background-image: url('pattern.png');
  background-size: 200px 200px; /* 平铺单元尺寸 */
  background-repeat: repeat;
}
```

## 完整示例

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>图片铺满效果演示</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: #f5f7fa;
      color: #333;
      line-height: 1.6;
    }
    
    .container {
      max-width: 1200px;
      margin: 0 auto;
      padding: 20px;
    }
    
    header {
      text-align: center;
      padding: 40px 0;
    }
    
    h1 {
      font-size: 2.5rem;
      margin-bottom: 20px;
      color: #2c3e50;
    }
    
    .grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
      gap: 30px;
      margin-top: 40px;
    }
    
    .card {
      background: white;
      border-radius: 12px;
      overflow: hidden;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
      transition: transform 0.3s ease, box-shadow 0.3s ease;
    }
    
    .card:hover {
      transform: translateY(-10px);
      box-shadow: 0 15px 40px rgba(0, 0, 0, 0.15);
    }
    
    .image-container {
      height: 250px;
      position: relative;
    }
    
    .card-content {
      padding: 25px;
    }
    
    h3 {
      font-size: 1.5rem;
      margin-bottom: 15px;
      color: #2c3e50;
    }
    
    p {
      color: #7f8c8d;
      margin-bottom: 15px;
    }
    
    /* 方法1: object-fit: cover */
    .method-cover .image-container img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      object-position: center;
    }
    
    /* 方法2: background-size: cover */
    .method-bg-cover .image-container {
      background: url('https://picsum.photos/800/600?image=10') center/cover no-repeat;
    }
    
    /* 方法3: 使用绝对定位 */
    .method-absolute .image-container img {
      position: absolute;
      min-width: 100%;
      min-height: 100%;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      object-fit: cover;
    }
    
    /* 方法4: 使用 flexbox */
    .method-flex .image-container {
      display: flex;
      justify-content: center;
      align-items: center;
      overflow: hidden;
    }
    
    .method-flex .image-container img {
      min-width: 100%;
      min-height: 100%;
      flex-shrink: 0;
    }
    
    .method-label {
      position: absolute;
      top: 15px;
      left: 15px;
      background: rgba(44, 62, 80, 0.85);
      color: white;
      padding: 5px 10px;
      border-radius: 4px;
      font-size: 0.85rem;
      z-index: 10;
    }
    
    footer {
      text-align: center;
      padding: 40px 0;
      margin-top: 40px;
      color: #7f8c8d;
      border-top: 1px solid #eee;
    }
  </style>
</head>
<body>
  <div class="container">
    <header>
      <h1>图片铺满容器的 CSS 实现方法</h1>
      <p>四种不同的实现方式及其效果展示</p>
    </header>
    
    <div class="grid">
      <!-- 方法1: object-fit -->
      <div class="card method-cover">
        <div class="image-container">
          <span class="method-label">object-fit: cover</span>
          <img src="https://picsum.photos/800/600?image=20" alt="示例图片">
        </div>
        <div class="card-content">
          <h3>使用 object-fit</h3>
          <p>现代浏览器推荐方法，使用CSS的object-fit属性控制图片填充方式。</p>
          <p>兼容性：IE11不支持，现代浏览器完全支持。</p>
        </div>
      </div>
      
      <!-- 方法2: 背景图 -->
      <div class="card method-bg-cover">
        <div class="image-container">
          <span class="method-label">background-size: cover</span>
        </div>
        <div class="card-content">
          <h3>使用背景图</h3>
          <p>通过background-size: cover实现图片铺满效果。</p>
          <p>优点：兼容性好，所有浏览器都支持。</p>
          <p>缺点：图片无法被搜索引擎抓取，不利于SEO。</p>
        </div>
      </div>
      
      <!-- 方法3: 绝对定位 -->
      <div class="card method-absolute">
        <div class="image-container">
          <span class="method-label">绝对定位 + transform</span>
          <img src="https://picsum.photos/800/600?image=30" alt="示例图片">
        </div>
        <div class="card-content">
          <h3>绝对定位居中</h3>
          <p>使用绝对定位和transform居中图片。</p>
          <p>兼容性好，但需要更多代码实现。</p>
          <p>适合需要支持旧版浏览器的场景。</p>
        </div>
      </div>
      
      <!-- 方法4: flexbox -->
      <div class="card method-flex">
        <div class="image-container">
          <span class="method-label">flexbox 布局</span>
          <img src="https://picsum.photos/800/600?image=40" alt="示例图片">
        </div>
        <div class="card-content">
          <h3>使用 Flexbox</h3>
          <p>通过flex容器控制图片铺满。</p>
          <p>优点：结合其他flex属性可实现复杂布局。</p>
          <p>缺点：需要容器有固定高度。</p>
        </div>
      </div>
    </div>
  </div>
  
  <footer>
    <p>© 2023 图片铺满效果演示 | 四种常用CSS实现方法</p>
  </footer>
</body>
</html>
```

## 关键属性说明

1. **`object-fit`**：
   - `cover`：保持比例填充整个容器，可能裁剪部分内容
   - `contain`：保持比例完整显示图片，可能留白
   - `fill`：拉伸图片填充容器，可能变形

2. **`background-size`**：
   - `cover`：背景图覆盖整个容器
   - `contain`：背景图完整显示在容器内
   - 百分比/长度：自定义背景图尺寸

3. **`object-position` / `background-position`**：
   - 控制图片在容器中的位置（`top`, `bottom`, `left`, `right`, `center` 或具体值）

## 最佳实践建议

1. **现代浏览器**：优先使用 `object-fit: cover` + `object-position`
2. **兼容性要求**：使用背景图 `background-size: cover`
3. **响应式设计**：结合媒体查询调整不同尺寸下的显示效果
4. **性能优化**：使用适当尺寸的图片，避免加载过大图片

根据实际需求选择合适的方法，现代项目推荐使用 `object-fit`，它提供了最简洁高效的解决方案。
