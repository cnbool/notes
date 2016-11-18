title: Spring JDBC 接口学习
categories:
  - spring
date: 2012-08-31 03:21:11
tags:
---

(本文内容源自http://lsy.iteye.com/blog/85651)

1.org.springframework.jdbc.core.RowMapper
用于处理查询结果，获得ResultSet对象里的数据，把每一行的数据放在一个DTO对象里，然后由JdbcTemplate对象把所有DTO对象放在一个List

```java
	class UserRowMapper implements RowMapper {
	    public Object mapRow(ResultSet rs, int index) throws SQLException {
	        User user = new User();
	        user.setId(rs.getString(user_id));
	        user.setName(rs.getString(name));
	        user.setSex(rs.getString(sex).charAt(0));
	        user.setAge(rs.getInt(age));
	        return user;
	    }
	}
	String sql = ;SELECT * FROM USER&;;
	jdbcTemplate.query(sql, new RowMapperResultReader(new UserRowMapper()));
```

2.org.springframework.jdbc.core.ResultSetExtractor
需要执行ResultSet.next()方法

```java
ResultSetExtractor rse = new ResultSetExtractor(){
    public Object extractData(ResultSet rs) throws SQLException, DataAccessException {
　　　　　　　　　List list = new ArrayList();
　　　　　　　　　while(rs.next()) {
　　　　　　　　　　　　list.add(new String[]{rs.getString(user_id), rs.getString(name)});
　　　　　　　　　}
　　　　　　　　　return list;
　　}
};
```

3.org.springframework.jdbc.core.RowCallbackHandler
用于处理查询结果，获得ResultSet对象里的数据

```java
final User user = new User();
jdbcTemplate.query(;SELECT * FROM USER WHERE user_id = ?,
                    new Object[] {id},
                    new RowCallbackHandler() {
                        public void processRow(ResultSet rs) throws SQLException {
                            user.setId(rs.getString(user_id));
                            user.setName(rs.getString(name));
                            user.setSex(rs.getString(sex).charAt(0));
                            user.setAge(rs.getInt(age));
                        }
                    });
```

4.接口org.springframework.jdbc.core.PreparedStatementSetter
用于PrepraredStatement对象动态设置参数，PrepraredStatement对象由JdbcTemplate对象创建

```java
jdbcTemplate.update(;INSERT INTO USER VALUES(?, ?, ?, ?),
                     new PreparedStatementSetter() {
                         public void setValues(PreparedStatement ps) throws SQLException {
                             ps.setString(1, id);
                             ps.setString(2, name);
                             ps.setString(3, sex);
                             ps.setInt(4, age);
                         }
                     });
```

5.org.springframework.jdbc.core.PreparedStatementCreator
用JdbcTemplate提供的Connection创建PreparedStatement对象，子类需要提供SQL以及为PreparedStatement对象设置必要的参数

```java
PreparedStatementCreator psc = new PreparedStatementCreator(){
    public PreparedStatement createPreparedStatement(Connection con) throws SQLException {
　　　　　　　　　PreparedStatement pstmt = con.prepareStatement(select * from user where name=? and age=?);
　　　　　　　　　pstmt.setString(1, "lsy");
                                pstmt.setInt(2, 24);
　　　　　　　　　return pstmt;
　　}  
};
```
