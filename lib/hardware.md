# 硬件基础

标签（空格分隔）： Linux基础

---

## 硬件设备文件

| 设备 | 设备在Linux内的文件名 |
| --- | --- |
| SCSI/SATA/USB硬盘机 | /dev/sd[a-p] |
| USB闪存盘 | /dev/sd[a-p] （与SATA相同） |
| VirtI/O界面 | /dev/vd[a-p] （用于虚拟机内） |
| 软盘机| /dev/fd[0-7] |
| CDROM/DVDROM | /dev/scd[0-1] （通用） /dev/sr[0-1] （通用，CentOS 较常见） /dev/cdrom （当前CDROM） |
| 磁带机 | /dev/ht0 （IDE 界面） /dev/st0 （SATA/SCSI界面） /dev/tape （当前磁带） |
| 鼠标 | /dev/input/mouse[0-15] （通用） /dev/psaux （PS/2界面） /dev/mouse （当前鼠标） |
| 打印机| /dev/lp[0-2] （25针打印机） /dev/usb/lp[0-15] （USB 接口） |

磁盘文件名

* /dev/sd[ap][1-128]：为实体磁盘的磁盘文件名
* /dev/vd[ad][1-128]：为虚拟磁盘的磁盘文件名

## 磁盘分区

### MBR分区格式

第一个扇区 512Bytes 记录以下两个数据

* 主要开机记录区（Master Boot Record, MBR）：记录开机管理程序，有446 Bytes
* 分区表（partition table）：记录整颗硬盘分区的状态，有64 Bytes
  * 最多仅能有四组记录区，每组记录区记录了该区段的启始与结束的柱面号码

分区

* 可以划分为主要分区和延伸分区
 * 延伸分区最多有一个
 * 主要分区 + 延伸分区最多4个
* 延伸分区又可继续划分为逻辑分区
 * 在延伸分区的最前面几个扇区来记载逻辑分区信息
* 主要分区和延伸分区的设备号为1-4，逻辑分区设备号从5开始

![MSDOS磁盘分区](https://raw.githubusercontent.com/wchaochao/images/master/gitbook-linux-base/msdos.png)

* P1:/dev/sda1
* P2:/dev/sda2
* L1:/dev/sda5
* L2:/dev/sda6
* L3:/dev/sda7
* L4:/dev/sda8
* L5:/dev/sda9

### GPT分区格式

使用34个LBA区块来记录分区信息，整个磁盘的最后33个LBA作为备份

* LBA：逻辑区块位址（Logical Block Address, LBA），默认为512Bytes，从0开始编号
* LBA0：MBR相容区块，前446Bytes存储开机管理程序，后64Bytes仅放入一个特殊标志表示为GPT 格式
* LBA1：GPT表头记录，记录了分区表本身的位置与大小，同时记录了备份用的GPT分区放置的位置，以及分区表的检验机制码
* LBA2-33：实际记录分区信息处，每个LBA都可以记录4笔分区录

![GPT分区格式](https://raw.githubusercontent.com/wchaochao/images/master/gitbook-linux-base/gpt.jpg)

## 开机流程

### BIOS开机

* BIOS：写入到主板上的一个固件，计算机系统主动执行的第一个程序
 * 读取CMOS中的硬件参数，到系统盘去执行开机管理程序
* 开机管理程序：提供开机选择菜单
  * 直接载入核心文件
  * 将开机管理转交给其他分区开机扇区上的开机管理程序

![BIOS开机](https://raw.githubusercontent.com/wchaochao/images/master/gitbook-linux-base/bios.gif)
