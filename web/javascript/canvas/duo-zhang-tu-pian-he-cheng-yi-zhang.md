```
            let img =  [{img:"http://baidu.com/png/png",height:10,width:10}];
            let width = img.reduce((a, b) => {
                if (a <= b.width) {
                    a = b.width;
                }

                return a
            }, 0);
            let height = img.reduce((a, b) => {
                a = a + b.height;
                return a
            }, 0);
            var canvas = document.createElement('canvas');  //创建canvas 对象
            var ctx = canvas.getContext('2d');
            ctx.fillStyle = "#fff";
            canvas.width = width;   //这里 由于绘制的dom 为固定宽度，居中，所以没有偏移
            canvas.height = height;
            let imgData = [];
            for (let i = 0; i < img.length; i++) {
                let imgHtml = new Image();
                imgHtml.src = img[i].img;
                let h = img.reduce((a, b, index) => {
                    if (index < i) {
                        a += b.height;
                    }

                    return a
                }, 0);
                imgHtml.setAttribute("crossOrigin", 'Anonymous')

                imgHtml.onload = () => {
                    imgData.push(imgHtml);
                    imgData.push(0);
                    imgData.push(h);
                    imgData.push(img[i].width);
                    imgData.push(img[i].height);
                    ctx.drawImage(imgHtml, 0, h, img[i].width, img[i].height);
                    if (i >= (img.length - 1)) {


                        let srccc = canvas.toDataURL("image/jpeg", 1);

                    }
                }

            }
```



