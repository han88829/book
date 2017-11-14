一、 mobx 数组的观察者模式

mobx的数组是人造数组，拥有数组的一切方法，如果一个数组是mobx数组，在数组里面添加新的索引会造成，mobx无法观察此索引的数据，无法及时更新，所以在数组添加数据或者数组内的索引时；务必先将数据全部转换成正常数组，然后在同意赋值给mobx数组

* 错误的书写：

```
let data = [{id:1},{id:2}];
//或者 使用mobx的toJS进行转换如下所示：
let dataMobx = toJS(this.props.store.data);

data.forEach((x, i) => {
    data[i].name = i;
});

this.props.store.data = data;
```

* 错误的书写：

```
let data = this.props.store.data;
data.forEach((x, i) => {
    data[i].name = i;
});

this.props.store.data = data;
```



