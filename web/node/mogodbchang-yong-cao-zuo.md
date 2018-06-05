# 在node中的mongodb及mongoose常见用法

> Mongoose是在node.js环境下对mongodb进行便捷操作的对象模型工具

## 安装

一开始需要安装node.js环境以及mongodb数据库，然后创建mongdb数据文件夹并且启动mongdb（[windows安装启动mongodb](https://link.juejin.im/?target=http%3A%2F%2Fwww.runoob.com%2Fmongodb%2Fmongodb-window-install.html)）。

### connect

connect 用于连接数据库

    mongoose.connect(uri(s), [options], [callback])
    //url(s):数据库地址,可以是多个,以`,`隔开
    //options:可选,配置参数
    //callback:可选,回调

    // 规则
    mongoose.connect('mongodb://数据库地址(包括端口号)/数据库名称')
    // 连接mongodb示例
    mongoose.connect('mongodb://localhost:27017/db');  //连接本地默认27017端口的mongodb
    //连接指定用户连接示例
    mongoose.connect('mongodb://用户名:密码@127.0.0.1:27017/数据库名称')
    //连接多个数据库示例
    //如果你的app中要连接多个数据库，只需要设置多个url以,隔开，同时设置mongos为true
    mongoose.connect('urlA,urlB,...', {
       mongos : true
    })

    作者：飞翔荷兰人
    链接：https://juejin.im/post/5b0b7bbb518825156539d5d3
    来源：掘金
    著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

## 创建schema & model

### schema

schema可以理解为mongoose对表结构的定义\(不仅仅可以定义文档的结构和属性，还可以定义文档的实例方法、静态模型方法、复合索引等\)，每个schema会映射到mongodb中的一个collection，schema不具备操作数据库的能力

```
const mongoose = require('mongoose');

const {Schema} = mongoose;
//  用户对象模型
const userSchema = new Schema({
  name: {
    type: String, //类型
    default: Date.now // 默认值
  },
  avatar: {
        type: String,
        required: true //必须有值
  },
  user: String, 
  passworld: String,
  hash: String, 
  score: Number, 
  learn: Array, 
  message: Array, 
  star: Array, 
  sign: Array,
  signdate: String, 
  isregister: Boolean, 
});

作者：飞翔荷兰人
链接：https://juejin.im/post/5b0b7bbb518825156539d5d3
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

**schema字段类型**

1. String
2. Number
3. Date
4. Buffer
5. Boolean
6. Mixed
7. ObjectId
8. Array

### Model

Model是由Schema编译而成的假想（fancy）构造器，具有抽象属性和行为。Model的每一个实例（instance）就是一个document。document可以保存到数据库和对数据库进行操作。

```
//创建并导出model
const db= {
  User: mongoose.model('MUser', muserSchema),
};
module.exports = db;
```

**现在我们就完成了mongodb的数据连接，数据对象模型的创建。**

## 操作mongodb数据库

引入之前导出的模型mongodb数据`const db = require('../models/db')`

### mongodb查询

#### 1.find

find用来查询并输出该条件下的所有文档`db.Userl.find({conditions}, {options}, callback)`conditions**Object类型**//查询条件 options**Object 类型**//查询配置参数 callback**Function**//回调

##### 查询操作示例

```
// 查询Article模型下所有数据
db.Article.find({}, function(err, docs){
    if (err) {
            console.log('出错'+ err);
            return;
    }
    res.json(docs); // 以json格式输出
});

// 查询Article模型下state字段为'publish'的内容，并且不输出articleContent和user字段内容。注！（1为只输出该字段，0为不输出该字段）
db.Article.find({state: "publish"}, {articleContent: 0,user: 0}, function(err, docs){
    if (err) {
            console.log('出错'+ err);
            return;
    }
    res.json(docs);
});
//查询阅读量大于500小于600的文章
db.Article.find(({views: {$gte: 500, $lte: 600}}), function(err, docs){
    if (err) {
            console.log('出错'+ err);
            return;
    }
    res.json({data:docs});
}) 
// 复杂条件查询
//$ro 查询该模型下的title字段或者tag字段，$regex 为正则匹配实现模糊查询，$options为不区分大小写
db.Article.find({ $or: [{title: {$regex: searchval, $options:'i'} }, { tag: {$regex: searchval, $options:'i' }}] }}, function(err, docs){
    if (err) {
            console.log('出错'+ err);
            return;
    }
    res.json({data:docs});
})

作者：飞翔荷兰人
链接：https://juejin.im/post/5b0b7bbb518825156539d5d3
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

##### 条件查询

1. $or --------------- 或关系
2. $nor ------------- 或关系取反
3. $gt --------------- 大于
4. $gte -------------- 大于等于
5. $lt ---------------- 小于
6. $lte --------------- 小于等于
7. $ne --------------- 不等于
8. $in ---------------- 在多个值范围内
9. $nin -------------- 不在多个值范围内
10. $all --------------- 匹配数组中多个值
11. $regex ----------- 正则，用于模糊查询
12. $size ------------- 匹配数组大小
13. $maxDistance -- 范围查询，距离（基于LBS）
14. $mod ------------ 取模运算
15. $near ------------ 邻域查询，查询附近的位置（基于LBS）
16. $exists ---------- 字段是否存在
17. $elemMatch --- 匹配内数组内的元素
18. $within ---------- 范围查询（基于LBS）
19. $box ------------- 范围查询，矩形范围（基于LBS）
20. $center ---------- 范围醒询，圆形范围（基于LBS）
21. $centerSphere - 范围查询，球形范围（基于LBS）
22. $slice ------------- 查询字段集合中的元素（比如从第几个之后，第N到第M个元素

