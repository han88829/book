删除Cookie时如果没有指定Path在静态页面下会不能删除

如下代码：

```
function delCookie(name){
var ex = new Date();ex.setTime(ex.getTime()-1);
document.cookie = cookieName + "=; expires="+ex.toGMTString();
}
```

用在动态页面可以，用在html静态页面不能正常删除

比对addCookie代码发现Path=/，经验证

如下代码可以正常使用：

```
function delCookie(name){
var ex = new Date();ex.setTime(ex.getTime()-1);
document.cookie = cookieName + "=; expires="+ex.toGMTString() + ";path=/";
}
```

# cookie存储path

cookie存储的时候，会自动加上path，只有子路由下面的才能在发送的时候获取到



