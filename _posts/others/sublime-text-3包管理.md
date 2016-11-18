title: sublime text 3包管理
categories:
  - Other
date: 2016-06-17 10:36:30
tags:
---

** 安装Package Control **

菜单 View->Show Console中输入如下内容
```
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
```

** 使用Package Control安装插件 **

*** 安装MarkDown Preview ***
快捷键Ctrl + Shift + P 在弹出的输入框中，输入Install Package 回车,稍等后输入MarkDown Preview回车

*** 安装ConvertToUTF8 ***
ConvertToUTF8可以解决GBK中文乱码问题
快捷键Ctrl + Shift + P 在弹出的输入框中，输入Install Package 回车,稍等后输入ConvertToUTF8回车