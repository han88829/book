### 小程序在在面之间用url来传递参数，但是这个url不用转码的话，太长就会截取掉，所以使用以下方式进行传递参数：

```
传送：
wx.navigateTo({
    url: '../video/video?id=' + encodeURIComponent(id)
});

接收：
onLoad: function (query) {
    var url = decodeURIComponent(query.id);
}
```



