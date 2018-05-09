# 如何修复那些奇怪的 JavaScript 错误

25

2015年01月

调试 JavaScript 也许是一场噩梦：一些错误非常难理解，并且给出的错误行号并不是总是很有帮助。如果有一个列表，列举这些错误的意思和如何修复它们，将对我们非常有帮助。

本文列举了 JavaScript 中一些奇怪的错误。对于相同的错误不同的浏览器可能给出不同的提示，所以分别给出了不同的例子。



## 如何阅读错误

进入正题之前，我们先快速分析一下错误消息的结构，这对我们理解错误消息非常有用，同时也将有助于你理解那些没有在本文中列举的错误。

Chrome 中一个典型的错误看起来像这样：

> Uncaught TypeError: undefined is not a function

该错误的结构如下：

1. **Uncaught TypeError:**
   该部分并不是很有用。
   `Uncaught`
   表示该错误没有被
   `catch`
   语句捕获，
   `TypeError`
   是错误名。
2. **undefined is not a function:**
   是消息体，需要从字面上理解。例如本例中，它的字面意思是，代码尝试将
   `undefined`
   当作函数使用。

其他基于 webkit 的浏览器，比如 Safari，错误消息与 Chrome 基本一样。Firefox 的错误消息与上面非常相似，但并不总是都包含第一部分，最近版本的 IE 的错误消息也比 Chrome 的简单，但在这里，更简单并不意味着更好。

下面看看我们经常会遇到的一些错误。

## Uncaught TypeError: undefined is not a function

同类错误：

* number is not a function
* object is not a function
* string is not a function
* Unhandled Error: ‘foo’ is not a function
* Function Expected

尝试将一个值（value）当作函数使用，但该值并不是一个函数。例如：

| 12 | var foo = undefined;foo\(\); |
| :--- | :--- |


这个错误很常见，当调用对象中的一个方法，但写错了方法名：

| 1 | var x = document.getElementByID\('foo'\); |
| :--- | :--- |


访问对象中不存在的属性时将返回`undefined`，上面代码就将出现该错误。

其他类似的错误，比如“number is not a function”发生在尝试将一个`Number`当作函数使用时。

**如何修复：**确保函数名正确。对于该错误，行号通常准确地指向了错误发生的位置。

## Uncaught ReferenceError: Invalid left-hand side in assignment

同类错误：

* Uncaught exception: ReferenceError: Cannot assign to ‘functionCall\(\)’
* Uncaught exception: ReferenceError: Cannot assign to ‘this’

当尝试给一个不能被赋值的变量赋值时将发生该错误。看下面的典型例子：

| 1 | if\(doSomething\(\) = 'somevalue'\) |
| :--- | :--- |


在上面例子中，开发人员不小心将`==`写成了`=`，错误消息“left-hand side in assignment”指等号左边包含不能被赋值的变量。

**如何修复：**确保不给函数函数的返回值或`this`关键字赋值。

## Uncaught TypeError: Converting circular structure to JSON

同类错误：

* Uncaught exception: TypeError: JSON.stringify: Not an acyclic Object
* TypeError: cyclic object value
* Circular reference in value argument not supported

该错误总是发生在使用`JSON.stringify`序列化一个存在循环引用的对象时。

| 1234 | var a = { };var b = { a: a };a.b = b;JSON.stringify\(a\); |
| :--- | :--- |


由于上面`a`和`b`两个对象都彼此相互引用，结果导致对象不能被转换为 JSON 字符串。

**如何修复：**移除将要被转换为 JSON 字符串对象内部的循环引用。

## Unexpected token ;

同类错误：

* Expected \)
* missing \) after argument list

通常发生在缺少括号或分号时。

该错误中所谓的符号（token）可以多种多样，如“Unexpected token \]”或“Expected {”等等。

**如何修复：**该错误提示的行号有时并不能指向正确的位置，这增加了修复难度。

* 错误信息中包含“\[ \] { } \( \)”时，通常是因为缺少配对的部分，检查所有括号，保证都是配对的。这种情况下，行号通常指向了其他位置，问不是错误的位置。
* 异常的
  `/`
  和正则表达式有关，行号指向了正确的位置。
* 异常的
  `;`
  通常发生在对象、数组或函数调用时参数列表内部包含
  `;`
  ，行号也指向了正确的位置。

## Uncaught SyntaxError: Unexpected token ILLEGAL

同类错误：

* Unterminated String Literal
* Invalid Line Terminator

字符串字面量缺少闭合的引号。

**如何修复：**确保所有字符串都包含闭合的引号。

## Uncaught TypeError: Cannot read property ‘foo’ of null, Uncaught TypeError: Cannot read property ‘foo’ of undefined

同类错误：

* TypeError: someVal is null
* Unable to get property ‘foo’ of undefined or null reference

尝试将`null`过`undefined`作为一个对象使用，例如：

| 12 | var someVal = null;console.log\(someVal.foo\); |
| :--- | :--- |


**如何修复：**通常是由于书写失误导致，确保错误提示的行号附近的变量都是书写正确的。

## Uncaught TypeError: Cannot set property ‘foo’ of null, Uncaught TypeError: Cannot set property ‘foo’ of undefined

同类错误：

* TypeError: someVal is undefined
* Unable to set property ‘foo’ of undefined or null reference

尝试为值为`null`或`undefined`的对象的属性赋值。

| 12 | var someVal = null;someVal.foo = 1; |
| :--- | :--- |


**如何修复：**这也通常是由于书写错误导致，检查错误提示的行号附近的变量名是否正确。

## Uncaught RangeError: Maximum call stack size exceeded

同类错误：

* Uncaught exception: RangeError: Maximum recursion depth exceeded
* too much recursion
* Stack overflow

通常是由程序逻辑问题，导致了无限递归的函数调用。

**如何修复：**检查函数的递归调用，确保函数不是无限递归的。

## Uncaught URIError: URI malformed

同类错误：URIError: malformed URI sequence

无效的`decodeURIComponent`调用将导致该错误。

**如何修复：**确保行号所指位置的`decodeURIComponent`调用的参数正确。

## XMLHttpRequest cannot load . No ‘Access-Control-Allow-Origin’ header is present on the requested resource

同类错误：Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at[http://some/url/](http://some/url/)

该错误总是由使用`XMLHttpRequest`时导致。

**如何修复：**确保请求的 url 满足[同源策略](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)。

## InvalidStateError: An attempt was made to use an object that is not, or is no longer, usable

同类错误：

* InvalidStateError
* DOMException code 11

该错误表示调用对象的方法时，对象的状态不对。在使用`XMLHttpRequest`时，在其准备好之前尝试调用其中的方法将导致该错误。

| 12 | var xhr = new XMLHttpRequest\(\);xhr.setRequestHeader\('Some-Header', 'val'\); |
| :--- | :--- |


上例中将导致错误，因为`setRequestHeader`方法只能在`xhr.open`之后调用。

**如何修复：**检查行号指示的位置，确保代码运行在合适的时间，或在这之前添加必要的函数调用（比如`xhr.open`）。

