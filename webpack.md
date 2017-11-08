一、 **打包去掉 console.log\(\)**

> > webpack.config.js 的 plugins 里面加上
> >
> > drop\_debugger: true,
> >
> > drop\_console: true

```
new webpack.optimize.UglifyJsPlugin({
  compress:{
    warnings: false,
    drop_debugger: true,
    drop_console: true
  }
})
```

二、

