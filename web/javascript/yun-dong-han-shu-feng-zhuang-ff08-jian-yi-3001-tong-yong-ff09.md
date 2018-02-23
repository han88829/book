```
// JavaScript Document
/*缓冲运动的封装，适用于各种运动，支持透明度*/

//获取元素的某一个属性
function getStyle(obj,attr){	
	return obj.currentStyle?obj.currentStyle[attr]:getComputedStyle(obj,false)[attr];
};

function bufferMove(obj,attr,iTarget,endFn){
	
	clearInterval(obj.timer);
	obj.timer = setInterval(function (){
		var iCur = 0;
		
		if(attr == opacity){
			
			iCur = parseInt(parseFloat(getStyle(obj,attr))*100);//将里面的小数转成整数；
			
		}else{
			
			iCur = parseInt(getStyle(obj,attr));//把元素的属性作为值存到iCur中
		};

		var iSpeed = (iTarget - iCur)/8 ;/*//定义一个变化的速度*/
		iSpeed = iSpeed>0?Math.ceil(iSpeed):Math.floor(iSpeed);//用不同的取整来调节速度，以免在0附近运动速度错误
		if(iTarget == iCur){
			
			clearInterval(obj.timer);
			endFn && endFn();//在运动结束添加函数
			
		}else{
			if(attr == opacity){
				
				obj.style.filter = 'alpha(opacity:'+(iCur + iSpeed)+')';
				obj.style[attr] = (iCur + iSpeed)/100 ;
			}else{
				obj.style[attr] = iSpeed + iCur + 'px';
			}
			
		}
		
	},30);
	
};
```



