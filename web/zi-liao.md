阮一峰 github 前端各大相关教程

地址：[https://github.com/wangdoc](https://github.com/wangdoc "教程地址")

小程序城市组件按首字母检索

[https://www.cnblogs.com/xjwy/p/8317144.html](https://www.cnblogs.com/xjwy/p/8317144.html)



复制功能

[https://clipboardjs.com/](https://clipboardjs.com/)

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>

<body>
    <article id="article">
        <h4>公园一日游</h4>
        <time>2016.8.15 星期二</time>
        <p>今天风和日丽，我和小红去了人民公园，玩了滑梯、打雪仗、划船，真是愉快的一天啊。</p>
    </article>
    <button id="copy">复制文章</button>
    <textarea style="width: 500px;height: 100px;" placeholder="试一试ctrl + v"></textarea>
    <script>
        function copyArticle(event) {
            const range = document.createRange();
            range.selectNode(document.getElementById('article'));

            const selection = window.getSelection();
            if (selection.rangeCount > 0) selection.removeAllRanges();
            selection.addRange(range);

            document.execCommand('copy');
        }

        document.getElementById('copy').addEventListener('click', copyArticle, false);
    </script>
</body>

</html>
```



