### **JavaScript 数组方法：变更原数组与不变更原数组的区别**

在 JavaScript 中，数组是非常常见且重要的数据结构。作为开发者，我们常常需要使用数组方法来处理数组数据。但是，数组的不同方法会以不同的方式影响原数组，它们可以分为**变更原数组**的方法和**不变更原数组**的方法。本文将详细探讨 JavaScript 中常见的数组方法，分析它们是如何工作的，并重点讨论哪些方法会修改原数组，哪些方法不会。
## **一、变更原数组的方法**

这些方法会直接对数组本身进行修改，改变其元素、长度或结构。

### **1. `push()`**
`push()` 方法将一个或多个元素添加到数组的末尾，并返回数组的新长度。

- **示例：**
  ```javascript
  let arr = [1, 2, 3];
  arr.push(4);
  console.log(arr); // [1, 2, 3, 4]
  ```

- **原理**：
  - `push()` 会直接修改原数组。在其内部，它会将新的元素添加到数组的末尾，并更新数组的长度。其基本实现如下：
  ```javascript
  Array.prototype.push = function (...args) {
    let len = this.length;
    for (let i = 0; i < args.length; i++) {
      this[len + i] = args[i];
    }
    return this.length;
  };
  ```

### **2. `pop()`**
`pop()` 方法删除数组的最后一个元素，并返回被删除的元素。

- **示例：**
  ```javascript
  let arr = [1, 2, 3];
  let removedElement = arr.pop();
  console.log(arr); // [1, 2]
  console.log(removedElement); // 3
  ```

- **原理**：
  - `pop()` 会修改数组并返回最后一个元素。其实现方式：
  ```javascript
  Array.prototype.pop = function () {
    let len = this.length;
    if (len === 0) return undefined;
    let removedElement = this[len - 1];
    this.length = len - 1;
    return removedElement;
  };
  ```

### **3. `shift()`**
`shift()` 方法从数组的开头删除一个元素，并返回该元素。

- **示例：**
  ```javascript
  let arr = [1, 2, 3];
  let removedElement = arr.shift();
  console.log(arr); // [2, 3]
  console.log(removedElement); // 1
  ```

- **原理**：
  - `shift()` 删除数组的第一个元素，并且其他元素会向左移动。其实现方式：
  ```javascript
  Array.prototype.shift = function () {
    let len = this.length;
    if (len === 0) return undefined;
    let removedElement = this[0];
    for (let i = 0; i < len - 1; i++) {
      this[i] = this[i + 1];
    }
    this.length = len - 1;
    return removedElement;
  };
  ```

### **4. `unshift()`**
`unshift()` 方法将一个或多个元素添加到数组的开头，并返回新数组的长度。

- **示例：**
  ```javascript
  let arr = [1, 2, 3];
  arr.unshift(0);
  console.log(arr); // [0, 1, 2, 3]
  ```

- **原理**：
  - `unshift()` 会将元素插入到数组的开头，并移动现有元素的位置。其实现代码：
  ```javascript
  Array.prototype.unshift = function (...args) {
    let len = this.length;
    for (let i = len - 1; i >= 0; i--) {
      this[i + args.length] = this[i];
    }
    for (let i = 0; i < args.length; i++) {
      this[i] = args[i];
    }
    return this.length;
  };
  ```

### **5. `reverse()`**
`reverse()` 方法将数组中的元素顺序颠倒，并直接修改原数组。

- **示例：**
  ```javascript
  let arr = [1, 2, 3];
  arr.reverse();
  console.log(arr); // [3, 2, 1]
  ```

- **原理**：
  - `reverse()` 直接改变原数组，颠倒数组元素顺序。其内部实现代码：
  ```javascript
  Array.prototype.reverse = function () {
    let len = this.length;
    for (let i = 0; i < Math.floor(len / 2); i++) {
      let temp = this[i];
      this[i] = this[len - 1 - i];
      this[len - 1 - i] = temp;
    }
    return this;
  };
  ```

### **6. `sort()`**
`sort()` 方法对数组中的元素进行排序，并返回排序后的数组。此方法会修改原数组。

- **示例：**
  ```javascript
  let arr = [23, 12, 8, 13, 21, 9];
  arr.sort();
  console.log(arr); // [12, 13, 21, 23, 8, 9] (默认按字符串排序)
  ```

- **原理**：
  - `sort()` 默认按照字符串排序，如果需要按数值排序，可以提供一个比较函数。其基本实现代码如下：
  ```javascript
  Array.prototype.sort = function (compareFunction) {
    let len = this.length;
    for (let i = 0; i < len - 1; i++) {
      for (let j = 0; j < len - 1 - i; j++) {
        if (compareFunction(this[j], this[j + 1]) > 0) {
          let temp = this[j];
          this[j] = this[j + 1];
          this[j + 1] = temp;
        }
      }
    }
    return this;
  };
  ```

### **7. `splice()`**
`splice()` 方法可以从数组中添加或删除元素，并直接修改原数组。

- **示例：**
  ```javascript
  let arr = [1, 2, 3, 4, 5];
  arr.splice(2, 1); // 删除索引2的元素
  console.log(arr); // [1, 2, 4, 5]
  ```

- **原理**：
  - `splice()` 通过删除和插入来修改原数组。其实现代码：
  ```javascript
  Array.prototype.splice = function (start, deleteCount, ...items) {
    let removedItems = [];
    for (let i = start; i < start + deleteCount; i++) {
      removedItems.push(this[i]);
    }
    let len = this.length;
    for (let i = len - 1; i >= start + deleteCount; i--) {
      this[i + items.length] = this[i];
    }
    for (let i = 0; i < items.length; i++) {
      this[start + i] = items[i];
    }
    this.length -= deleteCount;
    return removedItems;
  };
  ```

