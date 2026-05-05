## **解决 Python 调用 `execjs` 时的编码问题：Subprocess 的正确打开方式**

### **前言**
在使用 Python 开发中，有时候我们需要调用 JavaScript 代码。为此，很多人会选择 `execjs` 库，它能够通过多种 JavaScript 运行时（如 Node.js）来执行 JS 脚本。然而，在 Windows 环境下，部分开发者可能会遇到莫名的错误，特别是当 Python 的 `subprocess` 模块涉及编码问题时。本文将详细解析问题根源，并提供简单高效的解决方案。

---

### **问题现象**
当我们在 Windows 环境中使用 `execjs` 并选择 Node.js 作为运行时时，可能会报出以下类似错误：

```
UnicodeDecodeError: 'gbk' codec can't decode byte 0x80 in position 0: illegal multibyte sequence
```

错误源头可能指向 Python 的 `subprocess.py` 文件。这个问题常发生在 Python 调用 Node.js 子进程时，Python 默认使用系统编码（如 GBK），而 Node.js 返回的数据可能是 UTF-8，导致编码解析失败。

---

### **问题分析**
#### 1. **Python 的 `subprocess` 模块**
`subprocess` 是 Python 中用于调用外部程序的模块。当 `execjs` 使用 Node.js 时，会通过 `subprocess.Popen` 来启动一个子进程并与之通信。默认情况下，`subprocess` 使用系统的编码方式解析输入输出流。

#### 2. **Windows 的编码问题**
在 Windows 上，Python 通常会使用 GBK 编码作为默认字符集，而 Node.js 默认使用 UTF-8 编码输出结果。如果两者不一致，就会导致编码错误，抛出 `UnicodeDecodeError`。

#### 3. **Node.js 的使用环境**
`execjs` 自动检测系统中可用的 JavaScript 运行时。若检测到 Node.js，就会使用它来执行 JavaScript 脚本。因此，Node.js 的输出和 Python 的输入编码冲突是问题的根本原因。

---

### **解决方案**
为了避免编码问题，我们需要显式地为 `subprocess.Popen` 设置编码为 `UTF-8`。以下是完整的解决代码：

```python
# Windows 环境下解决 subprocess 编码问题
import subprocess
from functools import partial

# 修改 subprocess.Popen，设置默认编码为 UTF-8
subprocess.Popen = partial(subprocess.Popen, encoding="UTF-8")

# 确保 execjs 在这个修改之后引入
import execjs

# 测试代码
js_code = """
function add(a, b) {
    return a + b;
}
"""
ctx = execjs.compile(js_code)
result = ctx.call("add", 1, 2)
print("执行结果:", result)
```

---

### **代码详解**
#### 1. **`partial` 的作用**
我们使用 `functools.partial` 为 `subprocess.Popen` 设置一个默认参数 `encoding="UTF-8"`。  
`partial` 的作用是生成一个新函数，它在原函数基础上固定了一些参数。因此，这段代码将所有使用 `subprocess.Popen` 的地方强制设置为 `UTF-8`。

#### 2. **顺序问题**
需要特别注意的是，**必须在导入 `execjs` 之前修改 `subprocess.Popen`**，否则 `execjs` 会调用原始的 `subprocess.Popen`，仍然可能发生编码问题。

#### 3. **如何验证**
运行上述代码后，你应该能正确执行 `execjs` 脚本，而不会遇到编码问题。如果你在代码中直接输出中文字符，也能正常显示。

---

### **总结**
#### **问题根本原因**
- Python 的 `subprocess` 模块在处理子进程的输入输出流时默认使用系统编码（如 GBK）。
- Node.js 的默认输出编码为 UTF-8，导致编码冲突。

#### **解决办法**
通过以下三步解决：
1. 使用 `functools.partial` 修改 `subprocess.Popen`，为其强制指定 `encoding="UTF-8"`。
2. 确保修改 `subprocess.Popen` 的代码在 `import execjs` 之前执行。
3. 确认系统中已正确安装 Node.js，供 `execjs` 调用。

---

### **扩展阅读**
- [Python 文档：subprocess](https://docs.python.org/3/library/subprocess.html)
- [execjs 官方文档](https://github.com/doloopwhile/PyExecJS)
- [Node.js 官网](https://nodejs.org/)

---

### **结语**
Python 是一门功能强大的语言，但在与其他语言或工具（如 Node.js）交互时，往往会遇到编码或环境问题。通过本文的解决方案，相信你能更顺畅地使用 `execjs` 和 Node.js 执行 JavaScript 代码，为项目开发提供更大的便利。

希望本文对你有所帮助！如果你在开发中还有其他疑问，欢迎留言交流 👇！

---

## Cover 图

![cover_1](解决 Python 调用 execjs 时的编码问题：Subprocess 的正确打开方式.assets/37364cd929530784.jpeg)
