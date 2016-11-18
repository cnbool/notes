title: sqlserver bulk insert将文本数据导入数据库
categories:
  - SqlServer
date: 2013-05-05 06:11:44
tags:
---

现需要将文本数据导入sqlserver数据库,使用sqlserver的bulk insert命令,文本中的字段与表的列对应依靠format文件
**表结构:**
```sql
CREATE TABLE [T_Temp] (
	[RecordSequenceID] [char] (18) COLLATE Chinese_PRC_CI_AS NULL ,
	[UserIdType] [int] NULL ,
	[UserId] [varchar] (36) COLLATE Chinese_PRC_CI_AS NULL ,
	[ServiceType] [char] (2) COLLATE Chinese_PRC_CI_AS NULL ,
	[SpId] [varchar] (21) COLLATE Chinese_PRC_CI_AS NULL ,
	[ProductId] [varchar] (21) COLLATE Chinese_PRC_CI_AS NULL ,
	[UpdateType] [int] NULL ,
	[UpdateTime] [varchar] (19) COLLATE Chinese_PRC_CI_AS NULL
);
```

**数据文件D:/data.txt内容:**
```
0001000002013042619000090219700101000000209901010000000000032755                    
00000001	1	861557778	90	11234	9059002200	1	20130426181958	1231000000	0426190000
00000002	1	861557876	90	11234	9059001300	1	20130426181957	1231000000	0426190000
00000003	1	861319757	90	11234	9059002200	1	20130426181957	1231000000	0426190000
00000004	1	861313280	90	11234	9059001300	1	20130426181957	1231000000	0426190000
00000005	1	861321139	90	11234	9059002200	1	20130426181957	1231000000	0426190000
00000006	1	861321159	90	11234	9059002200	1	20130426181957	1231000000	0426190000
00000007	1	861320778	90	11234	9059001300	1	20130426181957	1231000000	0426190000

```
<!--more-->
**bulk insert语句**
```sql
BULK INSERT T_Temp FROM 'D:/data.txt'
WITH
(
	FIRSTROW   =   2,
	FORMATFILE = 'D:/T_Temp.fmt'
)
```
FROM 后跟的是数据文件的位置
FIRSTROW 表示 数据以文本文件的第二行开始
FORMATFILE指定格式化文件的位置

**format文本文件D:/T_Temp.fmt,内容如下:**
```
8.0
9
1       SQLCHAR       0       18      "t"       1     RecordSequenceID     Chinese_PRC_CI_AS
2       SQLCHAR       0       12      "t"       2     UserIdType           ""
3       SQLCHAR       0       36      "t"       3     UserId               Chinese_PRC_CI_AS
4       SQLCHAR       0       2       "t"       4     ServiceType          Chinese_PRC_CI_AS
5       SQLCHAR       0       21      "t"       5     SpId                 Chinese_PRC_CI_AS
6       SQLCHAR       0       21      "t"       6     ProductId            Chinese_PRC_CI_AS
7       SQLCHAR       0       12      "t"       7     UpdateType           ""
8       SQLCHAR       0       14      "t"       8     UpdateTime           Chinese_PRC_CI_AS
9       SQLCHAR       0       300      "n"       0    ExtraField           Chinese_PRC_CI_AS
```
第一行:8.0表示格式化文件的版本
第二行:9表示列数
3-11行: 
第一列:数据文件中字段的次序
第二列:数据的类型
第三列:前缀长度
第四列:数据的长度
第五列:字段的结束符
第六列:数据库表 列顺序
第七列:数据库表 列名
第八列:排序规则
Chinese_PRC_CS_AI_WS 
前半部份：指UNICODE字符集，Chinese_PRC_指针对大陆简体字UNICODE的排序规则。 
排序规则的后半部份即后缀 含义：    
_BIN 二进制排序    
_CI(CS) 是否区分大小写，CI不区分，CS区分   
_AI(AS) 是否区分重音，AI不区分，AS区分      
_KI(KS) 是否区分假名类型,KI不区分，KS区分       
_WI(WS) 是否区分宽度 WI不区分，WS区分  
区分大小写:如果想让比较将大写字母和小写字母视为不等，请选择该选项。 
区分重音:如果想让比较将重音和非重音字母视为不等，请选择该选项。如果选择该选项，比较还将重音不同的字母视为不等。 
区分假名:如果想让比较将片假名和平假名日语音节视为不等，请选择该选项。 
区分宽度:如果想让比较将半角字符和全角字符视为不等，请选择该选项