#### 2.findOne

与find类似，但只返回单个文档

```
db.Article.findOne({title: req.body.title}, function(err, docs){
    if (err) {
        console.log('出错'+ err);
    }
    res.json(docs);
})
```

#### 3.findById

与findOne类似，但它接收文档的 \_id 作为参数，返回单个文档。\_id 可以是字符串或 ObjectId 对象。

```
db.Article.findOne(id, function(err, docs){
    if (err) {
        console.log('出错'+ err);
    }
    res.json(docs);
})
```

#### 4.count

返回符合条件的文档数。

```
db.Article.count(id, function(err, docs){
    if (err) {
        console.log('出错'+ err);
    }
    res.json(docs);
})
```

#### 5.where

当查询比较复杂时，用 where：

```
db.Article.where('age').gte(25)
.where('tags').in(['movie', 'music', 'art'])
.select('name', 'age', 'tags')
.skip(20)
.limit(10)
.asc('age')
.slaveOk()
.hint({ age: 1, name: 1 })
.run(function(err, docs){
    if (err) {
        console.log('出错'+ err);
    }
    res.json(docs);
}));

作者：飞翔荷兰人
链接：https://juejin.im/post/5b0b7bbb518825156539d5d3
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

#### $where

有时我们需要在 mongodb 中使用 javascript 表达式进行查询，这时可以用`find({$where : javascript})`方式，$where 是一种快捷方式，并支持链式调用查询。

```
db.MUser.$where('this.firstname === this.lastname').exec((err, docs) => {
    res.json({docs});
});
```

#### sort & skip & limit

sort为排序方法，为该字段正序或者倒序输出内容\(-1为倒序\) skip为跳过多少条目 limit 为限制输出多少条

```
// 实现文章分页，按时间倒序排列输出
db.Article.find({tag:req.params.labe}, function(err, docs){
    if (err)return;
    res.json(docs)
}).sort({date:-1}).skip(page*pagenum).limit(pagenum)

作者：飞翔荷兰人
链接：https://juejin.im/post/5b0b7bbb518825156539d5d3
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### mongodb增加

save是一个实例方法，使用时需要先 new Model\(\) 来实例化

```
//保存一个用户信息，userobj为你创建的文档对象模型里的字段，需正确对应传入
const userobj={
    email: query,
    passworld: req.body.passworld,
    hash: hash,
    isregister: false,
    score: 5,
    sign: [],
    signdate: ''
}
new db.MUser(userobj).save(function(error){
    if (error) {
        res.status(500).send()
        return
    }
    res.json({statu: 200})
})

作者：飞翔荷兰人
链接：https://juejin.im/post/5b0b7bbb518825156539d5d3
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### mongodb删除

#### remove

删除数据方法

```
db.Course.remove({_id: req.body.id}, function(err, docs){
    if (err) {
            res.status(500).send();
            return
    }
    res.json({statu: 200})
})

作者：飞翔荷兰人
链接：https://juejin.im/post/5b0b7bbb518825156539d5d3
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### mongodbg更新

#### update

更新数据方法

```
// 更新指定email字段数据条目下字段为content的内容，如果不存在就创建该字段
db.Share.update({email: email},{$set:{content: newarr}}, function(err, docs){
    if (err) {
            res.status(500).send();
            return
    }
    res.json({statu: 200});
})

//$set 指定字段的值，这个字段不存在就创建它。可以是任何MondoDB支持的类型。
Article.update({_id : id}, {$set : {views : 51, title : ‘修改后的标题’ …}})

//$unset 同上取反，删除一个字段
Article.update({views : 50}, {$unset : {views : ‘remove’}})
//执行后: views字段不存在

//$inc 增减修改器，只对数字有效。
Article.update({_id : id}, {$inc : {views : 1}})

//$push 为字段为数组的内容push数据
Article.update({_id : id}, {$push : {message : messageobj}})

//$pop从头部或尾部删除单个元素（1为从后面删除,-1为从前面删除）
db.Article.update(({_id: id), {$pop:{relationships: -1})

//__$pull__删除满足条件的元素，不止删除一个
db.Article.update(({_id: id), {$pull:{“relationships”:{“fname”:”dongren”, ”lname”: ”zeng”}}})

```

**$set**指定字段的值，这个字段不存在就创建它。可以是任何MondoDB支持的类型。

`Article.update({_id : id}, {$set : {views : 51, title : '修改后的标题' ...}})`

**$unset**同上取反，删除一个字段

`Article.update({views : 50}, {$unset : {views : 'remove'}})`//执行后: views字段不存在

**$inc**增减修改器，只对数字有效。

`Article.update({_id : id}, {$inc : {views : 1}})`

**$push**为字段为数组的内容push数据

`Article.update({_id : id}, {$push : {message : messageobj}})`

**$pop**从头部或尾部删除单个元素（1为从后面删除,-1为从前面删除）

`db.Article.update(({_id: id), {$pop:{relationships: -1})`

**$pull**删除满足条件的元素，不止删除一个

`db.Article.update(({_id: id), {$pull:{“relationships”:{“fname”:”dongren”, ”lname”: ”zeng”}}})`

