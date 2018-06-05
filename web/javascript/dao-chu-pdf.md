```
 var downPdf = document.getElementById("renderPdf");
      downPdf.onclick = function() {
          html2canvas(document.body, {
              onrendered:function(canvas) {

                  //返回图片dataURL，参数：图片格式和清晰度(0-1)
                  var pageData = canvas.toDataURL('image/jpeg', 1.0);

                  //方向默认竖直，尺寸ponits，格式a4[595.28,841.89]
                  var pdf = new jsPDF('', 'pt', 'a4');

                  //addImage后两个参数控制添加图片的尺寸，此处将页面高度按照a4纸宽高比列进行压缩
                  pdf.addImage(pageData, 'JPEG', 0, 0, 595.28, 592.28/canvas.width * canvas.height );

                  pdf.save('stone.pdf');

              }
          })
      }
```

[https://segmentfault.com/a/1190000009211079](https://segmentfault.com/a/1190000009211079)

```
           html2canvas(document.getElementById('table1_1'), {
                allowTaint: true,
                height: document.getElementById('table1_1').offsetHeight,
            }).then(canvas => {
                //返回图片URL，参数：图片格式和清晰度(0-1)
                var pageData = canvas.toDataURL('image/jpeg', 1.0);

                //方向默认竖直，尺寸ponits，格式a4【595.28,841.89]
                var pdf = new jsPDF('l', 'pt', 'a4');

                //需要dataUrl格式
                pdf.addImage(pageData, 'JPEG', 0, 0, 595.28,592.28 / canvas.width* canvas.height );

                pdf.save('pdf.pdf');

                this.setState({ print: false });

            })
```





按照图片大小进行导出pdf

```
           html2canvas(document.getElementById('table1_3'), {
                allowTaint: true,
                dpi: window.devicePixelRatio,
            }).then(canvas => {

                //返回图片URL，参数：图片格式和清晰度(0-1)
                var pageData = canvas.toDataURL('image/jpeg', 1.0);

                //方向默认竖直，尺寸ponits, 按照图片大小进行输出
                var pdf = new jsPDF('l', 'px', [canvas.width, canvas.height]);
                //需要dataUrl格式
                pdf.addImage(pageData, 'JPEG', 0, 0, canvas.width, canvas.height);

                pdf.save('1-2.pdf');
            })
```



