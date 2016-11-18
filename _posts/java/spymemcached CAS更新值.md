title: spymemcached CAS更新值
id: 1417
categories:
  - Java
date: 2014-05-26 02:34:06
tags:
---

Memcached在1.2.4版本新增了CAS(Check and Set)协议,用于解决并发下修改键值导致的问题.

使用spymemcached客户端:
pom.xml添加:
[code lang="xml"]
<dependency>
	<groupId>net.spy</groupId>
	<artifactId>spymemcached</artifactId>
	<version>2.11.0</version>
</dependency>
```
spring配置
[code lang="xml"]
<bean name="memcachedClient" class="net.spy.memcached.MemcachedClient">
	<constructor-arg>
		<list>
			<bean class="java.net.InetSocketAddress">
				<constructor-arg value="127.0.0.1"></constructor-arg>
				<constructor-arg value="11211" type="int"></constructor-arg>
			</bean>
			<bean class="java.net.InetSocketAddress">
				<constructor-arg value="127.0.0.1"></constructor-arg>
				<constructor-arg value="11212" type="int"></constructor-arg>
			</bean>
		</list>
	</constructor-arg>
</bean>
```

```java
String key = "name";
CASValue<Object> casVal = memcachedClient.gets(key);
if (casVal != null) {
	String val = (String) casVal.getValue();
	val = val + "lalala"
	OperationFuture<CASResponse> future = memcachedClient.asyncCAS(key, casVal.getCas(), 3600, val);
}
```
