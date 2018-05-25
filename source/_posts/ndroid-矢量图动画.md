title: Android 矢量图动画
author: 大帅
date: 2018-05-25 11:30:11
tags:
---
#### 矢量图
[来源](https://blog.csdn.net/zwlove5280/article/details/73442464)

在Android中正常的是：
		  
    f <?xml version="1.0" encoding="utf-8"?>
	<vector xmlns:android="http://schemas.android.com/apk/res/android"
   		...>
        <path/>
	</vector>
    
vector标签中包含路径，动画就需要使用``<group> path... </group>``包裹路径，并对包裹路径的group设置动画属性来控制该标签内的路径变化；
- xml实现动画方式
	
    	<?xml version="1.0" encoding="utf-8"?>
		<animated-vector xmlns:android="http://schemas.android.com/apk/res/android"
                 android:drawable="vector图片">
    		<target
        		android:animation="@animator/动画xml"
        		android:name="要做动画的path、在group中path的定义名称"/>
   	 	<target
      	 	 android:name="要做动画的path"
       	 	android:animation="@animator/smile_morph"/>
		</animated-vector>
    AnimatedVectorDrawable smileDrawable = (AnimatedVectorDrawable) imageView.getDrawable();
                smileDrawable.start();调用

- java代码实现方式


[矢量图动画网站](https://shapeshifter.design/)