title: Spring 构造函数注入
categories:
  - spring
date: 2012-09-08 03:28:27
tags:
---

```java
package com.home;

public class User {
    private int age;
    private String country;

    User(int age, String country)
    {
        this.age=age;
        this.country=country;
    }
}
```

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="user" class="com.home.User" >
        <constructor-arg index="0" type="int" value="24"/>
        <constructor-arg index="1" type="java.lang.String" value="China"/>
    </bean>

</beans>
```
