# 1.使用`concat`方法
```js
let arr1 = [1,3,4]
let arr2 = ['a','c','d']
let combinedArr = arr1.concat(arr2)
```
# 2.使用`...`扩展运算符
```js
let arr1 = [1,3,5]
let arr2 = ['a','c','e']
let combinedArr = [...arr1,...arr2]
```
# 3.使用`push`方法

> `push`方法与`pop`方法为栈方法，操作数组最后一位元素，会改变原数组

```js
let arr1 = [1,3,6]
let arr2 = ['a','c','f']
// 最简单常用的浅拷贝方式，不改变arr2
let combinedArr = JSON.parse(JSON.stringify(arr1))
combinedArr.push(...arr2)
```
# 演示
![demonstrate](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/f6/f65a16bbe761436d43867aa2a6df931ab8dfc5fc51eccfcef6880d3419506955.png)
# 想一想

>**A:**  注意上面笔者使用浏览器的开发者工具控制台演示三段代码的效果，同样的三个变量笔者声明了三次，为什么可以呢？按理说`var`才可以不是吗？在项目中`let`由于具有不可重复声明的特点推荐使用的不是吗？

>**Q:**  见[深度解析：JavaScript变量声明的演变与核心差异（var/let/隐式声明）](https://blog.csdn.net/weixin_73334344/article/details/146430773)四、浏览器控制台的let重复声明之谜

>**A:**  文中为什么`concat`使用后可以直接赋值给`combinedArr`而`push`必须先进行拷贝？关于哪些数组操作会改变原对象，哪些不会改变可参考

>**Q:**  见[JS中对数组的操作哪些会改变原数组哪些不会？今天你一定要记下！](https://editor.csdn.net/md/?articleId=145384216)

---

## Cover 图

![cover_1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/f6/f65a16bbe761436d43867aa2a6df931ab8dfc5fc51eccfcef6880d3419506955.png)
