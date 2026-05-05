# URL 和 URI 的相同点与区别详解

作为一名 Web 开发者或技术爱好者，您可能经常听到 **URL** 和 **URI** 这两个术语。虽然它们常常被混用，但实际上它们有细微的区别。本文将深入探讨 URL 和 URI 的技术差异，浏览器中如何处理 URL，以及导航栏中编码与解码的作用。同时，还会介绍 JavaScript 中的 `encodeURIComponent` 和 `decodeURIComponent` 方法。

---

## **什么是 URI？**
**URI（统一资源标识符，Uniform Resource Identifier）** 是一种更广义的概念，用于标识互联网上的资源。它包括两种子类型：
1. **URL（统一资源定位符，Uniform Resource Locator）：** 指定资源的位置以及获取资源的方式。
   - 示例：`https://example.com/page.html`
2. **URN（统一资源名称，Uniform Resource Name）：** 仅指定资源的标识，而不提供其位置。
   - 示例：`urn:isbn:0451450523`

简单来说，每个 URL 都是 URI，但不是每个 URI 都是 URL。

---

## **什么是 URL？**
**URL（统一资源定位符，Uniform Resource Locator）** 是 URI 的一种特定类型，它提供了访问资源的完整参考信息，包括：
- **协议**（例如：`https` 或 `ftp`）
- **域名** 或 **IP 地址**（例如：`example.com`）
- 资源的 **路径**（例如：`/path/to/resource`）
- 可选组件，如 **查询字符串** 和 **片段标识符**

示例：
```
https://example.com:8080/path/to/page?query=123#section
```
该 URL 包含：
- 协议：`https`
- 域名：`example.com`
- 端口号：`8080`
- 路径：`/path/to/page`
- 查询字符串：`?query=123`
- 片段标识符：`#section`

---

## **URI 与 URL 的关键区别**
| 特性                  | URI                              | URL                              |
|-----------------------|----------------------------------|----------------------------------|
| **定义**             | 标识资源（位置或名称）            | 标识并定位资源                   |
| **是否包括 URL？**    | 是                               | 否                               |
| **示例**             | `urn:isbn:0451450523`, `https://example.com` | `https://example.com`, `ftp://files.com` |

---

## **浏览器中 `location.href` 的作用**
在浏览器中，`location.href` 表示当前页面的 **URL**，它总是包含完整且经过编码的 URL。

### **示例：**
对于以下网页：
```
https://example.com/search?q=hello%20world&lang=en
```
- **`location.href`** 的输出：
  ```
  https://example.com/search?q=hello%20world&lang=en
  ```
  该 URL 已被编码，以确保其符合互联网标准。
- 空格被编码为 `%20`。
- 特殊字符（如 `:` 和 `/`）保持原样。

---

## **为什么导航栏中的 URL 是编码的？**
浏览器对 URL 进行编码是为了确保：
1. **URL 的有效性：** 某些字符（如空格、中文字符和特殊符号）不能直接出现在 URL 中。编码会将它们转换为有效格式（百分号编码表示法）。
   - 示例：`你好`（中文“你好”）会被编码为 `%E4%BD%A0%E5%A5%BD`。

2. **网络传输的安全性：** 编码保护了 URL 在传输过程中的完整性，避免因保留字符（如 `&`）引起歧义。
   - 示例：`&` 被解释为查询参数的分隔符，因此在值中使用时必须编码。

---

## **JavaScript 中的编码与解码 URL 方法**
在编程中，JavaScript 提供了两个实用函数，用于处理编码的 URL：

### **1. `decodeURIComponent()`**
此函数用于解码编码的 URL 或其组件，使其变为可读形式。

#### 示例：
```javascript
const encodedUrl = "https://example.com/search?q=%E4%BD%A0%E5%A5%BD%E4%B8%96%E7%95%8C";
console.log(decodeURIComponent(encodedUrl));
// 输出：https://example.com/search?q=你好世界
```
可以用来解析查询字符串或片段标识符。

