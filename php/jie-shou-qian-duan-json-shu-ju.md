#### 前端请求代码：

```
let headres = {
            method: 'post',
            credentials: 'include',
            headers: {
                contentType: "application/json;charset=utf-8",
            },
            body: JSON.stringify(data)
        }
        fetch('/addCompared', headres).then(x => x.json()).then(x => {
            console.log(x);
        }).catch(err => {
            console.error(err);
        })
```

#### 后端php处理数据:

```
 $data = file_get_contents('php://input');
 $data = json_decode($data, true);//第二个参数true，使得$data变为array类型否则会出现以下错误

//Fatal error: Cannot use object of type stdClass as array in
//解决办法 $result = json_decode($data, true);
```



