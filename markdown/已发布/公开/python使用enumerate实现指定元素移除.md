### 题目：给定数组与指定元素，移除数组中所有指定元素并返回移除后数组长度

> 一道难度平易近人的算法题，存在多种解法，今天笔者要介绍的是使用enumerate获得所有目标元素索引并移除的方法。

### `enumerate()`方法
在 Python 中，enumerate() 是一个内置函数，用于在遍历序列（如列表、元组、字符串等）时同时获取元素的索引和值。它可以让代码更简洁且易读，避免了手动维护索引的繁琐操作。

#### 一、基本语法
```python
enumerate(iterable, start=0)
```
`iterable`: 需要遍历的序列（如列表、字符串等）。

`start` (可选): 索引的起始值，默认为 0。

返回值: 一个 `enumerate` 对象，生成由 (索引, 元素) 组成的元组。

#### 二、核心作用
在循环中直接获取元素的索引和值，无需单独维护计数器：

```python
fruits = ["apple", "banana", "cherry"]
# 传统写法（手动维护索引）
for i in range(len(fruits)):
    print(i, fruits[i])

# 使用 enumerate 的优雅写法
for index, fruit in enumerate(fruits):
    print(index, fruit)
```
### 题解
```python
import random

arr = eval(input())
target = random.choice(arr)
strippedArr = [v for k,v in enumerate(arr) if v != target]
print(target,strippedArr,len(strippedArr),sep='\t')
```

---

## Cover 图

![cover_1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/44/44657bb78a5ec2a638c89eef61537bbe7660fc6c878fe8210aeeb21e04053d33.webp)