---

### **2. `encodeURIComponent()`**
此函数用于对 URI 组件进行编码，使其可以安全地包含在 URL 中。

#### 示例：
```javascript
const searchQuery = "你好世界";
const encodedQuery = encodeURIComponent(searchQuery);
console.log(encodedQuery);
// 输出：%E4%BD%A0%E5%A5%BD%E4%B8%96%E7%95%8C

const url = `https://example.com/search?q=${encodedQuery}`;
console.log(url);
// 输出：https://example.com/search?q=%E4%BD%A0%E5%A5%BD%E4%B8%96%E7%95%8C
```
此函数确保查询字符串被正确格式化以供传输。

---

## **`decodeURIComponent` 和 `decodeURI` 的比较**
| 函数                 | 用途                                                                                         |
|----------------------|--------------------------------------------------------------------------------------------|
| **`decodeURI`**      | 用于解码整个 URL，同时保留保留字符（如 `?`、`&` 和 `#`）。                                      |
| **`decodeURIComponent`** | 用于解码 URL 的特定组件（如查询字符串或片段）。                                                |

#### 示例：
```javascript
const encodedFullUrl = "https://example.com/search?q=%E4%BD%A0%E5%A5%BD&lang=en";
console.log(decodeURI(encodedFullUrl));
// 输出：https://example.com/search?q=你好&lang=en
```

---

## **总结**
总而言之：
- **URI** 用于标识资源，而 **URL** 用于指定资源的位置和访问方式。
- 浏览器中的 `location.href` 表示当前页面的 **完整编码 URL**。
- 浏览器对 URL 进行编码是为了保证其有效性和安全性。
- 使用 `decodeURIComponent` 和 `encodeURIComponent` 可在编程中方便地处理编码 URL。

理解 URL、URI 和编码之间的关系，是 Web 开发人员必备的知识之一。下次当您在导航栏看到类似 `%E4%BD%A0%E5%A5%BD` 的片段时，不要单纯认为是“乱码”了！

---
# English Ver.
# Understanding the Same and Distinctions Between URL and URI

If you’re a web developer or a tech enthusiast, you’ve likely encountered the terms **URL** and **URI**. While they are often used interchangeably, they have subtle differences that are essential to understand. In this blog, we’ll dive into the technical distinctions, how browsers handle URLs in practice, and the role of encoding and decoding in the navigation bar. We’ll also touch upon `encodeURIComponent` and `decodeURIComponent` for handling encoded strings in JavaScript.

---

## **What is a URI?**
A **URI (Uniform Resource Identifier)** is a broader concept used to identify a resource on the internet. It includes two subtypes:
1. **URL (Uniform Resource Locator):** Specifies the location of the resource and the mechanism to retrieve it.
   - Example: `https://example.com/page.html`
2. **URN (Uniform Resource Name):** Specifies the identity of the resource without implying its location.
   - Example: `urn:isbn:0451450523`

In simple terms, every URL is a URI, but not all URIs are URLs.

---

## **What is a URL?**
A **URL (Uniform Resource Locator)** is a specific type of URI that provides a complete reference to access a resource, including:
- The **protocol** (e.g., `https` or `ftp`)
- The **domain name** or **IP address** (e.g., `example.com`)
- The **path** to the resource (e.g., `/path/to/resource`)
- Optional components like the **query string** and **fragment**

Example:
```
https://example.com:8080/path/to/page?query=123#section
```
This URL contains:
- Protocol: `https`
- Domain: `example.com`
- Port: `8080`
- Path: `/path/to/page`
- Query String: `?query=123`
- Fragment: `#section`

---

## **Key Differences Between URI and URL**
| Feature                | URI                                | URL                                |
|------------------------|------------------------------------|------------------------------------|
| **Definition**         | Identifies a resource (location or name) | Identifies and locates a resource    |
| **Includes URL?**      | Yes                                | No                                 |
| **Examples**           | `urn:isbn:0451450523`, `https://example.com` | `https://example.com`, `ftp://files.com` |

