title: Android Navigation使用
author: 大帅
date: 2018-07-06 10:23:58
tags:
---
链接地址 https://developer.android.com/topic/libraries/architecture/navigation/navigation-implementing


使用需要在三处地方进行操作：
- navigation目录下的xml文件，该文件是配置路由的配置信息，fragment之间的跳转、传值、deeplink链接
- java代码中进行跳转
- 视图xml中对View配置


###### 1.模块工程的.gradle文件种添加依赖

###### 2. resource资源目录下创建navigation目录保存使用的导航路由配置文件
- 根标签 navigation 各个参数的使用
		startDestination：配置默认展示的fragment id值
        

###### 3. 此框架应该是方便activity对fragment的管理，而不是管理activities之间的跳转管理。
###### 4. 配置标签
- fragment
-