> - URI(Uniform Resource Identifier)，通常包括URL(Uniform Resource Locator)&URN(Uniform Resource Name)
>- URL都是URI，但URI不一定是URL 
>- 参考[URL?URI??今天你一定要弄懂！！！（一命速通url&uri encodeURIComponent&decodeURIComponent）](https://blog.csdn.net/weixin_73334344/article/details/145380982?spm=1011.2415.3001.5331)

# 前端必备：深入理解 encodeURIComponent() 和 decodeURIComponent()

在Web开发中，处理URL和特殊字符是常见的需求。JavaScript提供了`encodeURIComponent()`和`decodeURIComponent()`这一对方法，专门用于安全地编码和解码URI组件。本文将结合实例详细讲解它们的用法、区别以及实际应用场景。

## 一、为什么需要URI编码？

### 1.1 URL中的特殊字符

URL中有些字符具有特殊含义，例如：
- `?` 表示查询字符串的开始
- `&` 用于分隔多个查询参数
- `=` 连接参数名和参数值
- `/` 表示路径分隔符
- `#` 表示片段标识符

当这些字符作为普通数据出现在URL中时，就需要进行编码，否则会导致URL解析错误。

### 1.2 非ASCII字符问题

URL只能使用ASCII字符集，当需要传输中文、日文等非ASCII字符时，必须先进行编码。

## 二、encodeURIComponent() 详解

### 2.1 基本用法

`encodeURIComponent()` 方法会对URI的组成部分进行编码，转义除字母、数字、`(`、`)`、`.`、`!`、`~`、`*`、`'`、`-`和`_`之外的所有字符。

```javascript
const param = '搜索?query=前端&sort=desc';
const encoded = encodeURIComponent(param);
console.log(encoded); 
// 输出："%E6%90%9C%E7%B4%A2%3Fquery%3D%E5%89%8D%E7%AB%AF%26sort%3Ddesc"
```

### 2.2 编码规则

1. 保留字符不编码：`A-Z a-z 0-9 - _ . ! ~ * ' ( )`
2. 空格编码为 `%20`
3. 其他字符转换为UTF-8编码，每个字节前加`%`

### 2.3 实际案例：构建安全的URL

```javascript
function buildSearchUrl(baseUrl, query, filters) {
  const params = new URLSearchParams();
  params.append('q', query);
  
  // 编码每个过滤条件
  Object.entries(filters).forEach(([key, value]) => {
    params.append(key, value);
  });
  
  return `${baseUrl}?${params.toString()}`;
}

const searchUrl = buildSearchUrl('https://example.com/search', '前端框架', {
  category: '技术',
  lang: 'zh-CN',
  price: '免费'
});

console.log(searchUrl);
// https://example.com/search?q=%E5%89%8D%E7%AB%AF%E6%A1%86%E6%9E%B6&category=%E6%8A%80%E6%9C%AF&lang=zh-CN&price=%E5%85%8D%E8%B4%B9
```

## 三、decodeURIComponent() 详解

### 3.1 基本用法

`decodeURIComponent()` 是 `encodeURIComponent()` 的逆操作，用于解码已编码的URI组件。

```javascript
const encoded = '%E6%90%9C%E7%B4%A2%3Fquery%3D%E5%89%8D%E7%AB%AF%26sort%3Ddesc';
const decoded = decodeURIComponent(encoded);
console.log(decoded); // 输出："搜索?query=前端&sort=desc"
```

### 3.2 错误处理

如果传入无效的编码序列，会抛出`URIError`异常，应该用try-catch处理：

```javascript
function safeDecode(encoded) {
  try {
    return decodeURIComponent(encoded);
  } catch (e) {
    console.error('解码失败:', e);
    return encoded; // 返回原值或默认值
  }
}
```

## 四、与encodeURI/decodeURI的区别

| 方法 | 编码范围 | 适用场景 |
|------|---------|---------|
| `encodeURIComponent()` | 编码除少量字符外的所有字符 | 单独参数值 |
| `encodeURI()` | 不编码`;/?:@&=+$,#`等保留字符 | 完整URL |
| `decodeURIComponent()` | 解码所有编码字符 | 单独参数值 |
| `decodeURI()` | 不解码保留字符的编码 | 完整URL |

**示例对比：**

```javascript
const url = 'https://example.com/path?name=张三&age=20';

// encodeURI 不会编码查询参数中的特殊字符
console.log(encodeURI(url));
// "https://example.com/path?name=%E5%BC%A0%E4%B8%89&age=20"

// encodeURIComponent 会编码所有特殊字符
console.log(encodeURIComponent(url));
// "https%3A%2F%2Fexample.com%2Fpath%3Fname%3D%E5%BC%A0%E4%B8%89%26age%3D20"
```

## 五、实际应用场景

### 5.1 处理查询参数

```javascript
// 构建带参数的URL
function buildUrl(base, params) {
  const queryString = Object.entries(params)
    .map(([key, value]) => 
      `${encodeURIComponent(key)}=${encodeURIComponent(value)}`
    )
    .join('&');
  return `${base}?${queryString}`;
}

// 解析URL参数
function parseQueryString(queryString) {
  return queryString.split('&').reduce((acc, pair) => {
    const [key, value] = pair.split('=');
    if (key) {
      acc[decodeURIComponent(key)] = decodeURIComponent(value || '');
    }
    return acc;
  }, {});
}
```

### 5.2 存储复杂数据

```javascript
// 将对象编码后存储在URL中
function saveStateToUrl(state) {
  const encoded = encodeURIComponent(JSON.stringify(state));
  window.location.hash = encoded;
}

// 从URL读取状态
function loadStateFromUrl() {
  try {
    return JSON.parse(decodeURIComponent(window.location.hash.slice(1)));
  } catch (e) {
    return null;
  }
}
```

### 5.3 处理Base64数据

```javascript
// 编码Base64字符串以便在URL中安全传输
function encodeBase64ForUrl(base64) {
  return encodeURIComponent(base64)
    .replace(/%([0-9A-F]{2})/g, (match, p1) => {
      return String.fromCharCode(parseInt(p1, 16));
    })
    .replace(/\+/g, '-')
    .replace(/\//g, '_')
    .replace(/=+$/, '');
}
```

## 六、常见问题与最佳实践

### 6.1 什么时候使用？

- **使用encodeURIComponent**：当需要编码单个URI组件时（如查询参数值）
- **使用encodeURI**：当需要编码完整URL但保留其有效性时

### 6.2 性能考虑

对于高频操作，可以缓存编码/解码结果：

```javascript
const encodingCache = new Map();

function cachedEncode(str) {
  if (!encodingCache.has(str)) {
    encodingCache.set(str, encodeURIComponent(str));
  }
  return encodingCache.get(str);
}
```

### 6.3 安全注意事项

1. 永远不要编码已经编码过的字符串
2. 解码用户输入前要进行验证
3. 考虑使用现代的`URL`和`URLSearchParams` API

## 七、现代替代方案：URLSearchParams

ES6引入了`URLSearchParams`接口，可以更方便地处理查询参数：

```javascript
const params = new URLSearchParams();
params.append('name', '张三');
params.append('city', '北京');

console.log(params.toString());
// "name=%E5%BC%A0%E4%B8%89&city=%E5%8C%97%E4%BA%AC"

// 直接用于URL
const url = new URL('https://example.com/search');
url.search = params;
console.log(url.href);
```

## 总结

`encodeURIComponent()`和`decodeURIComponent()`是处理URL编码的基础工具，理解它们的区别和正确使用场景对前端开发至关重要。随着现代Web API的发展，`URL`和`URLSearchParams`提供了更优雅的替代方案，但在许多场景下，直接使用编码/解码方法仍然是必要的。

