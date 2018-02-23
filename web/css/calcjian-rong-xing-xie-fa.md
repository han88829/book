css3 兼容性写法

```
width: 90%;/*写给不支持calc()的浏览器*/ 
width:-moz-calc(100% - (10px + 5px) * 2); 
width:-webkit-calc(100% - (10px + 5px) * 2); 
width: calc(100% - (10px + 5px) * 2);

```



