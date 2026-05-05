### 题目：给定任意数组，输出最大与次大元素乘积

> 难度很平易近人的一道算法题，可以有很多思路，笔者今天给大家带来的是先对数组排序然后直接取尾端最大次大相乘的解决方法，而排序分为很多种，这里列出三种比较简单且高效的，分别是：`python`内置`sorted()`方法、快速排序和冒泡排序。**在下面的三种排序解决代码中笔者尽量多地使用了不同的重要知识点供大家参考学习。**

### `python`内置`sorted()`方法

> `Python` 内置的 `sorted()` 函数使用的排序算法是 `Timsort`（由 Tim Peters 在 2002 年为 Python 设计）。`Timsort` 是一种混合排序算法，结合了 归并排序（Merge Sort） 和 插入排序（Insertion Sort） 的优点，特别适合处理现实世界中的部分有序数据（如时间序列、日志文件等）。**时间复杂度：最好情况 `O(n)`（已有序）、平均和最坏情况 `O(n log n)`**

```python
import json

def multi(arr):
    if arr[-1] != arr[-2]:
        return arr[-1]*arr[-2]
    else:
        return multi(arr[:-1])

providedArr = input()
providedArr = json.loads(providedArr)
sortedArr = sorted(providedArr)

print(multi(sortedArr))


```
### 快速排序

> 简洁递归版（平均 `O(n log n)`，最坏 `O(n²)`）

```python
def quickSort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[len(arr) // 2]
    left = [i for i in arr if i < pivot]
    middle = [i for i in arr if i == pivot]
    right = [i for i in arr if i > pivot]
    return quickSort(left) + middle + quickSort(right)

providedArr = input()
providedArr = eval(providedArr)
sortedArr = quickSort(providedArr)
deDuplicativeArr = list(dict.fromkeys(sortedArr))

print(deDuplicativeArr[-1]*deDuplicativeArr[-2])
```
### 冒泡排序

> O(n^2^)

```python
def bubbleSort(arr):
    for i in range(len(arr)): # 数组有多长，外层循环就进行多少次，在大的向后冒泡的前提下保证大小顺序
        for j in range(len(arr) - 1 - i): # 两两相邻元素依次比较往后，保证最后的元素是最大的
            if arr[j] > arr[j+1]:
                arr[j],arr[j+1] = arr[j+1],arr[j]
    return arr
providedArr = input()
providedArr = eval(providedArr)
deDuplicatedArr = list(set(providedArr))
sortedArr = bubbleSort(deDuplicatedArr)

print(sortedArr[-1]*sortedArr[-2])
```

---

## Cover 图

![cover_1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/5e/5ee6d291f0c90c68fa452de3e4d61219272c65a0095c4506a283511ebc9e8cb2.webp)
