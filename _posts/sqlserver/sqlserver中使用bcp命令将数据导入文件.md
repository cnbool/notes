title: sqlserver中使用bcp命令将数据导入文件
categories:
  - SqlServer
date: 2016-01-19 17:11:56
tags:
---

bcp命令:
```shell
bcp {dbtable | query} {in | out | queryout | format} 数据文件
```

在数据库中执行bcp命令：
```sql
EXEC master..xp_cmdshell '这里是bcp命令'
```

报错：
SQL Server 阻止了对组件 'xp_cmdshell' 的 过程 'sys.xp_cmdshell' 的访问，因为此组件已作为此服务器安全配置的一部分而被关闭。
系统管理员可以通过使用 sp_configure 启用 'xp_cmdshell'。
有关启用 'xp_cmdshell' 的详细信息，请参阅 SQL Server 联机丛书中的 "外围应用配置器"。

解决方法:
```sql
-- 允许配置高级选项
EXEC sp_configure 'show advanced options', 1
GO
-- 重新配置
RECONFIGURE
GO
-- 启用xp_cmdshell
EXEC sp_configure 'xp_cmdshell', 1
GO
--重新配置
RECONFIGURE
GO
```

例子：
1.整个表导出
```sql
--混合模式身份验证
EXEC master..xp_cmdshell 'bcp lh.dbo.T_HD out D:/hd.dat -c -S127.0.0.1 -Usa -Pmypwd'
  
--使用可信连接
EXEC master..xp_cmdshell 'bcp lh.dbo.T_HD out D:/hd.dat -c -T'
```

2.指定查询语句
```sql
--混合模式身份验证
EXEC master..xp_cmdshell 'bcp "select top 1000 * from lh.dbo.T_HD" queryout D:/hd_1000.dat -c -S127.0.0.1 -Usa -Pmypwd'

--使用可信连接
EXEC master..xp_cmdshell 'bcp "select top 1000 * from lh.dbo.T_HD" queryout D:/hd_1000.dat -c -T'
```
