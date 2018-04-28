2017年是全面屏爆发的大热潮，18：9屏幕的相拥而来，在使用18：9等非传统16：9的手机之后，部分手机应用出现了上下黑边。

### 解决方案： {#解决方案}

1.设置 Android:resizeableActivity

```
# compileSdkVersion 需要设置为 24以上,不然报错 resizeableActivity 属性不存在
compileSdkVersion : 24

# application 设置 resizeableActivity 属性为 true
<application  
        ...
        android:resizeableActivity="true">
```

这种方案会开启Android N分屏功能，所以需要适配下每个 Activity 自适应高度.

2.设置 Meta-Data:android.Max\_aspect

```
# 设置最大高宽比为 2.1
<application  
        ...>
        <meta-data
            android:name="android.max_aspect"
            android:value="2.1" />

</application>  
```

目前我主要用到第二种方式来适配18：9的全面屏。

