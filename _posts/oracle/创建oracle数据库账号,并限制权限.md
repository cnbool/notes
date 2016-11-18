title: 创建oracle数据库账号,并限制权限
categories:
  - oracle
date: 2013-03-03 07:05:34
---

需求:现需要给外包方提供oracle数据库的账号,将一些表和视图开放给外包方,并且让其可以建立自己的表

**1.首先登录数据库用户user_business,在此用户下创建外包方数据库用户**

```sql
--创建用户，指定默认表空间
create user user_wb identified by pwd_wb 
DEFAULT TABLESPACE TB_DATA;
```

**2.赋予权限**

```sql
GRANT resource, connect to user_wb;

--将table_a的所有操作权限赋予user_wb
GRANT ALL ON table_a TO user_wb;

--将V_GET_MSG视图的查询权限赋予user_wb
GRANT SELECT ON V_GET_MSG TO user_wb;

```

**3.用user_wb账户登录,即可查询table_a和V_GET_MSG**
因为表table_a和视图V_GET_MSG属于用户user_business,所以使用时必须加上前缀user_business

```sql
SELECT * FROM user_business.table_a;
DELETE FROM user_business.table_a;
SELECT * FROM user_business.V_GET_MSG;
```

**4.使用同义词简化语句**
登录user_business

```sql
--为user_business.table_a创建全局同义词table_a
CREATE PUBLIC SYNONYM table_a for user_business.table_a;

--为user_business.V_GET_MSG创建全局同义词V_GET_MSG
CREATE PUBLIC SYNONYM V_GET_MSG for user_business.V_GET_MSG;
```

在user_wb账户下即可使用如下查询

```sql
SELECT * FROM table_a;
DELETE FROM table_a;
SELECT * FROM V_GET_MSG;
```
