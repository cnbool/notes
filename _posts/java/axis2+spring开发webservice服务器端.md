title: axis2+spring开发webservice服务器端
id: 955
categories:
  - Webservice
date: 2013-07-23 00:34:56
tags:
---

需求:开发VAC与SP间订购通知接口服务器端(SP端),给定VacSyncService_SPClient.wsdl文件

首先,官网下载axis2-1.6.2-bin.zip和axis2-1.6.2-war.zip
axis2-1.6.2-bin.zip包含axis2的jar包,工具和例子
axis2-1.6.2-war.zip包含了axis2的web应用,发布web服务时,将自己的程序以特定文件结构发布到axis2的web应用的service目录中
<!--more-->
**1.根据wsdl生成服务器端代码**
解压axis2-1.6.2-bin.zip, cmd命令行进入到axis2-1.6.2\bin:

```shell
cd D:\axis2\axis2-1.6.2\bin
```

假设wsdl文件存放于D盘根目录,将服务器端代码生成到D:\gen_code目录:

```shell
WSDL2Java -uri D:\VacSyncService_SPClient.wsdl -p com.example -s -ss -sd -ssi -o d:\gen_code
```

参数说明:

```shell
　　-o : 指定生成代码的输出路径
　　-a : 生成异步模式的代码
　　-s : 生成同步模式的代码
　　-p : 指定代码的package名称
　　-l : 使用的语言(Java/C) 默认是java
　　-t : 为代码生成测试用例
　　-ss : 生成服务端代码 默认不生成
　　-sd : 生成服务描述文件 services.xml,仅与-ss一同使用
　　-d : 指定databingding，例如，adb,xmlbean,jibx,jaxme and jaxbri
　　-g : 生成服务端和客户端的代码
　　-pn : 当WSDL中有多个port时，指定其中一个port
　　-sn : 选择WSDL中的一个service
　　-u : 展开data-binding的类
　　-r : 为代码生成指定一个repository
　　-ssi : 为服务端实现代码生成接口类
　　-S : 为生成的源码指定存储路径
　　-R : 为生成的resources指定存储路径
　　--noBuildXML : 输出中不生成build.xml文件
　　--noWSDL : 在resources目录中不生成WSDL文件
　　--noMessageReceiver : 不生成MessageReceiver类
```

cmd里执行如上命令之后,d:\gen_code会生成如下文件:
/gen_code
/gen_code/resources
/gen_code/resources/services.xml
/gen_code/resources/SyncNotifySPServiceService.wsdl
/gen_code/src
/gen_code/build.xml

**2.根据wsdl生成客户端代码**
cmd命令行进入到axis2-1.6.2bin:
```shell
WSDL2Java -uri D:\myWebService.wsdl -o d:ws_client
```
使用:在代码中调用刚刚生成的以Stub结尾的类,可以测试服务器端

**3.新建maven web项目**
项目的spring配置略...
pom.xml需要添加axis2的依赖
[code lang="xml"]
		<dependency>
			<groupId>org.apache.axis2</groupId>
			<artifactId>axis2-kernel</artifactId>
			<version>1.6.2</version>
		</dependency>
		<dependency>
			<groupId>org.apache.axis2</groupId>
			<artifactId>axis2-adb</artifactId>
			<version>1.6.2</version>
		</dependency>
		<dependency>
			<groupId>org.apache.axis2</groupId>
			<artifactId>axis2-transport-http</artifactId>
			<version>1.6.2</version>
		</dependency>
		<dependency>
			<groupId>org.apache.axis2</groupId>
			<artifactId>axis2-jaxws</artifactId>
			<version>1.6.2</version>
		</dependency>
```

将axis2-1.6.2-war.zip中的war包解压,解压后WEB-INF下的conf,modules,services复制到项目的WEB-INF下
web.xml中的内容复制到web项目的web.xml中,要复制的内容:
[code lang="xml"]
	<servlet>
		<servlet-name>AxisServlet</servlet-name>
		<servlet-class>org.apache.axis2.transport.http.AxisServlet</servlet-class>
		<!--<init-param> -->
		<!--<param-name>axis2.xml.path</param-name> -->
		<!--<param-value>/WEB-INF/conf/axis2.xml</param-value> -->
		<!--<param-name>axis2.xml.url</param-name> -->
		<!--<param-value>http://localhost/myrepo/axis2.xml</param-value> -->
		<!--<param-name>axis2.repository.path</param-name> -->
		<!--<param-value>/WEB-INF</param-value> -->
		<!--<param-name>axis2.repository.url</param-name> -->
		<!--<param-value>http://localhost/myrepo</param-value> -->
		<!--</init-param> -->
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>AxisServlet</servlet-name>
		<url-pattern>/servlet/AxisServlet</url-pattern>
	</servlet-mapping>
	<servlet-mapping>
		<servlet-name>AxisServlet</servlet-name>
		<url-pattern>*.jws</url-pattern>
	</servlet-mapping>
	<servlet-mapping>
		<servlet-name>AxisServlet</servlet-name>
		<url-pattern>/services/*</url-pattern>
	</servlet-mapping>
```

