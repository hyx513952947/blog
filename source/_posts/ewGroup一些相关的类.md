title: ViewGroup一些相关的类
author: 大帅
date: 2018-06-26 15:39:51
tags:
---
- setWillNotDraw()
	
      在viewgroup初始化的时候，它调用了一个私有方法：initViewGroup --> setFlags（WILLL_NOT_DRAW,DRAW_MASK)，不会调用ondraw（）方法，自定义viewgroup时候需要setWillNotDraw(false)；

- descendantfocusability
		viewgroup控件在代码中或者xml中能设置该属性：
        FOCUS_BEFORE_DESCENDANTS ViewGroup本身先对焦点进行处理，如果没有处理则分发给child View进行处理
		FOCUS_AFTER_DESCENDANTS 先分发给Child View进行处理，如果所有的Child View都没有处理，则自己再处理
		FOCUS_BLOCK_DESCENDANTS ViewGroup本身进行处理，不管是否处理成功，都不会分发给ChildView进行处理
        
- ViewCompat 
		这个是解决版本兼容性的
        
        
- ViewPager
		内部的view根据位置绘制的，缓存多少个就在相邻位置绘制多少个