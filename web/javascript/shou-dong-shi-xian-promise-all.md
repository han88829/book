```
Promise.prototype.all = (arr) => {
    let values = [];
    return new Promise((res, rej) => {
        if (!isArray(arr)) {
            rej('参数不是一个数组！');
            return
        }

        for (let i = 0; i < arr.length; i++) {
            Promise.resolve(arr[i]).then(val => {
                values.push(val);
            }).catch(err => {
                rej(err);
                break
            });

            if (i == (arr.length - 1)) {
                res(values);
                break;
            }
        };
    });
}
```



