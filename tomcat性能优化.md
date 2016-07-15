---
title: tomcat性能优化
date: 2016-06-02 21:23:50
tags: 服务器
---
# Tomcat性能优化
## 内存

Linux修改TOMCAT_HOME/bin/catalina.sh，在前面加入:

```
JAVA_OPTS="-XX:PermSize=64M -XX:MaxPermSize=128m -Xms512m -Xmx1024m -Duser.timezone=Asia/Shanghai"
```
windows修改TOMCAT_HOME/bin/catalina.bat，在前面加入:

```
set JAVA_OPTS=-XX:PermSize=64M -XX:MaxPermSize=128m -Xms512m -Xmx1024m
```
## 线程
```xml
<Connector port="80" protocol="HTTP/1.1" maxThreads="600" minSpareThreads="100" maxSpareThreads="500" acceptCount="700"
connectionTimeout="20000" redirectPort="8443" />
```
maxThreads="600"       ///最大线程数
minSpareThreads="100"///初始化时创建的线程数
maxSpareThreads="500"///一旦创建的线程超过这个值，Tomcat就会关闭不再需要的socket线程。
acceptCount="700"//指定当所有可以使用的处理请求的线程数都被使用时，可以放到处理队列中的请求数，超过这个数的请求将不予处理
## 禁用DNS查询

当web应用程序向要记录客户端的信息时，它也会记录客户端的IP地址或者通过域名服务器查找机器名 转换为IP地址。
DNS查询需要占用网络，并且包括可能从很多很远的服务器或者不起作用的服务器上去获取对应的IP的过程，这样会消耗一定的时间。
修改server.xml文件中的Connector元素，修改属性enableLookups参数值: enableLookups="false"
如果为true，则可以通过调用request.getRemoteHost()进行DNS查询来得到远程客户端的实际主机名，若为false则不进行DNS查询，而是返回其ip地址

## Apr插件
补充中~



