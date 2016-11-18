title: 'mybatis批量保存'
categories:
  - mybatis
date: 2013-09-09 12:08:38
tags:
---

传入参数为List的时候集合的名称collection="list"
传入参数为数组的时候集合的名称collection="array"

mybatis将参数装入map中，当参数类型为List时，"list"作为key，List类型的参数作为val

```sql
<insert id="batchAdd" parameterType="ArrayList">
	<foreach collection="list" item="suggest">
   		INSERT INTO [T_Suggest]
		           ([phonenum],[email],[suggestContent],[suggestTime],[state])
		     VALUES
		           (#{suggest.phonenum},#{suggest.email},#{suggest.suggestContent},#{suggest.suggestTime},#{suggest.state})
	</foreach>
</insert>
```