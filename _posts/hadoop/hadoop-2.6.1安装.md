title: hadoop-2.6.1安装
categories:
- Hadoop
date: 2015-11-23 21:35:22
---

## 1.安装JDK 7

### 1.1新建目录
```shell
mkdir /usr/local/jdk/
```
将jdk1.7.0_79.tar.gz解压到此目录,目录结构
/usr/local/jdk/jdk1.7.0_79

### 1.2全局变量配置
vi /etc/profile 添加如下配置
```
export JAVA_HOME=/usr/local/jdk/jdk1.7.0_79
export PATH=$JAVA_HOME/bin:$PATH
```

使全局变量生效
```
source /etc/profile
```

## 2.SSH免密码登录

### 2.1启用公钥验证
CentOS启动ssh无密登录，/etc/ssh/sshd_config以下配置注释去掉:
```
#RSAAuthentication yes
#PubkeyAuthentication yes
```

如果没有安装ssh客户端,安装:
```
yum install openssh-clients
```

### 2.2.生成公钥
在各台机器上,使用root账号执行如下命令
```
ssh-keygen -t rsa
```
此时/root/.ssh/中生成公钥文件id_rsa.pub
```
cat id_rsa.pub>> authorized_keys
```
将其他机器的公钥也添加到这个文件中
```
ssh root@192.168.1.200 cat ~/.ssh/id_rsa.pub>> authorized_keys
```
### 2.3
把Master服务器的authorized_keys、known_hosts复制到Slave服务器的/root/.ssh目录
测试ssh root@192.168.1.100、ssh root@192.168.1.200 是否需要密码

## 4.hadoop安装
创建数据存放的文件夹
/home/hadoop
/home/hadoop/tmp
/home/hadoop/hdfs
/home/hadoop/hdfs/data
/home/hadoop/hdfs/name

## 5.创建目录
```
mkdir /usr/local/hadoop
```
hadoop-2.6.1.tar.gz解压至/usr/local/hadoop/下


## 6.配置各机器hosts
vi /etc/hosts
ip	机器名	
ip	机器名	

## 7.配置hadoop参数
### 7.1配置hadoop-2.6.1/etc/hadoop/core-site.xml
```xml
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://192.168.1.100:9000</value>
    </property>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>file:/home/hadoop/tmp</value>
    </property>
    <property>
        <name>io.file.buffer.size</name>
        <value>131702</value>
    </property>
</configuration>
```

### 7.2配置hadoop-2.6.1/etc/hadoop/hdfs-site.xml
```xml
<configuration>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>file:/home/hadoop/hdfs/name</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>file:/home/hadoop/hdfs/data</value>
    </property>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
        <name>dfs.namenode.secondary.http-address</name>
        <value>192.168.1.100:9001</value>
    </property>
    <property>
    <name>dfs.webhdfs.enabled</name>
    <value>true</value>
    </property>
</configuration>
```

### 7.3配置hadoop-2.6.1/etc/hadoop/mapred-site.xml
```xml
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
    <property>
        <name>mapreduce.jobhistory.address</name>
        <value>192.168.1.100:10020</value>
    </property>
    <property>
        <name>mapreduce.jobhistory.webapp.address</name>
        <value>192.168.1.100:19888</value>
    </property>
</configuration>
```

### 7.4配置hadoop-2.6.1/etc/hadoop/yarn-site.xml
```xml
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <name>yarn.nodemanager.auxservices.mapreduce.shuffle.class</name>
        <value>org.apache.hadoop.mapred.ShuffleHandler</value>
    </property>
    <property>
        <name>yarn.resourcemanager.address</name>
        <value>192.168.1.100:8032</value>
    </property>
    <property>
        <name>yarn.resourcemanager.scheduler.address</name>
        <value>192.168.1.100:8030</value>
    </property>
    <property>
        <name>yarn.resourcemanager.resource-tracker.address</name>
        <value>192.168.1.100:8031</value>
    </property>
    <property>
        <name>yarn.resourcemanager.admin.address</name>
        <value>192.168.1.100:8033</value>
    </property>
    <property>
        <name>yarn.resourcemanager.webapp.address</name>
        <value>192.168.1.100:8088</value>
    </property>
    <property>
        <name>yarn.nodemanager.resource.memory-mb</name>
        <value>22528</value>
    </property>
</configuration>
```

