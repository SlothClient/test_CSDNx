### JavaScript 的 `sort()` 方法与数字排序

在 JavaScript 中，`sort()` 方法是用于对数组进行排序的常用方法。然而，很多开发者在使用 `sort()` 方法时会遇到排序不符合预期的情况，特别是对于数字数组。本文将帮助你理解 `sort()` 方法的默认行为，以及如何正确地对数字数组进行排序。

#### 默认的 `sort()` 方法

**`sort()` 方法默认情况下会将数组元素转换为字符串，然后按字典顺序对它们进行排序。这意味着，如果你对数字数组进行排序，`sort()` 并不是按照数字的大小来排序，而是按照数字的字符编码来进行排序。**

例如，考虑数组 `[23, 12, 8, 13, 21, 9]`：

```javascript
let arr = [23, 12, 8, 13, 21, 9];
arr.sort();
console.log(arr);  // 输出: [ 12, 13, 21, 23, 8, 9 ]
```

#### 为什么会这样？

1. `23` 和 `12` 的字符比较是按字符串进行的：`'23'` 的字符 `'2'` 比 `'1'` 小，因此 `'23'` 排在 `'12'` 前面。
2. `23` 和 `8` 的字符比较是按字符串进行的：`'2'` 比 `'8'` 小，所以 `'23'` 排在 `'8'` 前面。
3. `13` 和 `21` 的字符比较是按字符串进行的：`'1'` 比 `'2'` 小，所以 `'13'` 排在 `'21'` 前面。

最终的输出是 `[ 12, 13, 21, 23, 8, 9 ]`，这并不是按数字大小排序的结果。

#### 如何正确排序数字数组？

为了正确排序数字数组，我们需要传入一个自定义的比较函数。比较函数告诉 `sort()` 方法如何对两个元素进行排序。

下面是如何正确对数字数组进行排序：

```javascript
let arr = [23, 12, 8, 13, 21, 9];

// 使用比较函数进行数字排序
arr.sort((a, b) => a - b);

console.log(arr);  // 输出: [ 8, 9, 12, 13, 21, 23 ]
```

#### 解释：
- `(a, b) => a - b` 作为比较函数，会确保按数字顺序进行排序。
  - 如果 `a - b` 为负数，`a` 会排在 `b` 前面。
  - 如果 `a - b` 为正数，`a` 会排在 `b` 后面。
  - 如果 `a - b` 为零，`a` 和 `b` 的相对位置不变。

这样，数组 `[23, 12, 8, 13, 21, 9]` 就会被正确地排序为 `[8, 9, 12, 13, 21, 23]`。

> 同理传入自定义函数(a, b) => b - a即可实现数字数组的逆序排列

#### 总结：

JavaScript 的 `sort()` 方法默认会按照字符串的字符顺序进行排序，这会导致数字数组的排序结果不符合预期。为了确保数字数组按大小正确排序，我们需要提供一个比较函数：`arr.sort((a, b) => a - b)`。这样就能确保数组按照数字大小正确排序。

---

#### American Version:

### Understanding JavaScript's `sort()` Method and How to Correctly Sort Numbers

In JavaScript, the `sort()` method is commonly used to sort arrays. However, many developers encounter unexpected sorting behavior when working with arrays of numbers. This blog will help you understand the default behavior of the `sort()` method and how to correctly sort an array of numbers.

#### Default Behavior of `sort()`

By default, the `sort()` method converts array elements into strings and sorts them lexicographically (dictionary order). This means that if you sort an array of numbers, `sort()` will not sort them by their numeric value, but by their string character codes.

For example, consider the array `[23, 12, 8, 13, 21, 9]`:

```javascript
let arr = [23, 12, 8, 13, 21, 9];
arr.sort();
console.log(arr);  // Output: [ 12, 13, 21, 23, 8, 9 ]
```

#### Why Does This Happen?

1. When comparing `23` and `12`, JavaScript compares the strings `'23'` and `'12'` lexicographically. Since `'2'` comes before `'1'`, `'23'` is placed before `'12'`.
2. When comparing `23` and `8`, JavaScript compares the strings `'23'` and `'8'`. Since `'2'` comes before `'8'`, `'23'` is placed before `'8'`.
3. When comparing `13` and `21`, JavaScript compares the strings `'13'` and `'21'`. Since `'1'` comes before `'2'`, `'13'` is placed before `'21'`.

As a result, the array is sorted as `[12, 13, 21, 23, 8, 9]`, which is not in numerical order.

#### How to Correctly Sort an Array of Numbers?

To correctly sort an array of numbers, we need to provide a custom compare function. The compare function tells the `sort()` method how to compare two elements.

Here’s how to correctly sort an array of numbers:

```javascript
let arr = [23, 12, 8, 13, 21, 9];

// Use a compare function for numeric sorting
arr.sort((a, b) => a - b);

console.log(arr);  // Output: [ 8, 9, 12, 13, 21, 23 ]
```

#### Explanation:
- The compare function `(a, b) => a - b` ensures that the sorting is done numerically:
  - If `a - b` is negative, `a` is placed before `b`.
  - If `a - b` is positive, `a` is placed after `b`.
  - If `a - b` is zero, the relative order of `a` and `b` remains unchanged.

Thus, the array `[23, 12, 8, 13, 21, 9]` is correctly sorted as `[8, 9, 12, 13, 21, 23]`.

#### Summary:

By default, JavaScript’s `sort()` method sorts elements as strings, which can lead to incorrect sorting for numerical arrays. To ensure correct numerical sorting, always provide a compare function to the `sort()` method, like this: `arr.sort((a, b) => a - b)`. This ensures that the numbers are compared numerically, resulting in the correct order.

---

## Cover 图

![cover_1](JS中的sort()数字排序不生效？--关于ascii那些事儿.assets/c0e9e2171c987703.png)
