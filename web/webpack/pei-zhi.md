一、打包去除所有console.log

webpack.config.js 的 plugins 里面加上

drop\_debugger: true,  
drop\_console: true

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



