在JavaScript中，对于`Object`和`Array`这类引用类型值，当从一个变量向另一个变量复制引用类型值时，这个值的副本其实是一个指针，两个变量指向同一个堆对象，改变其中一个变量，另一个也会受到影响。

这种拷贝分为两种情况：拷贝引用和拷贝实例，也就是我们说的浅拷贝和深拷贝

### 浅拷贝（shallow copy） {#articleHeader0}

拷贝原对象的引用，这是最简单的浅拷贝。

```
// 对象
var o1 = {a: 1};
var o2 = o1;

console.log(o1 === o2);  // =>true
o2.a = 2; 
console.log(o1.a); // => 2

// 数组
var o1 = [1,2,3];
var o2 = o1;

console.log(o1 === o2); // => true
o2.push(4);
console.log(o1); // => [1,2,3,4]
```

拷贝原对象的实例，但是对其内部的引用类型值，拷贝的是其引用，常用的就是如jquey中的`$.extend({}, obj);Array.prototype.slice()`和`Array.prototype.concat()`都会返回一个数组或者对象的浅拷贝，举个例子：

```
var o1 = ['darko', {age: 22}];
var o2 = o1.slice(); // 根据Array.prototype.slice()的特性，这里会返回一个o1的浅拷贝对象

console.log(o1 === o2); // => false，说明o2拷贝的是o1的一个实例

o2[0] = 'lee';
console.log(o1[0]); // => "darko" o1和o2内部包含的基本类型值，复制的是其实例，不会相互影响

o2[1].age = 23;
console.log(o1[1].age); // =>23 o1和o2内部包含的引用类型值，复制的是其引用，会相互影响
```

可以通过`Array.prototype.slice()`或`jQuery`中的`$.extend({}, obj)`完成对一个数组或者对象的浅拷贝，我们也可以自己写一个简单浅拷贝函数来加深对浅拷贝的理解、

```
// 浅拷贝实现，仅供参考
function shallowClone(source) {
    if (!source || typeof source !== 'object') {
        throw new Error('error arguments');
    }
    var targetObj = source.constructor === Array ? [] : {};
    for (var keys in source) {
        if (source.hasOwnProperty(keys)) {
            targetObj[keys] = source[keys];
        }
    }
    return targetObj;
}

```

### 深拷贝（deep copy） {#articleHeader1}

深拷贝也就是拷贝出一个新的实例，新的实例和之前的实例互不影响，深拷贝的实现有几种方法，首先我们可以借助jQuery，lodash等第三方库完成一个深拷贝实例。在jQuery中可以通过添加一个参数来实现递归extend，调用`$.extend(true, {}, ...)`就可以实现一个深拷贝。

我们也可以自己实现一个深拷贝的函数，通常有两种方式，一种就是用`递归`的方式来做，还有一种是利用`JSON.stringify`和`JSON.parse`来做，这两种方式各有优劣，先来看看递归的方法怎么做。

jQuery中的extend方法基本的就是按照这个思路实现的，但是没有办法处理源对象内部`循环引用`的问题，同时对Date，Funcion等类型值也没有实现真正的深度复制，但是这些类型的值在重新定义的时候一般都是直接覆盖，所以也不会对源对象产生影响，从一定程度上来说也算是实现了一个深拷贝。

```
// 递归实现一个深拷贝
function deepClone(source){
   if(!source || typeof source !== 'object'){
     throw new Error('error arguments', 'shallowClone');
   }
   var targetObj = source.constructor === Array ? [] : {};
   for(var keys in source){
      if(source.hasOwnProperty(keys)){
         if(source[keys] && typeof source[keys] === 'object'){
           targetObj[keys] = source[keys].constructor === Array ? [] : {};
           targetObj[keys] = deepClone(source[keys]);
         }else{
           targetObj[keys] = source[keys];
         }
      } 
   }
   return targetObj;
}
// test example
var o1 = {
  arr: [1, 2, 3],
  obj: {
    key: 'value'
  },
  func: function(){
    return 1;
  }
};
var o3 = deepClone(o1);
console.log(o3 === o1); // => false
console.log(o3.obj === o1.obj); // => false
console.log(o2.func === o1.func); // => true
```

还有一种实现深拷贝的方式是利用`JSON对象`中的`parse`和`stringify`，JOSN对象中的stringify可以把一个js对象序列化为一个JSON字符串，parse可以把JSON字符串反序列化为一个js对象，通过这两个方法，也可以实现对象的深复制。

我们从下面的例子就可以看到，源对象的方法在拷贝的过程中丢失了，这是因为`在序列化JavaScript对象时，所有函数和原型成员会被有意忽略`，这个实现可以满足一些比较简单的情况，能够处理JSON格式所能表示的所有数据类型，同时如果在对象中存在循环应用的情况也无法正确处理。

```
// 利用JSON序列化实现一个深拷贝
function deepClone(source){
  return JSON.parse(JSON.stringify(source));
}
var o1 = {
  arr: [1, 2, 3],
  obj: {
    key: 'value'
  },
  func: function(){
    return 1;
  }
};
var o2 = deepClone(o1);
console.log(o2); // => {arr: [1,2,3], obj: {key: 'value'}}
```