将生成的服务器端和客户端代码复制到项目中,服务器端代码中以Skeleton结尾的类需要修改并添加业务逻辑代码
此类的spring配置
[code lang="xml"]
<bean id="syncNotifySPServiceServiceSkeleton" class="com.xxx.vac.syncservice.SyncNotifySPServiceServiceSkeleton"></bean>
```

在web项目的WEB-INF/services目录下建立自己的webservice目录,如下:
WEB-INF/services/MyWebservice
WEB-INF/services/MyWebservice/META-INF
WEB-INF/services/MyWebservice/META-INF/services.xml

其中的services.xml为之前已生成的/gen_code/resources/services.xml
services.xml内容
[code lang="xml"]
<?xml version="1.0" encoding="UTF-8"?>
<!-- This file was auto-generated from WSDL -->
<!-- by the Apache Axis2 version: 1.6.1  Built on : Aug 31, 2011 (12:22:40 CEST) -->
<serviceGroup>
    <service name="SyncNotifySPServiceService">
        <messageReceivers>
            <messageReceiver mep="http://www.w3.org/ns/wsdl/in-out" class="com.bjhrrh.vac.syncservice.SyncNotifySPServiceServiceMessageReceiverInOut"/>
        </messageReceivers>
        <parameter name="ServiceClass">com.bjhrrh.vac.syncservice.SyncNotifySPServiceServiceSkeleton</parameter>
        <parameter name="useOriginalwsdl">true</parameter>
        <parameter name="modifyUserWSDLPortAddress">true</parameter>
        <operation name="orderRelationUpdateNotify" mep="http://www.w3.org/ns/wsdl/in-out" namespace="http://soap.bossagent.vac.unicom.com">
            <actionMapping>http://soap.bossagent.vac.unicom.com/SyncNotifySPService/orderRelationUpdateNotifyRequest</actionMapping>
            <outputActionMapping>http://soap.bossagent.vac.unicom.com/SyncNotifySPService/orderRelationUpdateNotifyResponse</outputActionMapping>
        </operation>
    </service>
</serviceGroup>
```

修改services.xml为
[code lang="xml" highlight="10,11"]
<?xml version="1.0" encoding="UTF-8"?>
<!-- This file was auto-generated from WSDL -->
<!-- by the Apache Axis2 version: 1.6.1  Built on : Aug 31, 2011 (12:22:40 CEST) -->
<serviceGroup>
    <service name="SyncNotifySPServiceService">
        <messageReceivers>
            <messageReceiver mep="http://www.w3.org/ns/wsdl/in-out" class="com.bjhrrh.vac.syncservice.SyncNotifySPServiceServiceMessageReceiverInOut"/>
        </messageReceivers>
        <parameter name="ServiceClass">com.bjhrrh.vac.syncservice.SyncNotifySPServiceServiceSkeleton</parameter>
        <parameter name="SpringBeanName">syncNotifySPServiceServiceSkeleton</parameter>
        <parameter name="useOriginalwsdl">false</parameter>
        <parameter name="modifyUserWSDLPortAddress">true</parameter>
        <operation name="orderRelationUpdateNotify" mep="http://www.w3.org/ns/wsdl/in-out" namespace="http://soap.bossagent.vac.unicom.com">
            <actionMapping>http://soap.bossagent.vac.unicom.com/SyncNotifySPService/orderRelationUpdateNotifyRequest</actionMapping>
            <outputActionMapping>http://soap.bossagent.vac.unicom.com/SyncNotifySPService/orderRelationUpdateNotifyResponse</outputActionMapping>
        </operation>
    </service>
</serviceGroup>
```
SpringBeanName的值与spring中配置的bean一致
useOriginalwsdl设置为false时,由axis2生成wsdl文件

**4.测试代码**
```java
	public class Test {
	static SyncNotifySPServiceServiceStub service;

	static {
		try {
			service = new SyncNotifySPServiceServiceStub("http://localhost:8080/vacsyncservice/services/SyncNotifySPServiceService");
		} catch (AxisFault e) {
			e.printStackTrace();
		}
	}

	public static void main(String[] args) throws RemoteException {
		System.out.println("begin...");
		OrderRelationUpdateNotify orderRelationUpdateNotify = new OrderRelationUpdateNotify();
		OrderRelationUpdateNotifyRequest param = new OrderRelationUpdateNotifyRequest();
		orderRelationUpdateNotify.setOrderRelationUpdateNotifyRequest(param);
		OrderRelationUpdateNotifyResponseE respE = service.orderRelationUpdateNotify(orderRelationUpdateNotify);
		OrderRelationUpdateNotifyResponse resp = respE.getOrderRelationUpdateNotifyReturn();
		System.out.println("ResultCode:" + resp.getResultCode());
	}
}
```
