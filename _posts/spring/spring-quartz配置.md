title: spring quartz配置
categories:
  - spring
date: 2014-03-27 21:18:06
tags:
---

applicationContext-quartz.xml配置文件
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:jee="http://www.springframework.org/schema/jee"
	xmlns:tx="http://www.springframework.org/schema/tx" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-2.5.xsd 
		http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-2.5.xsd 
		http://www.springframework.org/schema/jee http://www.springframework.org/schema/jee/spring-jee-2.5.xsd 
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-2.5.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-2.5.xsd"
	default-autowire="no" default-lazy-init="true">
	<bean name="quartzScheduler"
		class="org.springframework.scheduling.quartz.SchedulerFactoryBean">
		<property name="triggers">
			<list>
				<ref bean="soundPlayCountUpdateTrigger" />
			</list>
		</property>
	</bean>
	<bean id="soundPlayCountUpdateTrigger" class="org.springframework.scheduling.quartz.CronTriggerBean">
		<property name="jobDetail" ref="soundPlayCountUpdate" />
		<property name="cronExpression" value="0 0/1 * * * ?" />
	</bean>
	<bean id="soundPlayCountUpdate"
		class="org.springframework.scheduling.quartz.MethodInvokingJobDetailFactoryBean">
		<property name="targetObject" ref="soundService" />
		<property name="targetMethod" value="updatePlayCount" />
		<property name="concurrent" value="false" />
	</bean>
</beans>
```

如果MethodInvokingJobDetailFactoryBean的targetObject属性的值不是引用在xml中配置的bean,而是引用自动装配的bean时,
需要将applicationContext-quartz.xml的default-autowire="no"
