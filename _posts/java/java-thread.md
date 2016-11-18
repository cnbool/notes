title: java Thread
id: 986
categories:
  - Java
date: 2013-08-12 23:24:38
tags:
---

java线程的状态Thread.State是枚举类型,它的值有:
NEW
至今尚未启动的线程处于这种状态。
RUNNABLE
正在 Java 虚拟机中执行的线程处于这种状态。
BLOCKED
受阻塞并等待某个监视器锁的线程处于这种状态。
WAITING
无限期地等待另一个线程来执行某一特定操作的线程处于这种状态。
TIMED_WAITING
等待另一个线程来执行取决于指定等待时间的操作的线程处于这种状态。
TERMINATED
已退出的线程处于这种状态。

通过Thread.getState()可以得到线程的状态.
<!--more-->
**long与double的操作不是原子的:**
char,int的赋值与引用操作是原子的,是不可分割的:
n = 1;
其他线程中:
n = 2;
这样赋值操作后n的值不是1就是2
而long,double的赋值和引用并非不可分割
longVar = 123L;
其他线程:
longVar = 456L;
longVar的值可能是123L,456L也可能是两者之外的值0L,31419526L.
声明字段时加上volatile关键字可以使对这个字段的操作成为不可分割的.
