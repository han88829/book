css3新增的背景颜色渐变

```
background: linear-gradient(to bottom, #000000 0%, #5788fe 100%);
```



css3动画（透明度变化）

```
      #filter {
            width: 100%;
            height: 100%;
            position: absolute;
            top: 0;
            left: 0;
            background: #fe5757;
            //animation-iteration-count用以指定动画重复的次数，
            //仅仅使用该属性就能使动画重复播放。在该例中，设该属性为infinite以使动画无限重复
            animation: colorChange 10s ease-in-out infinite;
            animation-fill-mode: both;//指定动画执行前后如何为目标元素应用样式
            mix-blend-mode: overlay;
        }

        @keyframes colorChange {
            0%,
            100% {
                opacity: 0;
            }
            50% {
                opacity: .9;
            }
        }
```



