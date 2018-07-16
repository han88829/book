```
/**
 * 网络图像文件转Base64
 */
function getBase64Image(img) {
    var canvas = document.createElement("canvas");
    canvas.width = img.width;
    canvas.height = img.height;
    var ctx = canvas.getContext("2d");
    ctx.drawImage(img, 0, 0, img.width, img.height);
    var ext = img.src.substring(img.src.lastIndexOf(".") + 1).toLowerCase();
    var dataURL = canvas.toDataURL("image/" + ext);
    return dataURL;
}


/**
*Base64字符串转二进制
*/
function dataURLtoBlob(dataurl) {
    var arr = dataurl.split(','),
    mime = arr[0].match(/:(.*?);/)[1],
    bstr = atob(arr[1]),
    n = bstr.length,
    u8arr = new Uint8Array(n);
    while (n--) {
        u8arr[n] = bstr.charCodeAt(n);
    }
    return new Blob([u8arr], {
        type: mime
    });
}


var img = "图像文件URL地址";
var image = new Image();
image.src = img;
image.onload = function() {
    //这样就获取到了文件的Base64字符串
    var base64 = getBase64Image(image);
    //Base64字符串转二进制
    var file = dataURLtoBlob(base64);
}


/**
*blob字符串转Base64
*/
function blobToDataURL(blob, callback) {

    var a = new FileReader();

    a.onload = function (e) {

        callback(e.target.result);

    }

    a.readAsDataURL(blob);

}
```



