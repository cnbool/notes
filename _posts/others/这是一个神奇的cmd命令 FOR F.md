title: 这是一个神奇的cmd命令 FOR /F
id: 442
categories:
  - Other
date: 2012-11-28 07:56:37
tags:
---

这是一个神奇的命令
程序的运行日志里记录了很多的信息,其中包括短信下发的记录.但是短信下发记录和其他的信息混杂在一起,
想统计日志文本中短信下发的次数还是很困难,后来麻工告诉了我一条神奇的命令:
<!--more-->

```shell
FOR /F %i IN ('dir *.txt /a /b') DO (findstr /i "SENDSMS" %i>> result_dir/result_%i)
```

这条命令的作用是:
查找当前目录中以.txt为后缀的文件,将包含SENDSMS的行输出到本目录下的子目录result_dir的结果文件中,
结果文件的命名是result_原文件名
注意:result_dir目录需提前创建好

例子:
D:/logs/Run_2012-11-28.log 是程序日志文件,内容:

[code lang="plain"]
[2012-11-28 21:59:52:071] Get in position and wait for my go!
[2012-11-28 21:59:53:054] SENDSMS 18600002563 公子夜凉如水,还是进屋歇息吧
[2012-11-28 21:59:58:171] go go go ! fire in the hole!
[2012-11-28 21:59:59:154] SENDSMS 13100002564 take me to your heart
[2012-11-28 22:00:03:194] SENDSMS 18600002565 花田里犯了错,说好破晓前忘掉
[2012-11-28 22:00:04:083] TAKING FIRE!
[2012-11-28 22:00:05:066] SENDSMS 18600002566 you have to cry me out
```

需要在程序日志文件中查找下发短信的记录
下发短信的记录中肯定包含字符串"SENDSMS"
那么,需要在D:/logs/目录下新建目录result_dir,即D:/logs/result_dir/
然后在cmd命令行中进入D:/logs/目录, 敲下:

```shell
FOR /F %i IN ('dir *.log /a /b') DO (findstr /i "SENDSMS" %i>> result_dir/send_sms_%i)
```

之后会在D:/logs/result_dir/send_sms_Run_2012-11-28.log文件中看到查找结果:

[code lang="plain"]
[2012-11-28 21:59:53:054] SENDSMS 18600002563 公子夜凉如水,还是进屋歇息吧
[2012-11-28 21:59:59:154] SENDSMS 13100002564 take me to your heart
[2012-11-28 22:00:03:194] SENDSMS 18600002565 花田里犯了错,说好破晓前忘掉
[2012-11-28 22:00:05:066] SENDSMS 18600002566 you have to cry me out
```

注意:如果在bat文件里写FOR /F命令,那个%i要换成%%i
