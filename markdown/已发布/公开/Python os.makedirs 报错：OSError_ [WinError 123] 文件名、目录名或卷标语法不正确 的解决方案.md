# Python `os.makedirs` 报错：`OSError: [WinError 123] 文件名、目录名或卷标语法不正确` 的解决方案

在使用 `python` 的 `os.makedirs()` 创建文件夹时，你可能会遇到如下报错：

```python
File "D:\Python310\lib\os.py", line 225, in makedirs
    mkdir(name, mode)
OSError: [WinError 123] 文件名、目录名或卷标语法不正确。
```

---

## 一、问题原因

这是因为 **Windows 系统对文件夹（目录）命名有严格限制**。
如果文件名中包含以下非法字符，就会触发 `WinError 123` 错误：

```
\   /   :   *   ?   "   <   >   |
```

当你尝试用这些字符作为文件名时，Windows 会提示：

![非法文件名报错提示](Python os.makedirs 报错：OSError_ [WinError 123] 文件名、目录名或卷标语法不正确 的解决方案.assets/e2f6b7f709e5abc8.png)

---

## 二、解决思路

核心思路就是 **在调用 `os.makedirs()` 前，先检查文件名是否合法**。
如果包含非法字符，则提示用户重新输入。

---

## 三、示例代码

### 1. 定义检测函数

```python
# 易懂
def is_supported(dir_name):
    unsupported_char_list = ["\\", "/", ":", "*", "?", "\"", "<", ">", "|"]
    for char in unsupported_char_list:
        if char in dir_name:
            return 0
    return 1
```
```python
# 简洁
def is_supported(dir_name: str) -> bool:
    """
    判断目录名是否合法（Windows 下不允许含有特定字符）
    """
    unsupported_chars = ["\\", "/", ":", "*", "?", "\"", "<", ">", "|"]
    return not any(char in dir_name for char in unsupported_chars)
```

### 2. 在逻辑代码中使用

```python
title = "test|dir"   # 示例：非法目录名

if not is_supported(title):
    title = input(
        f'“{title}”不可作为文件名（请注意文件名不能包含下列任何字符 /:*?"<>| ），请输入合法名：'
    )

os.makedirs(title)
print(f"文件夹 {title} 创建成功！")
```

---

## 四、优化点

* **健壮性**：在实际项目中，可以将 `title` 清洗为合法文件名，而不仅仅是让用户重新输入。例如自动替换非法字符：
```python
import re

def sanitize_dir_name(dir_name: str) -> str:
    """
    自动替换非法字符为下划线 _
    """
    return re.sub(r'[\\/:"*?<>|]', "_", dir_name)
```

* **跨平台兼容**：Linux 和 MacOS 文件名限制少，但建议同样处理，避免迁移时出错。

---

## 五、总结

1. `OSError: [WinError 123]` 常见于 **非法目录名**。
2. Windows 下禁止的字符包括：`\ / : * ? " < > |`。
3. 建议在 `os.makedirs()` 前检测或清洗目录名，提升代码健壮性和用户体验。

---

## 六、思考题 🤔

* 如果目录名来自用户输入，如何设计更优雅的错误提示？
* 你更倾向于 **让用户重新输入**，还是 **程序自动替换非法字符**？

欢迎在评论区讨论！

---

## Cover 图

![cover_1](Python os.makedirs 报错：OSError_ [WinError 123] 文件名、目录名或卷标语法不正确 的解决方案.assets/723be1330a0742f4.png)
