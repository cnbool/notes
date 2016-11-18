title: Android Action Bar
id: 1195
categories:
  - Android
date: 2013-09-23 22:01:02
tags:
---

**安卓3.0版本(API level 11)才推出了ActionBar**
**If supporting only API level 11 and higher:**
import android.app.ActionBar
**If supporting API levels lower than 11:**
import android.support.v7.app.ActionBar

**使用android.support.v7.app.ActionBar需要支持库,获取方法:**
使用Android SDK Manager更新Android SDK Platform-tools到版本18,支持库位置:
sdkextrasandroidsupportv7appcompat

使用android.support.v7.app.ActionBar具体步骤:
1.eclipse中导入项目sdkextrasandroidsupportv7appcompat,导入之后可以看到项目android-support-v7-appcompat(如有叹号提示,需要更改此项目的Project Build Target)

2.eclipse中右键你的项目->Properties->Android
在Library中点击Add添加Library项目,选择android-support-v7-appcompat

3.项目的AndroidManifest.xml文件中给Activity指定主题:android:theme="@style/Theme.AppCompat.Light"
