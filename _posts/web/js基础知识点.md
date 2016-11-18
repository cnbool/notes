title: js基础知识点
id: 1135
categories:
  - Javascript
date: 2013-08-21 01:53:57
tags:
---

源自:[http://blog.jobbole.com/46286/](http://blog.jobbole.com/46286/http:// "http://blog.jobbole.com/46286/")

使用正则表达式替换字符串:
```js
var str = "Hello hello word!";
alert(str.replace(/Hello/g, "你好"));  //"你好 hello word!"
//忽略大小写替换
alert(str.replace(/Hello/gi, "你好"));  //"你好 你好 word!"
```

sort排序
```js
基本:
var arr = [1, 3, 6, 2];
arr.sort();
alert(arr);
//1,2,3,6

高级:
var arr = [{"name":"name_21", "age":21}, {"name":"name_25", "age":25}, {"name":"name_18", "age":18}];
arr.sort(function(o1, o2) {
	return o1.age > o2.age;
});

for (var i = 0; i < arr.length; i++) {
	alert(arr[i].name);
}
//name_18
//name_21
//name_25
```
