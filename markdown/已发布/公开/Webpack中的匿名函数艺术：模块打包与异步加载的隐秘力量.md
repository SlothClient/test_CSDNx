在Webpack中，匿名函数的应用广泛而精妙，尤其在模块打包和异步加载模块时显得尤为重要。本文将结合Webpack的代码示例，深入探讨匿名函数在Webpack中的几种巧妙用法。

### 1. 配置中的匿名函数变量赋值

在Webpack配置文件中，匿名函数可以赋值给变量，以便在配置的其他部分或插件中使用。例如，定义一个插件时，可以利用匿名函数来定制插件的行为：

```javascript
// webpack.config.js
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  // ...
  plugins: [
    new HtmlWebpackPlugin({
      template: './public/index.html',
    }),
  ],
};
```

在此例中，`HtmlWebpackPlugin` 构造函数中的匿名函数用于定义插件行为，该插件在构建过程中自动创建HTML文件，并将打包的JavaScript文件注入其中。

### ==2. 匿名函数自执行（重点）==

Webpack打包后的代码常以自执行匿名函数的形式出现，以保护内部变量和函数不被外部环境访问，避免全局命名空间污染。示例如下：

```javascript
(function(modules) {
  // ...
  return __webpack_require__(__webpack_require__.s = "./src/index.js");
})({
  // 模块定义
});
```

> 该自执行函数接收包含所有模块定义的对象作为参数，并在内部定义模块加载器（loader）`__webpack_require__`，负责加载和执行模块。

### 3. 匿名函数作为高阶函数

在Webpack中，匿名函数可作为高阶函数使用，即作为参数传递给其他函数，尤其在异步模块加载时非常有用。例如，使用`import()`语法异步加载模块：

```javascript
// ./src/index.js
import('./a').then(console.log);

// Webpack配置
module.exports = {
  entry: "./src/index",
  mode: "development",
  devtool: false,
  output: {
    filename: "[name].js",
    path: path.join(__dirname, "./dist"),
  },
};
```

Webpack打包后，生成的代码中包含异步加载函数`__webpack_require__.e`，它接受模块ID并返回一个Promise，模块加载完成后Promise解析。

### 4. 使用splitChunks分割代码

在Webpack中，`splitChunks`插件可以将代码分割成多个chunk，每个chunk作为一个单独的文件。这时，Webpack会生成一个包含所有chunks的匿名函数：

```javascript
// index.bundle.js
(function(modules) {
  // install a JSONP callback for chunk loading
  function webpackJsonpCallback(data) {
    // ...
  }
  var installedModules = {};
  // object to store loaded and loading chunks
  // undefined = chunk not loaded, null = chunk preloaded/prefetched
  // Promise = chunk loading, 0 = chunk loaded
  var installedChunks = {
    "runtime~main": 0
  };
  // ...
})();
```

在此例中，`webpackJsonpCallback`是一个匿名函数，负责处理chunk的加载和执行，而`installedChunks`对象跟踪chunks的加载状态。

通过这些示例，我们可以看到Webpack中匿名函数的多样化应用，它们在模块化、异步加载和代码分割等方面发挥着关键作用，提高了Webpack的灵活性和效率，同时保护了代码的封装性和安全性。

---

## Cover 图

![cover_1](https://pub-93427c0daded44f4ba9e82a03e799750.r2.dev/csdn-images/e2/e29aae5aea9b64e518fe3e98f76e3dcaee306336baf5b4b547e17d3ad41357db.jpeg)