### 7.5配置hadoop-2.6.1/etc/hadoop/hadoop-env.sh和hadoop-2.6.1/etc/hadoop/yarn-env.sh
```
export JAVA_HOME=/usr/local/jdk/jdk1.7.0_79
```

### 7.6配置slaves	添加从节点
vi hadoop-2.6.1/etc/hadoop/slaves

## 8.命令
初始化
```
bin/hdfs namenode -format
```

```
sbin/start-dfs.sh
sbin/start-yarn.sh

sbin/stop-dfs.sh
sbin/stop-yarn.sh
```
输入命令jps可以看到相关信息

执行jar
```
hadoop jar xxxxx.jar arg1 arg2 
```

** hdfs命令 **
列出目录下文件
```
hadoop fs -ls /
```

创建目录
```
hadoop fs -mkdir /newdir
```

本地文件复制到HDFS
```
hadoop fs -copyFromLocal /home/input/a.txt /input/a.txt
```

HDFS文件复制到本地
```
hadoop fs -copyToLocal /input/a.txt /home/input/a.txt
```

删除HDFS目录及其中文件
```
 hadoop fs -rm -f -r /output1
```

移动文件
```
hadoop fs -mv URI [URI …] <dest>
```

停止job
```
hadoop job -kill <id>
```

关闭安全模式
```
hadoop dfsadmin -safemode leave 
```

Cluster查看
http://192.168.1.100:8088/

HDFS查看
http://192.168.1.100:50070/

## 9.错误
### 初始化报错
host = java.net.UnknownHostException: centos: centos

查看/etc/sysconfig/network文件
```
NETWORKING=yes
HOSTNAME=centos
```

HOSTNAME是centos, 无法在/etc/hosts中找到对应IP

vi /etc/hosts,添加:
```
127.0.0.1   centos
```

### 启动报错:
/hadoop-2.6.1/sbin/hadoop-daemon.sh: Permission denied

从节点hadoop目录要有执行权限
```
chmod -R 755 hadoop-2.6.1
```
### ShuffleError

Error: org.apache.hadoop.mapreduce.task.reduce.Shuffle$ShuffleError: error in shuffle in fetcher#3
        at org.apache.hadoop.mapreduce.task.reduce.Shuffle.run(Shuffle.java:134)
        at org.apache.hadoop.mapred.ReduceTask.run(ReduceTask.java:376)
        at org.apache.hadoop.mapred.YarnChild$2.run(YarnChild.java:163)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:415)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1656)
        at org.apache.hadoop.mapred.YarnChild.main(YarnChild.java:158)
Caused by: java.lang.OutOfMemoryError: Java heap space
        at org.apache.hadoop.io.BoundedByteArrayOutputStream.<init>(BoundedByteArrayOutputStream.java:56)
        at org.apache.hadoop.io.BoundedByteArrayOutputStream.<init>(BoundedByteArrayOutputStream.java:46)
        at org.apache.hadoop.mapreduce.task.reduce.InMemoryMapOutput.<init>(InMemoryMapOutput.java:63)
        at org.apache.hadoop.mapreduce.task.reduce.MergeManagerImpl.unconditionalReserve(MergeManagerImpl.java:305)
        at org.apache.hadoop.mapreduce.task.reduce.MergeManagerImpl.reserve(MergeManagerImpl.java:295)
        at org.apache.hadoop.mapreduce.task.reduce.Fetcher.copyMapOutput(Fetcher.java:514)
        at org.apache.hadoop.mapreduce.task.reduce.Fetcher.copyFromHost(Fetcher.java:336)
        at org.apache.hadoop.mapreduce.task.reduce.Fetcher.run(Fetcher.java:193)
      
解决方法:在mapred-size.xml添加配置
```  
		<property>
        <name>mapreduce.reduce.shuffle.memory.limit.percent</name>
        <value>0.10</value>
    </property>
```


15/11/30 20:15:46 INFO hdfs.DFSClient: Exception in createBlockOutputStream
java.io.IOException: Bad connect ack with firstBadLink as 192.168.1.200:50010
        at org.apache.hadoop.hdfs.DFSOutputStream$DataStreamer.createBlockOutputStream(DFSOutputStream.java:1460)
        at org.apache.hadoop.hdfs.DFSOutputStream$DataStreamer.nextBlockOutputStream(DFSOutputStream.java:1361)
        at org.apache.hadoop.hdfs.DFSOutputStream$DataStreamer.run(DFSOutputStream.java:588)

解决方法:打开192.168.1.200 50010端口防火墙