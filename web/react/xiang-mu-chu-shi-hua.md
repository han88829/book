### 项目使用create-react-app创建

### 一、yarn eject \|\| npm run eject 显示配置文件。

### 二、安装所需依赖

1. babel-plugin-transform-decorators-legacy                使用装饰器
2. babel-plugin-transform-do-expressions                     转换表达式
3. babel-preset-es2015          babel-preset-react-app     babel-preset-stage-0      es6 es7 reactapp  新语法支持
4. node-sass-chokidar                    scss书写样式 具体配置参考    [https://book.h88829.top/web/css/scss.html](https://book.h88829.top/web/css/scss.html)





package.json配置文件参考

```
{
  "name": "react-router4-mobx",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "@antv/data-set": "^0.6.2",
    "@antv/g2": "^3.0.0",
    "antd": "^2.13.4",
    "autoprefixer": "7.1.2",
    "babel-core": "6.25.0",
    "babel-eslint": "7.2.3",
    "babel-jest": "20.0.3",
    "babel-loader": "7.1.1",
    "babel-plugin-import": "^1.6.0",
    "babel-plugin-transform-decorators-legacy": "^1.3.4",
    "babel-plugin-transform-do-expressions": "^6.22.0",
    "babel-preset-es2015": "^6.24.1",
    "babel-preset-react-app": "^3.0.3",
    "babel-preset-stage-0": "^6.24.1",
    "babel-runtime": "6.26.0",
    "bizcharts": "^3.0.1",
    "case-sensitive-paths-webpack-plugin": "2.1.1",
    "chalk": "1.1.3", 
    "css-loader": "0.28.4",
    "dotenv": "4.0.0",
    "echarts": "^3.7.2",
    "eslint": "4.4.1",
    "eslint-config-react-app": "^2.0.1",
    "eslint-loader": "1.9.0",
    "eslint-plugin-flowtype": "2.35.0",
    "eslint-plugin-import": "2.7.0",
    "eslint-plugin-jsx-a11y": "5.1.1",
    "eslint-plugin-react": "7.1.0",
    "extract-text-webpack-plugin": "3.0.0",
    "file-loader": "0.11.2",
    "fs-extra": "3.0.1",
    "html-webpack-plugin": "2.29.0",
    "jest": "20.0.4",
    "mobx": "^3.3.0",
    "mobx-react": "^4.3.3",
    "moment": "^2.19.1",
    "object-assign": "4.1.1",
    "postcss-flexbugs-fixes": "3.2.0",
    "postcss-loader": "2.0.6",
    "promise": "8.0.1",
    "react": "^16.0.0",
    "react-dev-utils": "^4.1.0",
    "react-dom": "^16.0.0",
    "react-lz-editor": "^0.11.9",
    "react-router-dom": "^4.2.2",
    "style-loader": "0.18.2",
    "sw-precache-webpack-plugin": "0.11.4",
    "url-loader": "0.5.9",
    "webpack": "3.5.1",
    "webpack-dev-server": "2.8.2",
    "webpack-manifest-plugin": "1.2.1",
    "whatwg-fetch": "2.0.3"
  },
  "scripts": {
    "start": "node scripts/start.js",
    "build": "node scripts/build.js",
    "test": "node scripts/test.js --env=jsdom"
  },
  "jest": {
    "collectCoverageFrom": [
      "src/**/*.{js,jsx}"
    ],
    "setupFiles": [
      "<rootDir>/config/polyfills.js"
    ],
    "testMatch": [
      "<rootDir>/src/**/__tests__/**/*.js?(x)",
      "<rootDir>/src/**/?(*.)(spec|test).js?(x)"
    ],
    "testEnvironment": "node",
    "testURL": "http://localhost",
    "transform": {
      "^.+\\.(js|jsx)$": "<rootDir>/node_modules/babel-jest",
      "^.+\\.css$": "<rootDir>/config/jest/cssTransform.js",
      "^(?!.*\\.(js|jsx|css|json)$)": "<rootDir>/config/jest/fileTransform.js"
    },
    "transformIgnorePatterns": [
      "[/\\\\]node_modules[/\\\\].+\\.(js|jsx)$"
    ],
    "moduleNameMapper": {
      "^react-native$": "react-native-web"
    },
    "moduleFileExtensions": [
      "web.js",
      "js",
      "json",
      "web.jsx",
      "jsx",
      "node"
    ]
  },
  "babel": {
    "presets": [
      "react-app",
      "es2015",
      "stage-0"
    ],
    "plugins": [
      "transform-decorators-legacy",
      "transform-do-expressions",
      [
        "import",
        [
          {
            "libraryName": "antd",
            "style": "css"
          }
        ]
      ]
    ]
  },
  "eslintConfig": {
    "extends": "react-app"
  },
  "proxy": "http://nestadmin.com"
}

```



