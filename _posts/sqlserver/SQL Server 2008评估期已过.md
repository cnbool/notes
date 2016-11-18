title: SQL Server 2008评估期已过
id: 1734
comment: false
categories:
  - SqlServer
date: 2015-07-14 14:42:25
tags:
---

从数据库中导出数据的时候失败了，提示Integration Services服务评估期已过，找到序列号然后升级一下SQL Server 2008就可以了

具体步骤：
1.查询版本

    select @@version

输出中包含Enterprise Evaluation Edition (64-bit)说明是评估版

1.修改注册表

HKEY_LOCAL_MACHINESOFTWAREMicrosoftMicrosoft SQL Server100ConfigurationState

将CommonFiles的值修改为3

2.Microsoft SQL Server 2008 -> 配置工具 -> SQL Server 安装中心 -> 维护 -> 版本升级 -> 输入产品密钥升级
