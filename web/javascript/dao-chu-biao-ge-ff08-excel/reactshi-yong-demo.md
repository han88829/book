以下在react使用导出excel demo，实测谷歌、火狐、360均可使用

       let data = [{add_time:'123213',sckey:"dasdasdasd",cname:"asdasd"}];
                let hhh = ``;
                data.map(x => {
                    hhh += `

                    <tr>
                    <td>${x.add_time}</td>
                    <td>${x.sckey}</td>
                    <td>${x.cname}</td>
                    <td>${x.add_time}</td>
                    <td>${x.sckey}</td>
                    <td>${x.cname}</td>
                    <td>${x.add_time}</td>
                    <td>${x.sckey}</td>
                </tr>
                `
                })
                let orderHtml = `
                <table border="1">
                <caption>合同管理</caption>
                <tr>
                    <th >创建时间</th>
                    <th >合同编号</th>
                    <th >客户</th>
                    <th >合同类型</th>
                    <th >金额</th>
                    <th >回款</th>
                    <th >负责人</th>
                    <th >货物状态</th>
                </tr>
                ${hhh}
            </table>
        `
                console.log(orderHtml)
                // 使用outerHTML属性获取整个table元素的HTML代码（包括<table>标签），
                // 然后包装成一个完整的HTML文档，设置charset为urf-8以防止中文乱码
                var html = "<html><head><meta charset='utf-8' /></head><body>" + orderHtml + "</body></html>";
                // 实例化一个Blob对象，其构造函数的第一个参数是包含文件内容的数组，第二个参数是包含文件类型属性的对象
                var blob = new Blob([html], { type: "application/vnd.ms-excel" });
                var a = document.getElementById("a");
                // 利用URL.createObjectURL()方法为a元素生成blob URL
                a.href = URL.createObjectURL(blob);
                // 设置文件名，目前只有Chrome和FireFox支持此属性
                a.download = "测试表格.xls";