---

## **The Role of `location.href` in Browsers**
In browsers, `location.href` represents the **URL** of the current page. It always includes the complete, encoded URL.

### **Example:**
For a webpage at:
```
https://example.com/search?q=hello%20world&lang=en
```
- **`location.href`** will output:
  ```
  https://example.com/search?q=hello%20world&lang=en
  ```
  The URL is encoded to ensure it complies with internet standards.
- Spaces are encoded as `%20`.
- Special characters like `:` and `/` are preserved.

---

## **Why URLs in the Navigation Bar Are Encoded**
Browsers encode URLs to ensure:
1. **URL Validity:** Certain characters like spaces, Chinese characters, and special symbols are not directly allowed in URLs. Encoding converts them into a valid format (percent-encoded representation).
   - Example: `你好` ("Hello" in Chinese) becomes `%E4%BD%A0%E5%A5%BD`.

2. **Network Safety:** Encoding protects URLs during transmission to avoid ambiguity caused by reserved characters.
   - Example: `&` is interpreted as a separator for query parameters, so it must be encoded in values.

---

## **Decoding and Encoding URLs in JavaScript**
To work with URLs programmatically, JavaScript provides two handy functions:

### **1. `decodeURIComponent()`**
This function decodes an encoded URL or its components, making it human-readable.

#### Example:
```javascript
const encodedUrl = "https://example.com/search?q=%E4%BD%A0%E5%A5%BD%E4%B8%96%E7%95%8C";
console.log(decodeURIComponent(encodedUrl));
// Output: https://example.com/search?q=你好世界
```

Use this to parse query strings or fragment identifiers.

---

### **2. `encodeURIComponent()`**
This function encodes a URI component to make it safe for inclusion in a URL.

#### Example:
```javascript
const searchQuery = "你好世界";
const encodedQuery = encodeURIComponent(searchQuery);
console.log(encodedQuery);
// Output: %E4%BD%A0%E5%A5%BD%E4%B8%96%E7%95%8C

const url = `https://example.com/search?q=${encodedQuery}`;
console.log(url);
// Output: https://example.com/search?q=%E4%BD%A0%E5%A5%BD%E4%B8%96%E7%95%8C
```

This ensures the query string is properly formatted for transmission.

---

## **Comparison of `decodeURIComponent` vs `decodeURI`**
| Function              | Use Case                                                                                     |
|-----------------------|----------------------------------------------------------------------------------------------|
| **`decodeURI`**       | Use to decode the entire URL while preserving reserved characters like `?`, `&`, and `#`.     |
| **`decodeURIComponent`** | Use to decode specific components of a URL, like query strings or fragments.                 |

#### Example:
```javascript
const encodedFullUrl = "https://example.com/search?q=%E4%BD%A0%E5%A5%BD&lang=en";
console.log(decodeURI(encodedFullUrl));
// Output: https://example.com/search?q=你好&lang=en
```

---

## **Wrapping Up**
In summary:
- A **URI** identifies a resource, while a **URL** specifies how to locate it.
- `location.href` in browsers represents the **full, encoded URL** of the current page.
- URLs are encoded in browsers for validity and safety.
- Use `decodeURIComponent` and `encodeURIComponent` to handle encoded URLs programmatically.

Understanding the interplay between URLs, URIs, and encoding is crucial for web developers, ensuring that resources are accessed and transmitted seamlessly on the web. Now, next time you see `%E4%BD%A0%E5%A5%BD`, you’ll know exactly what it means!

---

## Cover 图

![cover_1](URL_URI_今天你一定要弄懂！！！（一命速通url&uri encodeURIComponent&decodeURIComponent）.assets/c10c6d65b6e2f733.jpeg)
