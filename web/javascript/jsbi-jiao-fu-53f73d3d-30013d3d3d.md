最近网上看到一道有趣的题目：

```
if(a===1&&a===2&&a===3){
  console.log('hello word!')
}
```

如何才能打印出hello word！呢，要是放到其他语言估计回答就是no；但是在js中我们可以利用隐式转换来是等式成立

```
var a = {
    i:0,
    toString:function (){
        return ++i;
    }
}
```

在不同类型进行比较时，js会利用toString、valueOf对数据进行操作，所以我们修改这两个原型方法，使得每次调用时都修改下a中的i

