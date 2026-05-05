# Python的`json.dumps`与JavaScript的`JSON.stringify`：深入比较与实践

在数据交换和网络通信中，JSON（JavaScript Object Notation）扮演着至关重要的角色。Python的`json.dumps`和JavaScript的`JSON.stringify`都是将数据结构序列化为JSON格式字符串的重要工具。本文将详细比较这两个函数的异同，并提供实际代码示例。

### 相同之处：核心功能与JSON标准

1. **序列化能力**：`json.dumps`和`JSON.stringify`均用于将复杂的数据结构（如字典、列表或对象和数组）序列化为JSON格式的字符串。
2. **遵循JSON标准**：两者输出的字符串都遵循JSON标准格式，包含键值对，其中键在Python中为字符串，而在JavaScript中可以是字符串或其他类型。
3. **处理基本数据类型**：两者都能正确处理布尔值（`true`/`false`）和`null`（Python中的`None`）。
4. **支持复杂结构**：均能处理数组（Python中的列表）和对象（Python中的字典）。
5. **不可序列化值的处理**：遇到不可序列化的值（如函数、类实例等）时，两者都会抛出异常。

### 差异之处：语言特性与实现细节

1. **语言依赖性**：`json.dumps`是Python语言的一部分，而`JSON.stringify`是JavaScript语言的一部分。
2. **默认格式化**：`json.dumps`默认会在键值对之间添加空格，而`JSON.stringify`输出紧凑的JSON字符串。
3. **美化输出**：`json.dumps`通过`indent`参数美化输出，`JSON.stringify`通过第二个参数控制美化。
4. **日期处理**：`JSON.stringify`将日期对象转换为字符串，而`json.dumps`需要自定义处理函数。
5. **错误信息**：两者抛出的异常信息格式可能不同。
6. **浮点数精度**：`JSON.stringify`可能在处理浮点数时有精度问题，`json.dumps`可以通过`float`参数自定义序列化方式。
7. **编码问题**：`json.dumps`输出UTF-8编码的字符串，而`JSON.stringify`输出的字符串在JavaScript中直接使用，无需考虑编码。
8. **键的顺序**：Python 3.7+中`dict`有序，`json.dumps`保持键的插入顺序；`JSON.stringify`在ES6中也保证键的插入顺序，但在早期版本中不确定。

### 代码示例

#### Python的`json.dumps`

```python
import json

data = {
    "name": "Xiaobai",
    "age": 30,
    "is_admin": True
}

json_str = json.dumps(data, indent=4)
print(json_str)
# 输出：
# {
#     "age": 30,
#     "is_admin": true,
#     "name": "Xiaobai"
# }
```

#### JavaScript的`JSON.stringify`

```javascript
var data = {
  name: "Xiaobai",
  age: 30,
  isAdmin: true
};

var jsonStr = JSON.stringify(data, null, 4);
console.log(jsonStr);
// 输出：
// {
//     "name": "Xiaobai",
//     "age": 30,
//     "isAdmin": true
// }
```

### 总结

尽管`json.dumps`和`JSON.stringify`在不同编程语言中有不同的默认行为和细节差异，但它们的主要功能是将数据结构序列化为JSON字符串。理解这些差异对于在不同语言之间进行有效的数据交换和处理至关重要。通过掌握这些工具，开发者可以更高效地处理跨语言的数据交互。

---

## Cover 图

![cover_1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/83/83a1cc470c4231c1a85107f5cc8a87183ff15e6a77d300fba528da862e70053a.png)
