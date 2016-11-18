title: maven可执行jar打包,生成zip包
id: 1391
categories:
  - Maven
date: 2014-05-05 21:00:10
tags:
---

使用maven-jar-plugin插件生成可执行jar包
使用maven-assembly-plugin插件打包
<!--more-->
在pom.xml文件中添加
[code lang="xml" highlight="12,23"]
<build>
	<plugins>
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-jar-plugin</artifactId>
			<version>2.4</version>
			<configuration>
				<archive>
					<manifest>
						<addClasspath>true</addClasspath>
						<classpathPrefix>lib/</classpathPrefix>
						<mainClass>com.xxxx.signalingforward.Startup</mainClass>
					</manifest>
				</archive>
			</configuration>
		</plugin>
		<plugin>
			<artifactId>maven-assembly-plugin</artifactId>
			<configuration>
				<!-- not append assembly id in release file name -->
				<appendAssemblyId>false</appendAssemblyId>
				<descriptors>
					<descriptor>src/main/assemble/package.xml</descriptor>
				</descriptors>
			</configuration>
			<executions>
				<execution>
					<id>make-assembly</id>
					<phase>package</phase>
					<goals>
						<goal>single</goal>
					</goals>
				</execution>
			</executions>
		</plugin>
	</plugins>
</build>
```

在src/main/下建立assemble文件夹
新建package.xml,内容:
[code lang="xml"]
<assembly
	xmlns="http://maven.apache.org/plugins/maven-assembly-plugin/assembly/1.1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/plugins/maven-assembly-plugin/assembly/1.1.0 http://maven.apache.org/xsd/assembly-1.1.0.xsd">
	<id>bin</id>
	<formats>
		<format>zip</format>
	</formats>
	<fileSets>
		<fileSet>
			<directory>src/main/config</directory>
			<outputDirectory>/config</outputDirectory>
		</fileSet>

		<fileSet>
			<directory>${project.basedir}</directory>
			<outputDirectory>/</outputDirectory>
			<includes>
				<include>README*</include>
				<include>run*</include>
			</includes>
		</fileSet>
		<fileSet>
			<directory>${project.build.directory}</directory>
			<outputDirectory>/</outputDirectory>
			<includes>
				<include>*.jar</include>
			</includes>
		</fileSet>
	</fileSets>
	<dependencySets>
		<dependencySet>
			<outputDirectory>lib</outputDirectory>
			<scope>runtime</scope>
		</dependencySet>
	</dependencySets>
</assembly>
```
这样就将可执行jar包打包成zip格式,例如SignalingForward-0.0.1.zip,目录结构:
/config
/lib
/run.cmd
/SignalingForward-0.0.1.jar
