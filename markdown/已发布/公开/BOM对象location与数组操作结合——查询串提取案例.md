# BOM对象`location`与数组操作结合——查询串提取案例
## 前置知识
#### **1. Location 对象**

`Location` 对象是 JavaScript 提供的内置对象之一，它表示当前窗口或框架的 URL，并允许你通过它操作或获取 URL 的信息。可以通过 `window.location` 访问。

**主要属性**：
- `href`：完整的 URL 地址。
- `protocol`：协议部分（如 `http:` 或 `https:`）。
- `host`：域名和端口号（如 `example.com:8080`）。
- `hostname`：域名（不包括端口）。
- `port`：端口号。
- `pathname`：URL 的路径部分。
- `search`：查询字符串（以 `?` 开头）。
- `hash`：URL 的片段标识符（以 `#` 开头）。

**常用方法**：
- `assign(url)`：加载新页面到当前窗口。
- `replace(url)`：加载新页面但不保留历史记录。
- `reload()`：重新加载当前页面。

**示例**：
```javascript
console.log(window.location.href); // 当前页面的完整 URL
window.location.assign('https://example.com'); // 跳转到新的 URL
```

---

#### **2. 数组的可迭代方法**

数组的可迭代方法基于 JavaScript 的内置迭代器接口，使你可以对数组的每个元素进行操作。这些方法在日常开发中非常重要。重点掌握前三个方法即可。

**常用可迭代方法**：
1. **`forEach()`**  
   遍历数组中的每个元素，不返回结果。  
   ```javascript
   [1, 2, 3].forEach(num => console.log(num)); // 输出 1, 2, 3
   ```

2. **`map()`**  
   返回一个新数组，新数组中的每个元素是对原数组元素进行处理后的结果。  
   ```javascript
   let doubled = [1, 2, 3].map(num => num * 2);
   console.log(doubled); // 输出 [2, 4, 6]
   ```

3. **`filter()`**  
   返回一个新数组，包含所有通过筛选条件的元素。  
   ```javascript
   let evens = [1, 2, 3, 4].filter(num => num % 2 === 0);
   console.log(evens); // 输出 [2, 4]
   ```

4. **`reduce()`**  
   对数组中的所有元素进行累计计算，并返回一个最终结果。  
   ```javascript
   let sum = [1, 2, 3].reduce((acc, num) => acc + num, 0);
   console.log(sum); // 输出 6
   ```

5. **`some()` 和 `every()`**  
   - `some()`：检查数组中是否至少有一个元素满足条件。  
   - `every()`：检查数组中是否所有元素都满足条件。  
   ```javascript
   console.log([1, 2, 3].some(num => num > 2)); // 输出 true
   console.log([1, 2, 3].every(num => num > 2)); // 输出 false
   ```

6. **`find()` 和 `findIndex()`**  
   - `find()`：返回第一个满足条件的元素。  
   - `findIndex()`：返回第一个满足条件的元素的索引。  
   ```javascript
   let nums = [5, 10, 15];
   console.log(nums.find(num => num > 8)); // 输出 10
   console.log(nums.findIndex(num => num > 8)); // 输出 1
   ```

7. **`entries()`、`keys()` 和 `values()`**  
   - `entries()`：返回键值对的迭代器。  
   - `keys()`：返回索引的迭代器。  
   - `values()`：返回值的迭代器。  
   ```javascript
   let arr = ['a', 'b', 'c'];
   for (let [index, value] of arr.entries()) {
       console.log(index, value); // 输出 0 'a', 1 'b', 2 'c'
   }
   ```

8. **`flat()` 和 `flatMap()`**  
   - `flat()`：将多维数组拉平成一维。  
   - `flatMap()`：对数组每个元素执行映射后再扁平化。  
   ```javascript
   let nested = [1, [2, [3]]];
   console.log(nested.flat(2)); // 输出 [1, 2, 3]

   let words = ['hello world', 'foo bar'];
   console.log(words.flatMap(str => str.split(' '))); // 输出 ['hello', 'world', 'foo', 'bar']
   ```

