title: MBR
categories:
  - mbr
tags:
---

** MBR，全称为Master Boot Record，即硬盘的主引导记录 **

硬盘的0柱面、0磁头、1扇区称为主引导扇区(主引导记录MBR)

它由三个部分组成:
主引导程序(Boot Loader)	446个字节
硬盘分区表DPT(Disk Partition Table)	64个字节
分区有效标志(Magic Number)	2个字节。Little Endian处理器(如Intel)该值为0xAA55，Big Endian处理器(如Motorola6800)该值为0x55AA

MBR分区表的最大可寻址的存储空间2T