---


## **二、不改变原数组的方法**

这些方法会返回一个新数组，而不会修改原数组的内容。了解这些方法的实现原理，有助于我们更好地理解它们是如何工作的。

### **1. `concat()`**
`concat()` 方法用于合并多个数组或值，并返回一个新的数组。

- **示例：**
  ```javascript
  let arr1 = [1, 2, 3];
  let arr2 = [4, 5, 6];
  let newArr = arr1.concat(arr2);
  console.log(newArr); // [1, 2, 3, 4, 5, 6]
  console.log(arr1); // [1, 2, 3]
  console.log(arr2); // [4, 5, 6]
  ```

- **原理**：
  - `concat()` 会返回一个新的数组，原数组不会改变。实现代码：
  ```javascript
  Array.prototype.concat = function (...args) {
    let result = [...this];
    for (let i = 0; i < args.length; i++) {
      let currentArray = args[i];
      result.push(...currentArray);
    }
    return result;
  };
  ```

### **2. `slice()`**
`slice()` 方法返回数组的一个浅拷贝，并不改变原数组。

- **示例：**
  ```javascript
  let arr = [1, 2, 3, 4, 5];
  let newArr = arr.slice(1, 4);
  console.log(newArr); // [2, 3, 4]
  console.log(arr); // [1, 2, 3, 4, 5]
  ```

- **原理**：
  - `slice()` 会返回数组的一部分副本，不会改变原数组。其实现方式如下：
  ```javascript
  Array.prototype.slice = function (start, end) {
    let result = [];
    for (let i = start || 0; i < (end || this.length); i++) {
      result.push(this[i]);
    }
    return result;
  };
  ```

### **3. `map()`**
`map()` 方法通过提供的函数对数组中的每个元素进行处理，返回一个新数组，不会修改原数组。

- **示例：**
  ```javascript
  let arr = [1, 2, 3];
  let newArr = arr.map(item => item * 2);
  console.log(newArr); // [2, 4, 6]
  console.log(arr); // [1, 2, 3]
  ```

- **原理**：
  - `map()` 方法会根据提供的回调函数生成一个新数组，原数组不受影响。其实现代码：
  ```javascript
  Array.prototype.map = function (callback) {
    let result = [];
    for (let i = 0; i < this.length; i++) {
      result.push(callback(this[i], i, this));
    }
    return result;
  };
  ```

### **4. `filter()`**
`filter()` 方法基于条件返回数组中符合条件的元素组成的新数组，原数组不改变。

- **示例：**
  ```javascript
  let arr = [1, 2, 3, 4, 5];
  let newArr = arr.filter(item => item > 3);
  console.log(newArr); // [4, 5]
  console.log(arr); // [1, 2, 3, 4, 5]
  ```

- **原理**：
  - `filter()` 会根据回调函数判断每个元素是否满足条件，返回一个新数组，原数组不变。其实现代码：
  ```javascript
  Array.prototype.filter = function (callback) {
    let result = [];
    for (let i = 0; i < this.length; i++) {
      if (callback(this[i], i, this)) {
        result.push(this[i]);
      }
    }
    return result;
  };
  ```

### **5. `join()`**
`join()` 方法将数组的所有元素连接成一个字符串，并返回该字符串。

- **示例：**
  ```javascript
  let arr = [1, 2, 3];
  let result = arr.join("-");
  console.log(result); // "1-2-3"
  console.log(arr); // [1, 2, 3]
  ```

- **原理**：
  - `join()` 返回一个由数组元素连接而成的字符串，原数组没有改变。其实现方式如下：
  ```javascript
  Array.prototype.join = function (separator = ',') {
    let result = '';
    for (let i = 0; i < this.length; i++) {
      if (i > 0) result += separator;
      result += this[i];
    }
    return result;
  };
  ```

### **6. `some()`**
`some()` 方法检查数组中是否有至少一个元素符合条件，返回布尔值，原数组不变。

- **示例：**
  ```javascript
  let arr = [1, 2, 3, 4];
  let result = arr.some(item => item > 3);
  console.log(result); // true
  console.log(arr); // [1, 2, 3, 4]
  ```

- **原理**：
  - `some()` 检查数组中是否有符合条件的元素，如果有则返回 `true`，否则返回 `false`。原数组不受影响，代码实现如下：
  ```javascript
  Array.prototype.some = function (callback) {
    for (let i = 0; i < this.length; i++) {
      if (callback(this[i], i, this)) {
        return true;
      }
    }
    return false;
  };
  ```

### **7. `every()`**
`every()` 方法检查数组中的每个元素是否都满足条件，返回布尔值，原数组不变。

- **示例：**
  ```javascript
  let arr = [1, 2, 3, 4];
  let result = arr.every(item => item > 0);
  console.log(result); // true
  console.log(arr); // [1, 2, 3, 4]
  ```

- **原理**：
  - `every()` 会判断数组中的所有元素是否都满足给定条件，如果都满足返回 `true`，否则返回 `false`。其实现代码如下：
  ```javascript
  Array.prototype.every = function (callback) {
    for (let i = 0; i < this.length; i++) {
      if (!callback(this[i], i, this)) {
        return false;
      }
    }
    return true;
  };
  ```

---

至此，我们已经完成了 **数组操作以及操作是否改变数组本身的源码分析** 的完整解析。通过源码实现的方式，我们了解了不同数组方法是如何工作的，哪些方法会修改原数组，哪些方法不会。掌握这些方法的内部原理可以帮助我们在开发中更好地选择使用合适的数组操作方法。

---

## Cover 图

![cover_1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/65/65b9ad91b449873f6796a8e18aa2455e5ff6df225a8aed41806a377e84eab56d.png)
