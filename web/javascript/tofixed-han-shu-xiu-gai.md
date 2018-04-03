```
function toFixed(number, precision) {
    var str = number + ''
    var len = str.length
    var last = str.substr(len - 1, len)
    if (last == '5') {
        last = '6'
        str = str.substr(0, len - 1) + last
        return (str - 0).toFixed(precision)
    } else {
        return number.toFixed(precision)
    }
}
```



