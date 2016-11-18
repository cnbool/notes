title: Struts 2与Struts 1对比
id: 1143
categories:
  - Struts
date: 2013-09-01 21:58:59
tags:
---

源自:<Struts 2权威指南>

*   Struts 1 Action必须继承抽象基类; Struts 2 Action可以是一个包含execute方法的POJO
*   Struts 1 Action是单例模式,仅有Action的一个实例处理所以请求,Action资源必须是线程安全或同步的; Struts 2为每个请求产生一个实例
*   Struts 1依赖于Servlet API,Struts 1 Action execute方法中有HttpServletRequest和HttpServletResponse方法; Struts 2不依赖Servlet API
*   Struts 1 使用Action Form封装用户请求,所以的Action Form必须继承ActionForm类; Struts 2使用Action属性封装用户请求参数,也可使用单独的Model对象封装请求参数
