title: 使用sqlldr命令将文本数据导入oracle数据库
categories:
  - oracle
date: 2012-09-08 04:24:14
---

sqlldr cmd命令
```
sqlldr userid=username/password control='D:control.ctl' direct=true parallel=true log='D:logsimport1.log' bad='D:logsbadfile.txt' errors=10000
```

control.ctl 控制文件
```
LOAD DATA
INFILE "Dfile1.txt"
INFILE "Dfile2.txt"
INFILE "Dfile3.txt"

APPEND
INTO TABLE TEST_TABLE
(
    virtual_column FILLER terminated by 'time=',
    TIME DATE "YYYY-MM-DD HH24:MI:SS" terminated by ';sequenceid=',
    SEQUENCEID terminated by ';num1=',
    NUM1 terminated by ';num2=' "NVL(:NUM1, '99999999999')",
    NUM2 terminated by ';type=' "NVL(:NUM2, '99999999999')",
    TYPE terminated by ';LACI=',
    LACI terminated by ';CII=',
    CII terminated by ';',
    virtual_column2 FILLER
)
```

 
1.txt 数据文件
```
003;time=2012/9/8 18:07:14;sequenceid=460016540611504;num1=18657921719;num2=13988257692;type=1;LACI=55176;CII=3023;c_id=2720000000
003;time=2012/9/8 18:07:14;sequenceid=460012205234409;num1=13252012730;num2=15967234725;type=0;LACI=16976;CII=31406;c_id=2720000001
003;time=2012/9/8 18:06:03;sequenceid=460014669043726;num1=13065951700;num2=13989414873;type=1;LACI=18003;CII=44997;c_id=2720000002
003;time=2012/9/8 18:07:14;sequenceid=460013690000997;num1=13003694990;num2=18258885726;type=0;LACI=13840;CII=51700;c_id=2720000003
003;time=2012/9/8 18:07:14;sequenceid=460014879049351;num1=13067536240;num2=13276830412;type=0;LACI=16902;CII=12466;c_id=2720000004
003;time=2012/9/8 18:06:03;sequenceid=460015810606198;num1=18658102066;num2=13065903797;type=1;LACI=55045;CII=35211;c_id=2720000005
003;time=2012/9/8 18:06:03;sequenceid=460016221214359;num1=13282951712;num2=13486915044;type=0;LACI=55186;CII=15573;c_id=2720000006
003;time=2012/9/8 18:07:14;sequenceid=460016331275252;num1=13276830412;num2=13067536240;type=1;LACI=16902;CII=16341;c_id=2720000007
003;time=2012/9/8 18:06:03;sequenceid=460015710361506;num1=15658800990;num2=18668169315;type=1;LACI=55062;CII=35903;c_id=2720000008
003;time=2012/9/8 18:08:44;sequenceid=460016591200120;num1=13216596666;num2=13002625088;type=1;LACI=17042;CII=32906;c_id=2720000009

```
