不适用插件实现的三种方法：

## **&lt; iframe &gt;** {#iframe}

所有浏览器都支持 &lt; iframe &gt; 标签，直接将src设置为指定的PDF文件就可以预览了。此外可以把需要的文本放置在 &lt; iframe &gt; 和 之间，这样就可以应对无法理解 iframe 的浏览器，比如下面的代码可以提供一个PDF的下载链接：

```
<iframe src="/index.pdf" width="100%" height="100%">

This browser does not support PDFs. Please download the PDF to view it: <a href="/index.pdf">Download PDF</a>

</iframe>
```

## **&lt; embed &gt;** {#embed}

&lt; embed &gt; 标签定义嵌入的内容，比如插件。在HTML5中这个标签有4个属性：

| 属性 | 值 | 描述 |
| :--- | :--- | :--- |
| height | pixels | 设置嵌入内容的高度。 |
| width | pixels | 设置嵌入内容的宽度。 |
| type | type | 定义嵌入内容的类型。 |
| src | url | 嵌入内容的 URL。 |

但是需要注意的是这个标签不能提供回退方案，与&lt; iframe &gt;&lt; / iframe &gt;  
不同，这个标签是自闭合的的，也就是说如果浏览器不支持PDF的嵌入，那么这个标签的内容什么都看不到。用法如下：

```
<embed src="/index.pdf" type="application/pdf" width="100%" height="100%">

```

## **&lt; object &gt;** {#object}

&lt; object &gt;定义一个嵌入的对象，请使用此元素向页面添加多媒体。此元素允许您规定插入 HTML 文档中的对象的数据和参数，以及可用来显示和操作数据的代码。用于包含对象，比如图像、音频、视频、Java applets、ActiveX、PDF 以及 Flash。几乎所有主流浏览器都拥有部分对 &lt; object &gt; 标签的支持。这个标签在这里的用法和&lt; iframe &gt;很小，也支持回退：

```
<object data="/index.php" type="application/pdf" width="100%" height="100%">

This browser does not support PDFs. Please download the PDF to view it: <a href="/index.pdf">Download PDF</a>

</object>

```

当然，结合&lt; object &gt;和&lt; iframe &gt;能提供一个更强大的回退方案：

```
<object data="/index.pdf" type="application/pdf" width="100%" height="100%">

<iframe src="/index.pdf" width="100%" height="100%" style="border: none;">

This browser does not support PDFs. Please download the PDF to view it: <a href="/index.pdf">Download PDF</a>

</iframe>

</object>
```

以上三个标签是一种无需JavaScript支持的PDF预览方案。下面提到的PDFObject和PDF.js都是js库。

## **PDFObject** {#pdfobject}

看官网上的介绍，PDFObject并不是一个PDF渲染工具，它也是通过&lt; embed &gt;标签实现PDF预览：

PDFObject is not a rendering engine. PDFObject just writes an &lt; embed &gt; element to the page, and relies on the browser or browser plugins to render the PDF. If the browser does not support embedded PDFs, PDFObject is not capable of forcing the browser to render the PDF.

PDFObject提供了一个PDFObject.supportsPDFs用于判断该浏览器能否使用PDFObject：

```
if(PDFObject.supportsPDFs){
   console.log("Yay, this browser supports inline PDFs.");
} else {
   console.log("Boo, inline PDFs are not supported by this browser");
}
```

整个PDFObject使用起来非常简单，完整代码：

```
<!DOCTYPE html>
<html>
<head>
    <title>Show PDF</title>
    <meta charset="utf-8" />
    <script type="text/javascript" src='pdfobject.min.js'></script>
    <style type="text/css">
        html,body,#pdf_viewer{
            width: 100%;
            height: 100%;
            margin: 0;
            padding: 0;
        }
    </style>
</head>
<body>
    <div id="pdf_viewer"></div>
</body>
<script type="text/javascript">
    if(PDFObject.supportsPDFs){
        // PDF嵌入到网页
        PDFObject.embed("index.pdf", "#pdf_viewer" );
    } else {
        location.href = "/canvas";
    }
</script>
</html>
```

