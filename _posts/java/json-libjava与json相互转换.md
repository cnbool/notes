title: json-lib:java与json相互转换
id: 785
categories:
  - Java
date: 2013-08-14 23:10:39
tags:
---

**json-lib需要的包:**
json-lib-2.4-jdk15.jar
commons-beanutils-1.7.0.jar
commons-collections-3.2.jar
commons-lang-2.3.jar
commons-logging-1.0.4.jar
ezmorph-1.0.3.jar
<!--more-->

GirlFriend类:
```java
public class GirlFriend {
	private String name;
	private Integer age;

	public GirlFriend() {
	}

	public GirlFriend(String name, Integer age) {
		this.name = name;
		this.age = age;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public Integer getAge() {
		return age;
	}

	public void setAge(Integer age) {
		this.age = age;
	}
}
```

Boy类:
```java
public class Boy {
	private String name;
	private List<GirlFriend> girlFriends;

	public Boy() {
	}

	public Boy(String name, List<GirlFriend> girlFriends) {
		this.name = name;
		this.girlFriends = girlFriends;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public List<GirlFriend> getGirlFriends() {
		return girlFriends;
	}

	public void setGirlFriends(List<GirlFriend> girlFriends) {
		this.girlFriends = girlFriends;
	}

}
```

**
bean-->json字符串**
```java
//GirlFriend-->json字符串:
	GirlFriend girlFriend = new GirlFriend("xiaoli", 18);

	JSONObject jsonObject = JSONObject.fromObject(girlFriend);

	System.out.println(jsonObject.toString());
	// {"age":18,"name":"xiaoli"}

//Boy-->json字符串:
	ArrayList<GirlFriend> arrayList = new ArrayList<GirlFriend>();
	arrayList.add(new GirlFriend("GF1", 18));
	arrayList.add(new GirlFriend("GF2", 19));
	arrayList.add(new GirlFriend("GF3", 20));

	Boy boy = new Boy("xiaoming", arrayList);
	JSONObject jsonObject = JSONObject.fromObject(boy);

	System.out.println(jsonObject.toString());
	//{"girlFriends":[{"age":18,"name":"GF1"},{"age":19,"name":"GF2"},{"age":20,"name":"GF3"}],"name":"xiaoming"}

//String ArrayList-->json字符串:
	ArrayList<String> arrayList = new ArrayList<String>();
	arrayList.add("s1");
	arrayList.add("s2");
	arrayList.add("s3");

	JSONArray jsonObject = JSONArray.fromObject(arrayList);

	System.out.println(jsonObject.toString());
	//["s1","s2","s3"]

//Bean ArrayList-->json字符串:
	ArrayList<GirlFriend> arrayList = new ArrayList<GirlFriend>();
	arrayList.add(new GirlFriend("GF1", 18));
	arrayList.add(new GirlFriend("GF2", 19));
	arrayList.add(new GirlFriend("GF3", 20));

	JSONArray jsonObject = JSONArray.fromObject(arrayList);

	System.out.println(jsonObject.toString());
	//[{"age":18,"name":"GF1"},{"age":19,"name":"GF2"},{"age":20,"name":"GF3"}]

//map-->json字符串1:
	HashMap<String, String> map = new HashMap<String, String>();
	map.put("key1", "value1");
	map.put("key2", "value2");
	map.put("key3", "value3");

	JSONObject jsonObject = JSONObject.fromObject(map);

	System.out.println(jsonObject.toString());
	//{"key3":"value3","key2":"value2","key1":"value1"}

//map-->json字符串2:
	HashMap<String, GirlFriend> map = new HashMap<String, GirlFriend>();
	map.put("key1", new GirlFriend("GF1", 18));
	map.put("key2", new GirlFriend("GF2", 19));
	map.put("key3", new GirlFriend("GF3", 20));

	JSONObject jsonObject = JSONObject.fromObject(map);

	System.out.println(jsonObject.toString());
	//{"key3":{"age":20,"name":"GF3"},"key2":{"age":19,"name":"GF2"},"key1":{"age":18,"name":"GF1"}}

```

**json字符串-->bean**
```java
//GirlFriend json字符串-->bean:
	String jsonStr = "{'age':18,'name':'xiaoli'}";

	JSONObject jsonObject = JSONObject.fromObject(jsonStr);
	GirlFriend gf = (GirlFriend) JSONObject.toBean(jsonObject, GirlFriend.class);

	System.out.println("name=" + gf.getName() + ",age=" + gf.getAge());
	//name=xiaoli,age=18

//Boy json字符串-->bean:
	String str = "{'girlFriends':[{'age':18,'name':'GF1'},{'age':19,'name':'GF2'},{'age':20,'name':'GF3'}],'name':'xiaoming'}";
	JSONObject jsonObject = JSONObject.fromObject(str);

	HashMap<String, Class<?>> classMap = new HashMap<String, Class<?>>();
	classMap.put("girlFriends", GirlFriend.class);

	Boy boy = (Boy) JSONObject.toBean(jsonObject, Boy.class, classMap);

	System.out.println(boy.getName());
	for (GirlFriend gf : boy.getGirlFriends()) {
		System.out.println("name=" + gf.getName() + ",age=" + gf.getAge());
	}
	//xiaoming
	//name=GF1,age=18
	//name=GF2,age=19
	//name=GF3,age=20

//json字符串-->集合1:
	String str = "['s1','s2','s3']";

	JSONArray jsonArray = JSONArray.fromObject(str);
	Collection<String> arrayList =  JSONArray.toCollection(jsonArray);

	for (String s : arrayList) {
		System.out.println(s);
	}
	//s1
	//s2
	//s3

//json字符串-->集合2:
	String str = "[{'age':18,'name':'GF1'},{'age':19,'name':'GF2'},{'age':20,'name':'GF3'}]";

	JSONArray jsonArray = JSONArray.fromObject(str);
	Collection<GirlFriend> arrayList =  JSONArray.toCollection(jsonArray, GirlFriend.class);

	for (GirlFriend gf : arrayList) {
	    System.out.println("name=" + gf.getName() + ",age=" + gf.getAge());
	}
	//name=GF1,age=18
	//name=GF2,age=19
	//name=GF3,age=20
```

```java

```
