title: oracle物化视图
categories:
  - oracle
date: 2013-01-08 04:13:10
tags:
---

有一张短信下发记录表和短信回执表,记录数各有4000w+.两张表需要关联起来查询特别的慢,请刘哥帮忙把回执表的两个字段RECEIPT_RESULT,RESULTTIME更新到下发记录表中,刘哥没有这么干,他使用了"物化视图".

两张表的表结构如下:
T_SMS_AD

```sql
create table T_SMS_AD
(
  SEQUENCEID     VARCHAR2(64),
  PHONENO        VARCHAR2(64),
  SENDTIME       DATE,
  MESSAGECONTENT VARCHAR2(600),
  SUBMIT_RESULT  VARCHAR2(5),
  RECEIPT_RESULT VARCHAR2(64),
  RESULTTIME     DATE
)
```

T_SMS_CB

```sql
create table T_SMS_CB
(
  SEQUENCEID VARCHAR2(64) not null,
  RECEIPT_RESULT VARCHAR2(64),
  RESULTTIME DATE
)
```

使用物化视图:

```sql
CREATE MATERIALIZED VIEW LOG ON T_SMS_AD TABLESPACE AD_MV WITH ROWID;
CREATE MATERIALIZED VIEW LOG ON T_SMS_CB TABLESPACE AD_MV WITH ROWID;

CREATE MATERIALIZED VIEW AD_MV
   PARTITION BY range(SENDTIME)
   INTERVAL(NUMTODSINTERVAL(1,'day')) store in(AD_MV)
   (partition P1 values less than (to_date('2011-11-01','yyyy-mm-dd')))

                  refresh fast with rowid
                  START WITH TO_DATE('2013-01-09 05:00:00', 'YYYY-MM-DD HH24:MI:SS')
                  NEXT SYSDATE + 1

                  AS SELECT /*+ PARALLE */ A.ROWID AD_ROWID, A.SEQUENCEID,
A.PHONENO,A.SENDTIME,A.MESSAGECONTENT,B.RECEIPT_RESULT,B.RESULTTIME,B.ROWID CB_ROWID
FROM T_SMS_AD A, T_SMS_CB B
WHERE A.SEQUENCEID=B.SEQUENCEID
```

说明:
5-6行:物化视图按照SENDTIME字RANGE分区,每一天为一个分区
9行:快速刷新(增量刷新)
10-11行:物化视图第一次刷新时间在2013-01-09 05:00:00,并且以后每天都在05:00:00刷新:

后记:
后来在查询物化视图的时候发现,物化视图中的那句SQL语句不符合需求(13-16)
这个SQL使用的是内连接,如果一条下发没有对应的回执的话,从物化视图中就查不到下发记录了.
所以物化视图中的SQL需要改成左连接.建立新的物化视图,那需要先删除原来的物化视图.
删除物化视图步骤:建议先删VIEW LOG, 再删VIEW,这样删除会比较快

```sql
DROP MATERIALIZED VIEW LOG ON T_SMS_AD;
DROP MATERIALIZED VIEW LOG ON T_SMS_CB;
DROP MATERIALIZED VIEW AD_MV;
```

建立新的物化视图

```
CREATE MATERIALIZED VIEW LOG ON T_SMS_AD TABLESPACE AD_MV WITH ROWID;
CREATE MATERIALIZED VIEW LOG ON T_SMS_CB TABLESPACE AD_MV WITH ROWID;

CREATE MATERIALIZED VIEW AD_MV
   PARTITION BY range(SENDTIME)
   INTERVAL(NUMTODSINTERVAL(1,'day')) store in(AD_MV)
   (partition P1 values less than (to_date('2011-11-01','yyyy-mm-dd')))

                  refresh fast with rowid
                  START WITH TO_DATE('2013-01-09 05:00:00', 'YYYY-MM-DD HH24:MI:SS')
                  NEXT SYSDATE + 1

                  AS SELECT /*+ PARALLE */ A.ROWID AD_ROWID, A.SEQUENCEID,
A.PHONENO,A.SENDTIME,A.MESSAGECONTENT,B.RECEIPT_RESULT,B.RESULTTIME,B.ROWID CB_ROWID
FROM T_SMS_AD A LEFT JOIN T_SMS_CB B
ON A.SEQUENCEID=B.SEQUENCEID
```

执行这段SQL建立新物化视图的时候报错了:
ORA-12015:c cannot create a fast refresh materialized view from a complex query
问了一下刘哥,他说快速刷新模式不允许左连接这样的语句,高亮的那句指定了快速刷新:
只好改成了COMPLETE刷新,修改后的物化视图创建语句:

```sql
CREATE MATERIALIZED VIEW LOG ON T_SMS_AD TABLESPACE AD_MV WITH ROWID;
CREATE MATERIALIZED VIEW LOG ON T_SMS_CB TABLESPACE AD_MV WITH ROWID;

CREATE MATERIALIZED VIEW AD_MV
   PARTITION BY range(SENDTIME)
   INTERVAL(NUMTODSINTERVAL(1,'day')) store in(AD_MV)
   (partition P1 values less than (to_date('2011-11-01','yyyy-mm-dd')))

                  refresh COMPLETE  with rowid
                  START WITH TO_DATE('2013-01-09 05:00:00', 'YYYY-MM-DD HH24:MI:SS')
                  NEXT SYSDATE + 1

                  AS SELECT /*+ PARALLE */ A.ROWID AD_ROWID, A.SEQUENCEID,
A.PHONENO,A.SENDTIME,A.MESSAGECONTENT,B.RECEIPT_RESULT,B.RESULTTIME,B.ROWID CB_ROWID

FROM T_SMS_AD A LEFT JOIN T_SMS_CB B
ON A.SEQUENCEID=B.SEQUENCEID
```

因T_SMS_AD,T_SMS_CB表太大,物化视图COMPLETE刷新慢,还是使用快速刷新(SQL有变化):

```sql
CREATE MATERIALIZED VIEW LOG ON T_SMS_AD TABLESPACE AD_MV WITH ROWID;
CREATE MATERIALIZED VIEW LOG ON T_SMS_CB TABLESPACE AD_MV WITH ROWID;

CREATE MATERIALIZED VIEW AD_MV

  PARTITION  BY range(SENDTIME)
  INTERVAL(NUMTODSINTERVAL(1,'day')) store in(AD_MV)
  (partition P1 values less than (to_date('2011-11-01','yyyy-mm-dd')))

                  REFRESH FAST WITH ROWID
                  START WITH TO_DATE('2013-02-19 05:30:00', 'YYYY-MM-DD HH24:MI:SS')
                  NEXT SYSDATE + 1

                  AS SELECT /*+ PARALLE */ A.ROWID AD_ROWID, A.SEQUENCEID,A.PHONENO,A.SENDTIME,
A.MESSAGECONTENT,B.RECEIPT_RESULT,B.RESULTTIME,B.ROWID CB_ROWID

FROM T_SMS_AD A , T_SMS_CB B
WHERE  A.SEQUENCEID=B.SEQUENCEID(+)
```

后续发现问题:发现物化视图中新增的记录都是重复的, 下发的短信在物化视图中都有两条记录
找到原因:
T_SMS_AD,T_SMS_CB两张表新增记录的时候, 如果先在T_SMS_AD表中插入数据, 后在T_SMS_CB表插入数据
当物化视图快速刷新时,会导致此记录在物化视图中重复,这个和建立物化视图时使用的左连接SQL有关系
