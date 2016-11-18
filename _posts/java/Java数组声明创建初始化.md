title: Java数组声明创建初始化
id: 115
categories:
  - Java
date: 2012-09-02 04:31:38
tags:
---

_(本文转自[http://developer.51cto.com/art/200906/128274.htm](http://developer.51cto.com/art/200906/128274.htm))_

初始化：

1.动态初始化：数组定义与为数组分配空间和赋值的操作分开进行；
2.静态初始化：在定义数字的同时就为数组元素分配空间并赋值；
3.默认初始化：数组是引用类型，它的元素相当于类的成员变量，因此数组分配空间后，每个元素也被按照成员变量的规则被隐士初始化。<!--more-->

例子:
1.动态初始化

```java
public class TestD
{
     public static void main(String args[]) {
         int a[] ;
         a = new int[3] ;
         a[0] = 0 ;
         a[1] = 1 ;
         a[2] = 2 ;
         Date days[] ;
         days = new Date[3] ;
         days[0] = new Date(2008,4,5) ;
         days[1] = new Date(2008,2,31) ;
         days[2] = new Date(2008,4,4) ;
     }
}

class Date
{
     int year,month,day ;
     Date(int year ,int month ,int day) {
         this.year = year ;
         this.month = month ;
         this.day = day ;
     }
}
```

2.静态初始化

```java
public class TestS
{
     public static void main(String args[]) {
         int a[] = {0,1,2} ;
         Time times [] = {new Time(19,42,42),new Time(1,23,54),new Time(5,3,2)} ;
     }
}

class Time
{
     int hour,min,sec ;
     Time(int hour ,int min ,int sec) {
         this.hour = hour ;
         this.min = min ;
         this.sec = sec ;
     }
}
```

3.默认初始化

```java
public class TestDefault
{
     public static void main(String args[]) {
         int a [] = new int [5] ;
         System.out.println("" + a[3]) ;
     }
}
```
