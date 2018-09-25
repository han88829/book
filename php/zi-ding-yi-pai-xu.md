```
$arr = [[1, 2, 3], [3, 3, 3], [5, 2], [0, 2]];
// // 自定义排序
uasort($arr, function ($a, $b) {
     if ($a == $b) {
         return 0;
     }
     return ($a < $b) ? -1 : 1;
 });

```



