title: Struts 2 国际化
id: 1158
categories:
  - Struts
date: 2013-09-03 01:01:55
tags:
---

中文语言环境资源文件的代码

[code lang="text"]
loginPage= 登录页面
errorPage= 错误页面
succPage= 成功页面'
failTip=对不起，您不能登录!
succTip= 欢迎， ${O}，您已经登录!
viewLink=查看作者李刚已出版的图书
bookPageTitle=作者李刚已出版的图书
bookName=书名:
user=用户名
pass=密码
login=登录
```

因为该资源文件中包含了非西欧字符，因此必须使用native2ascii命令来处理该文件。
将上面文件保存在WEB-INF/classes路径下，文件名为"messageResource.properties"。
保存该文件后，必须使用native2ascii命令来处理该文件，处理该文件的命令格式为:

[code lang="text"]
native2ascii messageResource.properties messageResource_zh_CN.properties
```
<!--more-->
上面命令将包含非西欧字符的资源文件处理成标准的ASCII格式，处理完成后生成了一份新文件:messageResource_zh_CN.properties文件。
这个文件的文件名符合资源文件的命名格式，资源文件的文件名命名格式为:
**basename_语言代码-国家代码.properties**

英文语言环境的资源文件

[code lang="text"]
loginPage=Login Page
errorPage=Error Page
succPage=Welcome Page
failTip=Sorry, You can't log in!
succTip=welcome, {O} , you l:l as ;!, ogg al d
viewLink=View LiGang's Books
bookPageTitle=LiGang's Books
bookName=BookName:
user=User Name
pass=User Pass
login=Login
```

将上面资源文件保存在WEB-INF/classes 路径下，文件名为"messageResource_en_US.properties"。
当请求来自美国时，系统自动使用这份资源文件的内容输出。

使用全局属性加载Struts2国际化资源文件。
加载资源文件可以通过struts.properties文件来定义，本应用的struts.properties文件仅有如下一行代码:
定义Struts 2的资源文件的baseName是messageResource

[code lang="text"]
struts.custom.i18n.resources=messageResource
```

为了让程序可以显示国际化信息，则需要在JSP页面中输出key，而不是直接输出字符串常量。
Struts 2提供了如下两种方式来输出国际化信息:

• <s:textname="messageKey"/>: 使用s:text标签来输出国际化信息。
• <s:propertyvalue="%(getText("messageKey") }"/>:使用表达式方式输出国际化信息。
