### 深入理解 Axios 拦截器的执行链机制  

在现代前端开发中，Axios 是最流行的 HTTP 请求库之一，而 **拦截器**（Interceptor）功能是其核心特性之一。通过拦截器，我们可以在请求发送前或响应返回后进行灵活的预处理或后处理。然而，很多人并不了解拦截器在 Axios 内部是如何构建和执行的。本文将深入剖析 Axios 拦截器的 **执行链机制**，帮助更多开发者掌握这一重要知识。

---

## 1. Axios 拦截器的结构  

拦截器分为两类：  

- **请求拦截器**（Request Interceptors）：在请求被发送前执行，可以用来修改请求配置（如添加认证 Token）。
- **响应拦截器**（Response Interceptors）：在请求返回后执行，可以用来处理响应数据或错误（如统一错误处理）。

### **拦截器存储结构**
Axios 使用一个 `InterceptorManager` 类来管理拦截器。每个拦截器由两个回调函数组成：`fulfilled`（成功时执行）和 `rejected`（失败时执行）。这些拦截器被保存在 `handlers` 数组中。

```javascript
class InterceptorManager {
  constructor() {
    this.handlers = []; // 存储拦截器的数组
  }

  use(fulfilled, rejected) {
    this.handlers.push({ fulfilled, rejected });
    return this.handlers.length - 1; // 返回拦截器索引，用于删除
  }

  eject(id) {
    if (this.handlers[id]) {
      this.handlers[id] = null; // 通过索引禁用拦截器
    }
  }
}
```

---

## 2. Axios 拦截器的执行链  

Axios 的执行链是一个由 **Promise 串联的动态数组**，其结构如下：  

### **拦截器执行链的三部分**
1. **左侧：请求拦截器（Request Interceptors）**
   - 顺序：按照注册顺序的 **逆序** 执行（后注册的先执行）。
   - 作用：在请求被发送之前对配置对象进行处理。

2. **中间：核心逻辑（dispatchRequest）**
   - 负责实际的 HTTP 请求。
   - 分隔了请求拦截器和响应拦截器。

3. **右侧：响应拦截器（Response Interceptors）**
   - 顺序：按照注册顺序的 **正序** 执行（先注册的先执行）。
   - 作用：在请求返回后对响应数据进行处理。

### **执行链的构建代码**
以下是 Axios 拦截器执行链的源码片段：

```javascript
Axios.prototype.request = function request(config) {
  const chain = [dispatchRequest, undefined]; // 中间逻辑初始化
  let promise = Promise.resolve(config); // 初始化 Promise 链

  // 添加请求拦截器（从前插入，后注册的先执行）
  this.interceptors.request.forEach((interceptor) => {
    chain.unshift(interceptor.fulfilled, interceptor.rejected || undefined);
  });

  // 添加响应拦截器（从后追加，先注册的先执行）
  this.interceptors.response.forEach((interceptor) => {
    chain.push(interceptor.fulfilled, interceptor.rejected || undefined);
  });

  // 执行拦截器链
  while (chain.length) {
    promise = promise.then(chain.shift(), chain.shift());
  }

  return promise;
};
```

---

## 3. 请求拦截器与响应拦截器的执行顺序  

通过上面的代码可以看出，Axios 是如何动态构建拦截器链的。下面以一个例子说明拦截器的执行顺序：

### **示例代码**
```javascript
// 注册两个请求拦截器
axios.interceptors.request.use((config) => {
  console.log('请求拦截器 1');
  return config;
});

axios.interceptors.request.use((config) => {
  console.log('请求拦截器 2');
  return config;
});

// 注册两个响应拦截器
axios.interceptors.response.use((response) => {
  console.log('响应拦截器 1');
  return response;
});

axios.interceptors.response.use((response) => {
  console.log('响应拦截器 2');
  return response;
});

// 发起请求
axios.get('/example');
```

### **执行链的构建结果**
```text
[请求拦截器 2, 请求拦截器 1, dispatchRequest, undefined, 响应拦截器 1, 响应拦截器 2]
```

### **执行顺序**
1. 请求拦截器（从左到中间）：
   - 请求拦截器 2
   - 请求拦截器 1
2. 核心逻辑（中间部分）：
   - `dispatchRequest` 执行实际的 HTTP 请求。
3. 响应拦截器（从中间到右）：
   - 响应拦截器 1
   - 响应拦截器 2

**最终输出：**
```text
请求拦截器 2
请求拦截器 1
响应拦截器 1
响应拦截器 2
```

---

## 4. 拦截器中的 `unshift` 与 `push`  

在 Axios 的源码中，拦截器的添加方式直接影响了执行顺序：  

1. **请求拦截器**  
   使用 `unshift` 添加到数组头部，确保后注册的拦截器优先执行。  
   ```javascript
   chain.unshift(interceptor.fulfilled, interceptor.rejected);
   ```

2. **响应拦截器**  
   使用 `push` 添加到数组尾部，确保先注册的拦截器优先执行。  
   ```javascript
   chain.push(interceptor.fulfilled, interceptor.rejected);
   ```

---

## 5. 为什么要分隔拦截器？  

在 Axios 中，`dispatchRequest` 既是 HTTP 请求的核心逻辑，也是请求和响应拦截器的天然分隔点。分隔的意义如下：  

1. **分工明确**  
   - 请求拦截器仅处理请求数据（如添加认证信息、修改 URL）。
   - 响应拦截器仅处理返回数据（如解析 JSON、统一错误处理）。

2. **Promise 链式调用的完整性**  
   - Axios 通过 `undefined` 占位确保 Promise 链不会断裂，即使某个拦截器没有 `rejected` 方法。

---

## 6. 总结  

Axios 的拦截器机制为开发者提供了强大的灵活性和扩展性，其核心逻辑包括：  

- 请求拦截器通过 `unshift` 插入链头，保证后注册的拦截器优先执行。
- 响应拦截器通过 `push` 插入链尾，保证先注册的拦截器优先执行。
- 中间逻辑 `dispatchRequest` 是请求和响应的分界点。
- 使用动态链式调用确保拦截器的执行顺序和逻辑一致性。

掌握拦截器的内部实现，有助于我们更高效地调试和优化代码。希望本文能够帮助更多开发者深入理解 Axios 的核心原理，进而提升开发效率！  

---

如果您觉得这篇文章对您有所帮助，欢迎点赞、转发，或留下评论一起讨论！

---

## Cover 图

![cover_1](深入理解 Axios 拦截器的执行链机制.assets/c96f92885cf45520.webp)
