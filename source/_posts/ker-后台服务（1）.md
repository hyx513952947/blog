title: daker 后台服务（1）
author: 大帅
date: 2018-08-22 14:57:45
tags:
---
## 开始使用 SpringBoot2.0 

> 开始使用SpringBoot2.0作为daker的后台服务，暂时先把后台服务先写了，之后再快速将Android端完成，完善之后再进行小程序的开发

1.  平台使用的ecliplse neno ，工程构建工具使用的maven，gradle不太会用，啊哈哈哈哈哈，尴尬的😀
	- 按照正常网上的流程，ecliplse集成maven工具与SpringBoot的工具套件。
   - 创建工程、根据提示配置工程名称、构建方式、打包格式、java版本等信息
   - 集成工程依赖
   		其中上面的几个都是应该跟微服务有关的，暂时应该先不管，先学会基础的就好
     
2. 依赖库
> 构建一个完整的web项目中，功能模块对应依赖的库

	 - MyBatis 
     
     	  MyBatis 是一款优秀的持久层框架，它支持定制化 SQL、存储过程以及高级映射。MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。MyBatis 可以使用简单的 XML 或注解来配置和映射原生信息，将接口和 Java 的 POJOs(Plain Old Java Objects,普通的 Java对象)映射成数据库中的记录。
          
   - redis 
   		是一个由Salvatore Sanfilippo写的key-value存储系统。
   - devtoosl
   		　spring为开发者提供了一个名为spring-boot-devtools的模块来使Spring Boot应用支持热部署，提高开发者的开发效率，无需手动重启Spring Boot应用。
   - Validation
   		提供了一系列验证各种参数的方法
   - retry
   		第三方接口或者使用mq时，会出现网络抖动，连接超时等网络异常，所以需要重试
	 
 - Aspect
    		常用用于实现拦截的有：Filter、HandlerInterceptor、MethodInterceptor
            HandlerInterceptoer拦截的是请求地址，所以针对请求地址做一些验证、预处理等操作比较合适。当你需要统计请求的响应时间时MethodInterceptor将不太容易做到，因为它可能跨越很多方法或者只涉及到已经定义好的方法中一部分代码。 
			MethodInterceptor利用的是AOP的实现机制，在本文中只说明了使用方式，关于原理和机制方面介绍的比较少，因为要说清楚这些需要讲出AOP的相当一部分内容。在对一些普通的方法上的拦截HandlerInterceptoer就无能为力了，这时候只能利用AOP的MethodInterceptor。 
			Filter是Servlet规范规定的，不属于spring框架，也是用于请求的拦截。但是它适合更粗粒度的拦截，在请求前后做一些编解码处理、日志记录等。
    
   - Batch
   	  Spring Batch 是一个轻量级的、完善的批处理框架,旨在帮助企业建立健壮、高效的批处理应用。Spring Batch是Spring的一个子项目,使用Java语言并基于Spring框架为基础开发,使的已经使用 Spring 框架的开发者或者企业更容易访问和利用企业服务。
   - mail
   	  发送邮件
      
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
