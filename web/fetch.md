**一、 fetch默认不发送cookie session**

```
get post 请求都必须加上  credentials: 'include'
```

**二、 服务端跨域请求头配置**

```
try_files $uri $uri/ /index.php?s=$uri&$args;
index index.html index.htm index.php .nears.php;
add_header Access-Control-Allow-Origin *;
add_header Access-Control-Allow-Headers X-Requested-With;
add_header Access-Control-Allow-Methods GET,POST,OPTIONS;
```



