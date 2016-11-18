title: jQuery ui入门
id: 801
categories:
  - jQuery
date: 2013-05-23 07:31:34
tags:
---

**1 介绍**
jQuery UI 是以jQuery为基础的网页用户界面代码库,是jQuery的插件。
jQuery UI 主要分为3个部分：交互、微件和效果库。
交互
交互部件是一些与鼠标交互相关的内容，包括Draggable,Droppable,Resizable,Selectable和Sortable等。
微件
主要是一些界面的扩展，包括Accordion,AutoComplete,ColorPicker,Dialog,Slider,Tabs,DatePicker,Magnifier,ProgressBar,Spinner等。
效果库
用于提供丰富的动画效果，让动画不再局限于jQuery的animate()方法。

jQuery UI 与 jQuery 的主要区别是：
(1) jQuery是一个js库，主要提供的功能是选择器，属性修改和事件绑定等等。
(2) jQuery UI则是在jQuery的基础上，利用jQuery的扩展性，设计的插件。提供了一些常用的界面元素，诸如对话框、拖动行为、改变大小行为等等。
(以上内容摘自百度百科[http://baike.baidu.com/view/2998196.htm](http://baike.baidu.com/view/2998196.htm))
<!--more-->
**2 下载**
jQuery UI官网:[http://jqueryui.com/](http://jqueryui.com/)
写此文时最新版本:jquery-ui-1.10.3
下载文件:jquery-ui-1.10.3.zip

**3 目录结构**
解压zip,目录结构:
jquery-ui-1.10.3/
jquery-ui-1.10.3/demos   [功能演示]
jquery-ui-1.10.3/external
jquery-ui-1.10.3/tests   [测试例子]
jquery-ui-1.10.3/themes   [主题]
jquery-ui-1.10.3/ui   [全部的js文件]

**3.1 jquery-ui-1.10.3/ui**目录
此文件夹中存放的是jQuery UI的所有js文件,jQuery UI每个功能都分成了独立的js,例如:
手风琴效果jquery.ui.accordion.js
可拖拽效果:jquery.ui.draggable.js

jquery.ui.core.js这个是jQuery UI核心的js文件,使用时要引入
如果只需要使用jQuery UI的部分功能,则引用相应的js文件即可,例如只使用手风琴效果和可拖拽功能:
[code lang="js" highlight="2,3,4"]
<script src="scripts/jquery-1.9.1.js"></script>
<script src="scripts/ui/jquery.ui.core.js"></script>
<script src="scripts/ui/jquery.ui.accordion.js"></script>
<script src="scripts/ui/jquery.ui.draggable.js"></script>
```

jquery-ui.js这个js文件的体积比较大,因为它包含了jQuery UI的所有功能,
也包含了jquery.ui.core.js,所以如果需要使用jQuery的所有功能,只需要引用此js文件即可,例:
```js
<script src="scripts/jquery-1.9.1.js"></script>
<script src="scripts/ui/jquery-ui.js"></script>
```

以上例子中引入的js文件都是未经过压缩的,实际开发中为了节约带宽,最好使用js文件的压缩版本
jquery-ui-1.10.3/ui/minified里提供了所有js文件的压缩版

**3.2 jquery-ui-1.10.3/themes**目录
jquery-ui-1.10.3/base这个目录里放的是jQuery UI各组件的css,默认主题的css,图片.
jquery.ui.theme.css是jQuery UI默认主题的css
jquery.ui.core.css是jQuery UI核心的css文件
各组件的样式:
jquery.ui.accordion.css  手风琴效果样式
jquery.ui.datepicker.css  日期选择器样式

jquery.ui.theme.css和jquery.ui.core.css是必须的,其他组件的css可以选择引入
base目录中有个jquery.ui.all.css文件,这个文件简化了引用,使用jQuery UI时可以只引用这一个css文件:
[code lang="css"]
<link rel="stylesheet" href="styles/themes/base/jquery.ui.all.css">
```

原因是这个jquery.ui.all.css文件引用了其他的css文件,内容:
[code lang="css"]
@import "jquery.ui.base.css";
@import "jquery.ui.theme.css";
```
jquery.ui.base.css内容:
[code lang="css"]
@import url("jquery.ui.core.css");

@import url("jquery.ui.accordion.css");
@import url("jquery.ui.autocomplete.css");
@import url("jquery.ui.button.css");
@import url("jquery.ui.datepicker.css");
@import url("jquery.ui.dialog.css");
@import url("jquery.ui.menu.css");
@import url("jquery.ui.progressbar.css");
@import url("jquery.ui.resizable.css");
@import url("jquery.ui.selectable.css");
@import url("jquery.ui.slider.css");
@import url("jquery.ui.spinner.css");
@import url("jquery.ui.tabs.css");
@import url("jquery.ui.tooltip.css");
```
如果只使用了部分组件,修改jquery.ui.base.css,只保留需要的import.

jquery-ui.css文件比较大,因为它包含了默认情况下jquery.ui.base.css的所有内容(即jquery.ui.core.css和所有组件的css)
如果使用全部的组件,可以修改jquery.ui.all.css为
[code lang="css" highlight="1,2"]
/*@import "jquery.ui.base.css";*/
@import "jquery-ui.css";
@import "jquery.ui.theme.css";
```

**4 使用其他主题**
在官网下载全部的主题:jquery-ui-themes-1.10.3.zip
jquery-ui-themes-1.10.3/themes目录下是所有的主题文件夹
jquery-ui-themes-1.10.3/themes/cupertino是其中的一款主题,结构如下:
[code lang="text"]
cupertino/jquery.ui.theme.css
cupertino/jquery-ui.css
cupertino/jquery-ui.min.css
cupertino/images
```
jquery-ui.css 包含了jQuery UI核心css和各组件的css, jquery-ui.min.css是它的压缩版,
主题文件夹下的jquery-ui.css和base目录中的是一样的,可以不使用它.
使用主题时,将主题文件夹放到项目中,目录结构例如:
[code lang="text"]
styles/theme/
styles/theme/base/images/
styles/theme/base/jquery.ui.all.css
styles/theme/base/jquery.ui.theme.css
styles/theme/base/jquery.ui.base.css
styles/theme/base/jquery.ui.core.js
styles/theme/base/jquery.ui.accordion.css
styles/theme/base/...
styles/theme/cupertino/jquery.ui.theme.css
styles/theme/cupertino/images/
```
更换主题方法:
更改jquery.ui.all.css文件
[code lang="css" highlight="2,3"]
@import "jquery.ui.base.css";
/*@import "jquery.ui.theme.css";*/
@import "../cupertino/jquery.ui.theme.css";
```
