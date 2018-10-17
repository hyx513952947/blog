title: springboot 数据库
author: 大帅
date: 2018-08-23 10:30:31
tags:
---
### 数据库相关配置
application.properties 文件中：
			
    spring.datasource.url=jdbc:mysql://127.0.0.1:3306/jpa
	spring.datasource.username= root
	spring.datasource.password= qwer1234
	spring.datasource.driver-class-name=com.mysql.jdbc.Driver
	spring.jpa.database=mysql
	spring.jpa.show-sql=true
	spring.jpa.hibernate.ddl-auto=update
https://blog.csdn.net/fxbin123/article/details/80387668