```

Number.prototype.toFixed = function (exponent) {
    if (this > 0) {
        return parseInt(this * Math.pow(10, exponent) + 0.5) / Math.pow(10, exponent);
    } else {
        return parseInt(this * Math.pow(10, exponent) - 0.5) / Math.pow(10, exponent);
    }
}
```



