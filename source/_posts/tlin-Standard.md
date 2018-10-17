title: kotlin -Standard
author: 大帅
date: 2018-10-12 15:08:14
tags:
---
	fun AppCompatActivity.setupActionBar(@IdRes toolbarId: Int, action: ActionBar.() -> Unit) {
        setSupportActionBar(findViewById(toolbarId))
      		supportActionBar?.run {
        	     	action()
   			}
	}

其中的action:ActionBar.()->Unit
Unit相当于Java的void，意思是该函数返回为空，主体参数是ActionBar类型的，附加参数为空？
