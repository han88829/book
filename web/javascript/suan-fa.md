### 一、快速排序

```
//快速排序 
        var quickSort = function (arr) {
            if (arr.length <= 1) {
                return arr;
            }
            var index = Math.floor(arr.length / 2);
            // 取出位于中间的这个数字
            var piovt = arr.splice(index, 1)[0];
            var left = [];
            var right = [];

            for (let i = 0; i < arr.length; i++) {
                if (arr[i] < piovt) {
                    left.push(arr[i])
                } else {
                    right.push(arr[i])
                }
            }
            //使用递归不断重复quickSort，直到只剩一个元素
            return [...quickSort(left), piovt, ...quickSort(right)]
        };
        console.log(quickSort([1, 2, 3, 4, 2, 1, 2, 3, 5, 73, 312, 6]))
```



