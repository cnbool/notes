title: android开发学习笔记(一)
id: 883
categories:
  - Android
date: 2013-05-28 07:20:48
tags:
---

## Create a Project with Eclipse

<!--more-->
Click **New**
select **Android Application Project**, and click **Next**.
**Application Name** is the app name that appears to users. For this project, use "My First App."
**Project Name** is the name of your project directory and the name visible in Eclipse.
**Package Name** is the package namespace for your app
Your package name must be unique across all packages installed on the Android system
For this reason, it's generally best if you use a name that begins with the reverse domain name of your organization or publisher entity
you cannot publish your app on Google Play using the "com.example" namespace.
**Minimum Required SDK** is the lowest version of Android that your app supports, indicated using the [API level](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels). To support as many devices as possible, you should set this to the lowest version available that allows your app to provide its core feature set.
Leave this set to the default value for this project.
**Target SDK** indicates the highest version of Android (also using the [API level](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)) with which you have tested with your application.
**Compile With** is the platform version against which you will compile your app. By default, this is set to the latest version of Android available in your SDK
**Theme** specifies the Android UI style to apply for your app. You can leave this alone.
Click **Next**.
On the next screen to configure the project, leave the default selections and click **Next**.
The next screen can help you create a launcher icon for your app.
Now you can select an activity template from which to begin building your app.
For this project, select **BlankActivity** and click **Next**.
Leave all the details for the activity in their default state and click **Finish**.
Your Android project is now set up with some default files and you’re ready to begin building the app.

**You should be aware of a few directories and files in the Android project:**
**AndroidManifest.xml**
The [manifest file](http://developer.android.com/guide/topics/manifest/manifest-intro.html) describes the fundamental characteristics of the app and defines each of its components.

One of the most important elements your manifest should include is the [`<uses-sdk>`](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html) element. This declares your app's compatibility with different Android versions using the [`android:minSdkVersion`](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html#min) and[`android:targetSdkVersion`](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html#target) attributes. For your first app, it should look like this:

[code lang="xml"]
<manifest xmlns:android="http://schemas.android.com/apk/res/android" ... >
    <uses-sdk android:minSdkVersion="8" android:targetSdkVersion="17" />
    ...
</manifest>
```

**src/**
Directory for your app's main source files. By default, it includes an Activity class that runs when your app is launched using the app icon.
**res/**
Contains several sub-directories for app resources. Here are just a few:
drawable-hdpi/
Directory for drawable objects (such as bitmaps) that are designed for high-density (hdpi) screens. Other drawable directories contain assets designed for other screen densities.
**layout/**
Directory for files that define your app's user interface.
**values/**
Directory for other various XML files that contain a collection of resources, such as string and color definitions.
