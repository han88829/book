使用 babel-plugin-proposal-decorators-legacy保存，提示更新后使用方式更改

报错信息：

```
The 'decorators' plugin requires a 'decoratorsBeforeExport' option, whose value must be a boolean. If you are migrating from Babylon/Babel 6 or want to use the old decorators proposal, you should use the 'decorators-legacy' plugin instead of 'decorators'
```

更换依赖为：

```
@babel/plugin-proposal-decorators
```

并在package.json中配置plugin：

```
"plugins": [
    [
        "@babel/plugin-proposal-decorators",
        {
            "legacy": true
        }
    ]
]
```



