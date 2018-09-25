

帐号密码连接：

```
mongodb://username:password@localhost:27017/
```

一直提示失败，加入一下验证成功连接

```
?authSource=admin
```

```
Hmm also what database is the user defined on? MongoDB users are scoped to dbs, even if they have cross-db privileges, so if the user you have isn't defined on db you'll need to put something like mongoose.connect('mongodb://${user}:${pass}@${uri}/${db}?authSource=admin') if the user is defined on the 'admin' db.

Also, if you're on mongodb 3.x, you might be using SCRAM-SHA-1 auth or not. Try both of the below:

mongoose.connect('mongodb://${user}:${pass}@${uri}/${db}?authMechanism=SCRAM-SHA-1')
mongoose.connect('mongodb://${user}:${pass}@${uri}/${db}?authMechanism=MONGODB-CR')
```



