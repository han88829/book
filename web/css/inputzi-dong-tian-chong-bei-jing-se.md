谷歌浏览器自动填充密码的背景色一直都是黄色，这个只能通过谷歌独有的

修改：有辦法直接修改。

```
input
:-webkit-autofill
 {
    
background-color
: 
rgb
(250, 255, 189);
    
background-image
: none;
    
color
: 
rgb
(0, 0, 0);
}

```

貌似不能覆蓋默認樣式，但是可以用別的樣式曲線達到目的。

```
input
:-webkit-autofill
 {
    
-webkit-box-shadow
: 
0
0
0px
1000px
 white inset;
}
```



上面只能修改颜色

```
:-webkit-autofill {-webkit-text-fill-color: #fff !important;
  transition: background-color 5000s ease-in-out 0s;//设置5000s后改变填充密码框的背景色，唯一使背景透明的方法
}
```



