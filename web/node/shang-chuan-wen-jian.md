**文章目录**

1. [1.formidable简介](http://kirochen.com/2015/07/21/upload-demo-formidable/#formidable简介)
2. [2.简单实现上传文件的功能](http://kirochen.com/2015/07/21/upload-demo-formidable/#简单实现上传文件的功能)
3. [3.简单封装](http://kirochen.com/2015/07/21/upload-demo-formidable/#简单封装)
4. [4.返回的结果](http://kirochen.com/2015/07/21/upload-demo-formidable/#返回的结果)

上传文件是一个网站的基础，所以这次分享一个之前写的一个demo，给初学者参考，大神忽略吧。

### formidable简介 {#formidable简介}

在早期的Node.js，上传文件是一个相对复杂的操作，这是由于Node对Http的封装较低缘故，随后在一些开发者的贡献下，出现了`formidable`，`connect-form`等三方库。这类三方库的出现简化了文件上传。而`connect-form`自己没用过，不好评价和比较。这里就简要介绍下`formidable`。  
formidable优点：上传速度比较可观，占用内存也比较低，简单易用  
使用`formidable`肯定要先安装咯

```
npm install formidable
```

### 简单实现上传文件的功能 {#简单实现上传文件的功能}

在页面前

```
//jade
form(role="form", method="post", enctype='multipart/form-data', action="/upload")
    input(type="text", name="name")
    input(type="file", name="file" id="file")
    button(id="submit", type="submit") upload
```

后端Node代码

```
var express = require('express');
var router = express.Router();
var formidable = require('formidable');
var uuid = require('node-uuid');
var fs = require('fs');

router.post('/upload', function(req, res, next) {
    //创建上传表单
    var form = new formidable.IncomingForm();
    //设置编辑
    form.encoding = 'utf-8';
    //设置上传目录
    form.uploadDir = 'public/upload/';
    form.keepExtensions = true;
    //文件大小
    form.maxFieldsSize = 10 * 1024 * 1024;
    form.parse(req, function (err, fields, files) {
        if(err) {
            res.send(err);
            return;
        }
        console.log(fields);
        var extName = /\.[^\.]+/.exec(files.file.name);
        var ext = Array.isArray(extName)
            ? extName[0]
            : '';
        //重命名，以防文件重复
        var avatarName = uuid() + ext;
        //移动的文件目录
        var newPath = form.uploadDir + avatarName;
        fs.renameSync(files.file.path, newPath);
        res.send('success');
    });
});
```

### 简单封装 {#简单封装}

实现将传入的form-data数据自动将其文件上传，并返回相关的信息

```
//form
form(role="form", method="post", enctype='multipart/form-data', action="/upload")
    input(type="text", name="name")
    input(type="file", name="file" id="file")
    input(type="file", name="file2" id="file2")
    button(id="submit", type="submit") upload


//ajax
input(type="file", name="file" id="file1")
button(id="upload", name="upload") submit
script(type='text/javascript').
    $(function(){
        $('#upload').click(function() {
            var data = new FormData();
            var files = $("#file1")[0].files;
            if(files) {
                data.append("file", files[0]);
            }
            $.ajax({
                type: 'post',
                dataType: 'json',
                url: '/upload',
                data: data,
                contentType: false,
                processData: false,
                success: function(err,result) {
                    console.log(err);
                    console.log(result);
                }
            });
        });
    });
```

```
//multi_upload.js
var formidable = require('formidable');
var uuid = require('node-uuid');
var fs = require('fs');

module.exports = function (req, options, next) {
    if(typeof options === 'function'){
        next = options;
        options = {};
    }
    //创建上传表单
    var form = new formidable.IncomingForm();
    //设置编辑
    form.encoding = options.encoding || 'utf-8';
    //设置上传目录
    form.uploadDir = options.uploadDir || './public/upload/';
    //文件大小
    form.maxFieldsSize = options.maxFieldsSize || 10 * 1024 * 1024;
    //解析
    form.parse(req, function (err, fields, files) {
        if(err) return next(err);
        for (x in files) {
            //后缀名
            var extName = /\.[^\.]+/.exec(files[x].name);
            var ext = Array.isArray(extName)
                ? extName[0]
                : '';
            //重命名，以防文件重复
            var avatarName = uuid() + ext;
            //移动的文件目录
            var newPath = form.uploadDir + avatarName;
            fs.renameSync(files[x].path, newPath);
            fields[x] = {
                size: files[x].size,
                path: newPath,
                name: files[x].name,
                type: files[x].type,
                extName: ext
            };
        }
        next(null, fields);
    });
}


//route
router.post('/upload', function(req, res, next) {
    node_upload(req,function (err, fields) {
        console.log(err);
        res.json(fields);
    });
});
```

###  {#返回的结果}

### 返回的结果 {#返回的结果}

```
//这是在浏览器上的
{
    name: "sss",
    file: {
        size: 775702,
        path: "./public/upload/04ec5a72-867a-4075-9926-f2ea5ff55544.jpg",
        name: "Jellyfish.jpg",
        type: "image/jpeg",
        extName: ".jpg"
    },
    file2: {
        size: 1411,
        path: "./public/upload/7429b7b8-feca-44d1-9039-6446920e46a2.gif",
        name: "QQ截图20150721143806.gif",
        type: "image/gif",
        extName: ".gif"
    }
}
```

[http://kirochen.com/2015/07/21/upload-demo-formidable/](http://kirochen.com/2015/07/21/upload-demo-formidable/)

