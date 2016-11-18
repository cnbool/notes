title: sqlserver版本分类
id: 1296
categories:
  - SqlServer
date: 2014-04-01 01:00:19
tags:
---

源自:[http://fengweipeng1208.blog.163.com/blog/static/21277318020135811436565/](http://fengweipeng1208.blog.163.com/blog/static/21277318020135811436565/)

LocalDB (SqlLocalDB) 
        LocalDB 是 Express 的一种轻型版本，该版本具备所有可编程性功能，但在用户模式下运行，并且具有快速的零配置安装和必备组件要求较少的特点。如果您需要通过简单方式从代码中创建和使用数据库，则可使用此版本。此版本可与 Visual Studio 之类的应用程序和数据库开发工具捆绑在一起，也可以与需要本地数据库的应用程序一起嵌入。

        Express (SQLEXPR)
        Express 版本仅包含 SQL Server 数据库引擎。它最适合需要接受远程连接或以远程方式进行管理的情况。 

        Express with Tools (SQLEXPRWT)
        此包包含将 SQL Server 作为数据库服务器进行安装和配置所需的全部内容，包括 SQL Server 2012 Management Studio SP1 的完整版本。根据您的上述需求来选择 LocalDB 或 Express。

        SQL Server Management Studio Express (SQLManagementStudio)
        此版本不包含数据库，只包含用于管理 SQL Server 实例的工具（包括 LocalDB、SQL Express、SQL Azure、SQL Server 2012 Management Studio SP1 的完整版本等）。如果您拥有数据库且只需要管理工具，则可使用此版本。

        Express with Advanced Services (SQLEXPRADV)
        此包包含 SQL Server Express 的所有组件，包括 SQL Server 2012 Management Studio SP1 的完整版本。此包的下载大小大于“带有工具”的版本，因为它还同时包含“全文搜索”和 Reporting Services。
