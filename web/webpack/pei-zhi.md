一、打包去除所有console.log 和注释

        具体配置如下：

```
new webpack.optimize.UglifyJsPlugin({
   compress: {
       warnings: false,
       drop_debugger: true,
       //去除所有console.log
       drop_console: true,

   },
   output: {
       // 去掉注释内容
       comments: false,
   },

}),
```



