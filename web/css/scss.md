### 在react中使用scss;

react在版本升级到16.0之后就不在内置支持scss了，要根据官方文档进行配置；

1、先安装node-sass-chokidar

```
npm install --save node-sass-chokidar

yarn add node-sass-chokidar
```

2、安装完之后要在package.json进行进行配置

```
"build-css": "node-sass-chokidar src/ -o src/",//打包之前把scss转换成css
"watch-css": "npm run build-css && node-sass-chokidar src/ -o src/ --watch --recursive",//npm start 之前解析所有的scss
```

3、把scss解析命令集成到npm start    npm run build中

```
先安装npm-run-all

npm install --save npm-run-all

yarn add npm-run-all


安装好之后在package.json中进行配置
  "scripts": {
     "build-css": "node-sass-chokidar src/ -o src/",
     "watch-css": "npm run build-css && node-sass-chokidar src/ -o src/ --watch --recursive",
-    "start": "react-scripts start",
-    "build": "react-scripts build",
+    "start-js": "react-scripts start",
+    "start": "npm-run-all -p watch-css start-js",
+    "build-js": "react-scripts build",
+    "build": "npm-run-all build-css build-js",
     "test": "react-scripts test --env=jsdom",
     "eject": "react-scripts eject"
   }
```

直接运行项目就可以了

npm run build 之后scss解析的css文件会很多，可以在.gitignore文件中忽视掉这些文件

```
src/**/*.css               //屏蔽所有的css文件
```



