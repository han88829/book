一、打包去除所有console.log 和注释

```
    具体配置如下：
```

```
new webpack.optimize.UglifyJsPlugin({
   compress: {
       warnings: false,
       dead_code: true, // 去除不可达代码(不会运行的代码)
        pure_funcs: ['console.log'], // 配置发布时，不被打包的函数
        drop_debugger: true, // 发布时去除debugger
        // drop_console: true // 发布时去除console

   },
   output: {
       // 去掉注释内容
       comments: false,
   },

}),
```



