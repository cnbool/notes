title: Struts 2 ActionContext
id: 1153
categories:
  - Struts
date: 2013-09-02 23:35:15
tags:
---

源自:<Struts 2权威指南>

为了访问HttpSession实例. Struts 2提供了一个ActionContext类，该类提供了一个
getSession 的方法，但该方法的返回值类型并不是HttpSession. 而是Map。

ActionContext的getSession返回的不是HttpSession对象，但Struts2的系列拦截
器会负责该Session和HttpSession之间的转换。

通过ActionContext对象访问Web应用的Session
ActionContext.getContext().getSession().put("user" , getUsername());

当Action 设置了某个属性值后. Struts 2 将这些属性值全部封装在一个叫做
struts.valueStack的请求属性里。

从数据结构上来看，ValueStack有点类似于Map结构，但它比Map结构更加强大(因
为它可以根据表达式来查询值)Action所有的属性都被封装到了ValueStack对象中，Action
中的属性名可以理解为ValueStack中value的名字。

获取封装输出信息的ValueStack对象
ValueStack vs = (ValueStack)request.getAttribute("struts.valueStack");
调用ValueStack的fineValue方法获取Action中的books属性值
String[] books = (String[])vs.findValue("books");
