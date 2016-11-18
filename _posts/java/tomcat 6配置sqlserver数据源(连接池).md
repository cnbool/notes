title: tomcat 6配置sqlserver数据源(连接池)
id: 973
categories:
  - Tomcat
date: 2013-07-30 21:10:55
tags:
---

1.1修改tomcat的conf文件夹下的context.xml配置文件
[code lang="xml" highlight="0,16-22"]
<Context>

    <!-- Default set of monitored resources -->
    <WatchedResource>WEB-INF/web.xml</WatchedResource>

    <!-- Uncomment this to disable session persistence across Tomcat restarts -->
    <!--
    <Manager pathname="" />
    -->

    <!-- Uncomment this to enable Comet connection tacking (provides events
         on session expiration as well as webapp lifecycle) -->
    <!--
    <Valve className="org.apache.catalina.valves.CometConnectionManagerValve" />
    -->
   <Resource name="jdbc/TestDB" 
	   	auth="Container" 
	   	type="javax.sql.DataSource"           
	    maxActive="100" maxIdle="30" maxWait="10000"
	    username="sa" password="1234!@#$" 
	    driverClassName="net.sourceforge.jtds.jdbc.Driver"
	    url="jdbc:jtds:sqlserver://192.168.0.250:1433;databaseName=4test"/>

</Context>
```
<!--more-->
1.2在项目的web.xml中加入资源引用
[code lang="xml"]
  <resource-ref>
      <description>DB Connection</description>
      <res-ref-name>jdbc/TestDB</res-ref-name>
      <res-type>javax.sql.DataSource</res-type>
      <res-auth>Container</res-auth>
  </resource-ref>
```

1.3 spring中使用数据源
[code lang="xml"]
    <bean id="dataSource" class="org.springframework.jndi.JndiObjectFactoryBean">  
        <property name="jndiName">  
            <value>java:comp/env/jdbc/TestDB</value>  
        </property>  
    </bean>  
```
