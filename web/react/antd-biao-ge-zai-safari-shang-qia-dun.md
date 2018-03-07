使用antd2.x版本在safari上面异常卡顿，很多客户反馈，测试了一下，卡顿的主要是11.0版本以上的；

去issues上面查了一下，有人讲是因为table里面使用overflow:hidden; 导致的，于是去除node\_modules中antd table中所有的overflow:hidden; 解决了这个问题；

参考地址： [https://github.com/ant-design/ant-design/issues/7799](https://github.com/ant-design/ant-design/issues/7799);

解决问题作者：[https://github.com/ant-design/ant-design/issues/8538](https://github.com/ant-design/ant-design/issues/8538)



```
浏览器版本：版本 11.0.2 (13604.4.7.1.3)
系统：10.13.2（macos high sierra）

试验过后发现antd的table 在safari 渲染直接阻塞住页面其他事件，造成页面卡顿
期间 table 有一个prefixCls 的属性，我将属性不传递给rc-table的时候也没渲染不卡了，
感觉是css什么属性影响到了，但是没找到

其中 https://preview.pro.ant.design/#/list/table-list 例子中翻页，排序，然后点击其他均有卡顿现象。

在系统升级之前，safari（12.06）确认过同样的页面渲染和交互不卡顿。升级之后造成的。

后来经过排查，发现ant-table && ant-table table 的overflow: hidden造成的,去掉以后再safari下不卡顿，
```



