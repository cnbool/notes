title: maven的安装和使用
id: 1305
categories:
  - Maven
date: 2014-04-09 18:29:12
tags:
---

maven主页:http://maven.apache.org/

1.安装java运行环境

2.下载maven最新稳定版本(此时最新稳定版3.2.1),解压后目录E:apache-maven-3.2.1

3.添加环境变量 变量名:M2_HOME 变量值:E:apache-maven-3.2.1

4.PATH系统变量修改 PATH变量末尾添加:%M2_HOME%bin;

5.测试 打开cmd输入:mvn -v

* * *

**Maven 安装 JAR 包的命令**

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context-support</artifactId>
      <version>3.1.0.RELEASE</version>
    </dependency>

    mvn install:install-file -Dfile=jar包位置 -DgroupId=org.springframework -DartifactId=spring-context-support -Dversion=3.1.0.RELEASE -Dpackaging=jar
    
