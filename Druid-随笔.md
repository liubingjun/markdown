---
title: Druid 随笔
date: 2016-07-15 20:32:12
tags: 服务器
---
# StatViewServlet
Druid内置提供了一个StatViewServlet用于展示Druid的统计信息。  
这个StatViewServlet的用途包括：  
1、提供监控信息展示的html页面  
2、提供监控信息的JSON API
```xml
	<servlet>
		<servlet-name>DruidStatView</servlet-name>
		<servlet-class>com.alibaba.druid.support.http.StatViewServlet</servlet-class>
	</servlet>
	<servlet-mapping>
		<servlet-name>DruidStatView</servlet-name>
		<url-pattern>/druid/*</url-pattern>
	</servlet-mapping>
```  
StatViewServlet是一个标准的javax.servlet.http.HttpServlet，需要配置在你web应用中的WEB-INF/web.xml中。  
根据配置中的url-pattern来访问内置监控页面，如果是上面的配置，内置监控页面的首页是/druid/index.html。

