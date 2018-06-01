### 使用node打印所选文件夹目录列表，并生成一个readme.md文件



    const path = require('path');
    const fs = require('fs');

    let target = path.join(__dirname, process.argv[2] || "./");
    let jsonText = '\n\n';
    function onreadFile(target, deep) {
        const dirInfo = fs.readdirSync(target);

        let level = new Array(deep + 1).join('│ ');

        let dir = [];
        let files = [];

        dirInfo.forEach(item => {
            let stat = fs.statSync(path.join(target, item));

            if (stat.isDirectory()) {
                dir.push(item);
            } else {
                files.push(item);
            }
        });

        let len = deep + 1;

        dir.forEach(item => {
            console.log(`${level}├─${item}`);
            jsonText += `${level}├─${item}` + '\n';
            onreadFile(path.join(target, item), len);
        })

        files.forEach((item, index) => {
            if (index == (files.length - 1)) {
                console.log(`${level}└─${item}`);
                jsonText += `${level}└─${item}` + '\n';
            } else {
                console.log(`${level}├─${item}`);
                jsonText += `${level}├─${item}` + '\n';
            }
        })
        if (dir.length <= 0) {
            let timer = setTimeout(() => {
                fs.writeFile('./readme.md', jsonText, function (err) {
                    console.log(err);
                });
                clearTimeout(timer);
            }, 0)

        }
    }

    onreadFile(target, 0);



