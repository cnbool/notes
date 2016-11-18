title: spring mvc编码设置
categories:
  - spring
date: 2014-07-29 19:39:22
tags:
---

在web.xml里添加filter来指定解析表单提交信息时采用的编码

```xml
<filter>
	<filter-name>CharacterEncodingFilter</filter-name>
	<filter-class>
		org.springframework.web.filter.CharacterEncodingFilter
	</filter-class>
	<init-param>
		<param-name>encoding</param-name>
		<param-value>utf-8</param-value>
	</init-param>
</filter>

<filter-mapping>
	<filter-name>CharacterEncodingFilter</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>
```
