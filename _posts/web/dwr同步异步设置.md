title: dwr同步异步设置
id: 511
categories:
  - Javascript
date: 2013-01-05 19:50:42
tags:
---

AJAX支持同步和异步的调用
XMLHttpRequest的open函数

```js
XMLHttpRequest.open(String method, String URL, boolean asynchronous);
```

Dwr同步异步设置

```js
DWREngine.setAsync(false);
adverTastDetail.getAdvertDetailById(detailId,getDetailCallback);
DWREngine.setAsync(true);
```

以下内容摘自:[http://hi.baidu.com/limuziwei/item/6ced1125fa656efc50fd87e9](http://hi.baidu.com/limuziwei/item/6ced1125fa656efc50fd87e9 "http://hi.baidu.com/limuziwei/item/6ced1125fa656efc50fd87e9")
进入 dwr.jar 包, 打开 orgdirectwebremotingengine.js 文件，
搜索该文件中是否存在DWREngine变量的定义。
(因为在dwr3.x版本的engine.js中已经取消了DWREngine的定义)
如果没有DWREngine，可以把 DWREngine 改为 dwr.engine
(ps:我没有验证过)
