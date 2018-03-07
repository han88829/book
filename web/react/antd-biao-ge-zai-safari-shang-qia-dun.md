使用antd2.x版本在safari上面异常卡顿，很多客户反馈，测试了一下，卡顿的主要是11.0版本以上的；

去issues上面查了一下，有人讲是因为table里面使用overflow:hidden; 导致的，于是去除node\_modules中antd table中所有的overflow:hidden; 解决了这个问题；



参考地址： [https://github.com/ant-design/ant-design/issues/7799](https://github.com/ant-design/ant-design/issues/7799);

解决问题作者：[https://github.com/ant-design/ant-design/issues/8538](https://github.com/ant-design/ant-design/issues/8538)

