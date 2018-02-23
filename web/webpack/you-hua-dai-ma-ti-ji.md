# Tree Shaking

_tree shaking_

是一个术语，通常用于描述移除 JavaScript 上下文中的未引用代码\(dead-code\)。它依赖于 ES2015 模块系统中的

[静态结构特性](http://exploringjs.com/es6/ch_modules.html#static-module-structure)，例如[`import`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)和[`export`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)。这个术语和概念实际上是兴起于 ES2015 模块打包工具[rollup](https://github.com/rollup/rollup)。



```
npm install --save-dev uglifyjs-webpack-plugin
```

**webpack.config.js**



```
const path = require('path');
+ const UglifyJSPlugin = require('uglifyjs-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
- }
+ },
+ plugins: [
+   new UglifyJSPlugin()
+ ]
};
```