---
## 查询串提取案例
**`location`获取**：浏览器搜索任意词条即可
```bash
> location.search
> '?pglt=297&q=%E4%BD%A0%E5%A5%BD%E4%B8%96%E7%95%8C&cvid=c83a5c858e364a06bbd131895d6bd7a4&gs_lcrp=EgRlZGdlKgYIABBFGDkyBggAEEUYOTIGCAEQABhAMgYIAhAAGEAyBggDEAAYQDIGCAQQABhAMgYIBRAAGEAyBggGEAAYQDIGCAcQABhAMgYICBAAGEDSAQg1NjQ0ajBqMagCALACAA&FORM=ANNTA1&PC=U531'
> searchStr = location.search.substring(1)
> 'pglt=297&q=%E4%BD%A0%E5%A5%BD%E4%B8%96%E7%95%8C&cvid=c83a5c858e364a06bbd131895d6bd7a4&gs_lcrp=EgRlZGdlKgYIABBFGDkyBggAEEUYOTIGCAEQABhAMgYIAhAAGEAyBggDEAAYQDIGCAQQABhAMgYIBRAAGEAyBggGEAAYQDIGCAcQABhAMgYICBAAGEDSAQg1NjQ0ajBqMagCALACAA&FORM=ANNTA1&PC=U531'
> searchArr = searchStr.split('&')
> (6) ['pglt=297', 'q=%E4%BD%A0%E5%A5%BD%E4%B8%96%E7%95%8C', 'cvid=c83a5c858e364a06bbd131895d6bd7a4', 'gs_lcrp=EgRlZGdlKgYIABBFGDkyBggAEEUYOTIGCAEQABhAMg…AAYQDIGCAcQABhAMgYICBAAGEDSAQg1NjQ0ajBqMagCALACAA', 'FORM=ANNTA1', 'PC=U531']
> const mapArr = (sep,index,arr) => {
    return arr.map((item) => {
        return item.split(sep)[index]
    })
}
> undefined
> searchArr = searchArr.map(item => {
    return decodeURIComponent(item)
})
> (6) ['pglt=297', 'q=你好世界', 'cvid=c83a5c858e364a06bbd131895d6bd7a4', 'gs_lcrp=EgRlZGdlKgYIABBFGDkyBggAEEUYOTIGCAEQABhAMg…AAYQDIGCAcQABhAMgYICBAAGEDSAQg1NjQ0ajBqMagCALACAA', 'FORM=ANNTA1', 'PC=U531']
> keyArr = mapArr('=',0,searchArr)
> (6) ['pglt', 'q', 'cvid', 'gs_lcrp', 'FORM', 'PC']
> valueArr = mapArr('=',1,searchArr)
> (6) ['297', '你好世界', 'c83a5c858e364a06bbd131895d6bd7a4', 'EgRlZGdlKgYIABBFGDkyBggAEEUYOTIGCAEQABhAMgYIAhAAGE…AAYQDIGCAcQABhAMgYICBAAGEDSAQg1NjQ0ajBqMagCALACAA', 'ANNTA1', 'U531']
> searchDict = {}
> {}
> keyArr.map((item,index) => {
    searchDict[item] = valueArr[index]
})
> (6) [undefined, undefined, undefined, undefined, undefined, undefined]
> searchDict
> {pglt: '297', q: '你好世界', cvid: 'c83a5c858e364a06bbd131895d6bd7a4', gs_lcrp: 'EgRlZGdlKgYIABBFGDkyBggAEEUYOTIGCAEQABhAMgYIAhAAGE…AAYQDIGCAcQABhAMgYICBAAGEDSAQg1NjQ0ajBqMagCALACAA', FORM: 'ANNTA1', …}
```
![console p1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/b9/b9004b6bac0d36164ff77671d8d8c0b6bdd714e677e9140c4ae56a6d4c8654ab.png)
![console p2](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/93/9325d8dc34433f878553cbcdb616f84bbcea4eb8e9593ec85ccd61a3edb80316.png)
## 需要注意

 - 关于数组的迭代方法基本都是传入一个`function`，参数列表为`item,index,array`，分别表示迭代对象的子元素、子元素索引以及迭代对象，三者按需引入，一般仅使用`item`
 - 注意`es6`语法，比如使用`(args) => {return;}`可简单表示一个匿名函数，当参数列表长度为1，即只有一个参数时可省略箭头前的括号

## 思考
- 倘若搜索的词条中包含了咱们用于拆分搜索字符串的特殊字符会怎样呢？例如**搜索词条“你好=世界&”**

---

## Cover 图

![cover_1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/b9/b9004b6bac0d36164ff77671d8d8c0b6bdd714e677e9140c4ae56a6d4c8654ab.png)