效果如下：  
![](https://img-blog.csdn.net/20170903155444545?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1eWFxaTE5OTM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast "这里写图片描述")

## **PDF.js** {#pdfjs}

PDF.js可以实现在html下直接浏览pdf文档，是一款开源的pdf文档读取解析插件，非常强大，能将PDF文件渲染成Canvas。PDF.js主要包含两个库文件，一个pdf.js和一个pdf.worker.js，一个负责API解析，一个负责核心解析。  
首先引入pdf.js文件`<script type="text/javascript" src='pdf.js'></script>`  
PDF.js大部分用法都是基于Promise的，PDFJS.getDocument\(url\)方法返回的就是一个Promise：

```
PDFJS.getDocument('../index.pdf').then(pdf=>{
        var numPages = pdf.numPages;
        var start = 1;
        renderPageAsync(pdf, numPages, start);
    });
```

Promise返回的pdf是一个PDFDocumentProxy对象官网API介绍是：

Proxy to a PDFDocument in the worker thread. Also, contains commonly used properties that can be read synchronously.

PDF的解析工作需要通过pdf.getPage\(page\)去执行，这个方法返回的也是一个Promise，因此可以通过async/await函数去逐页解析PDF：

```
async function renderPageAsync(pdf, numPages, current){
        for(let i=1; i<=numPages; i++){
            // 解析page
            let page = await pdf.getPage(i);
            // 渲染
            // ...
        }
    }

```

得到的page是一个PDFPageProxy对象，即Proxy to a PDFPage in the worker thread 。这个对象得到了这一页的PDF解析结果，我们可以看下这个对象提供的方法：

| 方法 | 返回 |
| :--- | :--- |
| getAnnotations | A promise that is resolved with an {Array} of the annotation objects. |
| getTextContent | That is resolved a TextContent object that represent the page text content. |
| getViewport | Contains ‘width’ and ‘height’ properties along with transforms required for rendering. |
| render | An object that contains the promise, which is resolved when the page finishes rendering. |

我们可以试试调用getTextContent方法，并将其结果打印出来：

```
page.getTextContent().then(v=>console.log('page', v));
```

第一页部分结果如下：

```
{
    "items": [
        {
            "str": "小册子标题",
            "dir": "ltr",
            "width": 240,
            "height": 2304,
            "transform": [
                48,
                0,
                0,
                48,
                45.32495,
                679.04
            ],
            "fontName": "g_d0_f1"
        },
        {
            "str": " ",
            "dir": "ltr",
            "width": 9.600000000000001,
            "height": 2304,
            "transform": [
                48,
                0,
                0,
                48,
                285.325,
                679.04
            ],
            "fontName": "g_d0_f2"
        }
      ],
    "styles": {
        "g_d0_f1": {
            "fontFamily": "monospace",
            "ascent": 1.05810546875,
            "descent": -0.26171875,
            "vertical": false
        },
        "g_d0_f2": {
            "fontFamily": "sans-serif",
            "ascent": 0.74365234375,
            "descent": -0.25634765625
        }
    }
 }
```

我们可以发现，PDF.js将每页文本的字符串、位置、字体都解析出来，感觉还是挺厉害的。

官网有个demo，还用到了官网提到的viewer.js（我认为它的作用是对PDF.js渲染结果再次处理）：[http://mozilla.github.io/pdf.js/web/viewer.html](http://mozilla.github.io/pdf.js/web/viewer.html)，我看了一下它的HTML机构，首先**底图是一个Canvas**，内容和PDF一样（通过下面介绍的page.render方法可以得到），底图之上是一个textLayer，我猜想这一层就是通过page.getTextContent\(\)得到了字体的位置和样式，再覆盖在Canvas上：  
![](https://img-blog.csdn.net/20170903163833125?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1eWFxaTE5OTM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast "这里写图片描述")  
通过这种方式就能实现再预览文件上选中文字（刚开始我还在纳闷为什么渲染成Canvas还能选择文字）  
![](https://img-blog.csdn.net/20170903164118478?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1eWFxaTE5OTM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast "这里写图片描述")

将page渲染成Canvas是通过render方法实现的，代码如下：

```
   async function renderPageAsync(pdf, numPages, current){
        console.log("renderPage async");
        for(let i=1; i<=numPages; i++){
            // page
            let page = await pdf.getPage(i);

            let scale = 1.5;
            let viewport = page.getViewport(scale);
            // Prepare canvas using PDF page dimensions.
            let canvas = document.createElement("canvas");
            let context = canvas.getContext('2d');
            document.body.appendChild(canvas);

            canvas.height = viewport.height;
            canvas.width = viewport.width;

            // Render PDF page into canvas context.
            let renderContext = {
                    canvasContext: context,
                    viewport: viewport
            };
            page.render(renderContext);
        }
    }
```

PDF.js是Mozilla实验室的作品，感觉真的很强大！  
我在码云上有个demo，结合了PDFObject和PDF.js。因为PDFObject使用的&lt; embed &gt;标签可以直接显示PDF文件，速度很快；但是手机上很多浏览器不支持，比如微信的浏览器、小米浏览器，所以我就使用了PDF.js将其渲染成Canvas，速度与PDFObject相比慢多了，但至少能看。-\_-\|\|  
demo地址：[https://git.oschina.net/liuyaqi/PDFViewer.git](https://git.oschina.net/liuyaqi/PDFViewer.git)



原文地址：[https://blog.csdn.net/liuyaqi1993/article/details/77822946](https://blog.csdn.net/liuyaqi1993/article/details/77822946)

