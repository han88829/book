### STEP 1

首先你需要在npm官网注册一个npm账号，官网地址是（[npm\*\*](https://link.jianshu.com?t=http://link.zhihu.com/?target=https%3A//www.npmjs.com/)）  
  


###  STEP2 建立一个待发布项目



一：执行命令 mkdir mynpm170328 && cd mynpm170328/



二：执行npm init根据提示会自动创建项目的package.json文件



三：在同目录下创建一个index.js文件，把下面代码粘贴进去保存

```
module.exports = 'This is my first NPM project';
```

### STEP3 添加npm用户

一：执行命令 npm adduser，出现提示 Username，Password，email的时候直接按回车键结束。成功后npm会把认证信息存储在~/.npmrc 文件中

二：执行命令 vim ~/.npmrc 查看（本地的authtoken跟你npm账号上的token是一样的）



### STEP 4 登陆刚刚建立的用户

一：执行命令 npm login,根据提示登录

二：执行命令 npm whoami 查看登录状态



### STEP 5 发布项目

一：执行命令 npm publish . 或者npm publish@1.0.0发布且添加版本号（发布的时候如果提示错误就改个名字，也许被人占用了）

二：发布成功后会有 + mynpm\_fuckyou\_haha@1.0.0的提示，后台也会有



### STEP 6 验证你刚发布的npm包

新创建一个目录执行 npm i mynpm\_fuckyou\_haha，安装成功会有下图提示（下面的warn提示是因为项目的package.json中信息没写全，可以写也可以不写）



