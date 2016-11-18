title: 使用Tomcat的defaultServlet来处理静态文件
id: 1278
categories:
  - Spring
  - Tomcat
date: 2014-03-14 03:03:13
tags:
---

使用了spring mvc作为mvc框架时,静态资源.css/.js/.jpg请求会被org.springframework.web.servlet.DispatcherServlet拦截.如果想用tomcat的DefaultServlet处理这些请求,需要在项目的xml中做如下配置:
注意:
DefaultServlet的servlet-mapping标签需在DispatcherServlet的servlet-mapping标签之前.
<!--more-->
[code lang="xml" highlight="0,26-49"]
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://java.sun.com/xml/ns/javaee"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	id="WebApp_ID" version="2.5">
	<display-name>ResourceServer</display-name>
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:applicationContext*.xml</param-value>
	</context-param>
	<context-param>
		<param-name>log4jConfigLocation</param-name>
		<param-value>classpath:log4j.properties</param-value>
	</context-param>
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
	<listener>
		<listener-class>org.springframework.web.util.Log4jConfigListener</listener-class>
	</listener>
	<servlet>
		<servlet-name>spring</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>default</servlet-name>
		<url-pattern>*.gif</url-pattern>
	</servlet-mapping>
	<servlet-mapping>
		<servlet-name>default</servlet-name>
		<url-pattern>*.css</url-pattern>
	</servlet-mapping>
	<servlet-mapping>
		<servlet-name>default</servlet-name>
		<url-pattern>*.js</url-pattern>
	</servlet-mapping>
	<servlet-mapping>
		<servlet-name>default</servlet-name>
		<url-pattern>*.png</url-pattern>
	</servlet-mapping>
	<servlet-mapping>
		<servlet-name>default</servlet-name>
		<url-pattern>*.jpg</url-pattern>
	</servlet-mapping>
	<servlet-mapping>
		<servlet-name>default</servlet-name>
		<url-pattern>*.htm</url-pattern>
	</servlet-mapping>
	<servlet-mapping>
		<servlet-name>spring</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
	<welcome-file-list>
		<welcome-file>index.html</welcome-file>
		<welcome-file>index.htm</welcome-file>
		<welcome-file>index.jsp</welcome-file>
		<welcome-file>default.html</welcome-file>
		<welcome-file>default.htm</welcome-file>
		<welcome-file>default.jsp</welcome-file>
	</welcome-file-list>
</web-app>
```
