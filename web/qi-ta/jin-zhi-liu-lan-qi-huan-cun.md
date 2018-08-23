```
// 配合 mate禁用 缓存标签，实现禁用浏览器缓存（实现原理，自动刷新）     
// <meta HTTP-EQUIV="pragma" CONTENT="no-cache">      
// <meta HTTP-EQUIV="Cache-Control" CONTENT="no-store, must-revalidate">      
// <meta HTTP-EQUIV="expires" CONTENT="Wed, 26 Feb 1997 08:21:57 GMT">      
// <meta HTTP-EQUIV="expires" CONTENT="0">     
if(!window.name){         
    var str = Math.random().toString(36).substr(2);//随机字符串         
    window.location.href += '?S='+ str;//兼容微信浏览器刷新
    window.name = 'isreload';    
 }

```



