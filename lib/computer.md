# 计算机基础

标签（空格分隔）： Linux基础

---

## 计算机

* 输入单元：如键盘、鼠标
* 处理单元
 * CPU控制单元：协同各单元工作
 * CPU算数逻辑单元：负责程序运算和逻辑判断
 * 内存：存储CPU处理的数据
* 输出单元：如显示器

![计算机硬件单元](https://raw.githubusercontent.com/wchaochao/images/master/gitbook-linux-base/computer.gif)

## 硬件

### 主板

* 北桥：负责链接速度较快的CPU、内存与显卡接口等元件，已整合到CPU中
* 南桥：负责连接速度较慢的设备接口， 包括硬盘、USB、网卡等
* I/O位址：主板上设备的地址
* IRQ中断信道：主板上设置连接到CPU的专门路径
* CMOS：记录主板上面的重要参数，包括系统时间、CPU电压与频率、各项设备的I/O位址与IRQ等，借着额外的电源来发挥记录功能
* BIOS：载入CMOS当中的参数，并尝试调用储存设备中的开机程序，进一步进入操作系统的程序

### CPU

内置微指令集，进行控制和运算

* 多核: 一个实体的CPU外壳中，含有两个以上的CPU单元
* RISC: 精简指令集（Reduced Instruction Set Computer），如SPARC系列、PowerPC系列、ARM CPU 系列
* CISC: 复杂指令集（Complex Instruction Set Computer），主要有AMD、Intel、VIA等的x86架构的CPU

#### 频率

* 频率：每秒钟可以进行的工作次数，频率 = 外频 * 倍频
* 外频：CPU与外部元件进行数据传输时的速度
* 倍频：CPU 内部用来加速工作性能的一个倍数

#### 位

CPU一次数据读取的最大量

* 32位：如i386, i586, i686
* 64位：如x86_64

较高阶的硬件通常会向下相容旧有的软件，但较高阶的软件可能无法在旧机器上面安装

### 内存

存储CPU处理的数据

![内存](https://raw.githubusercontent.com/wchaochao/images/master/gitbook-linux-base/memory.gif)

* SRAM：静态随机存取内存（Static Random Access Memory），如CPU内的第二层高速缓存内存
* DRAM：动态随机存取内存（Dynamic Random Access Memory）, 只有在通电时才能记录与使用，断电后数据就消失
* ROM：只读存储器（Read Only Memory），在没有通电时也能够将数据记录下来，如写入 BIOS 的内存芯片

#### 带宽

* DDR SDRAM：双倍数据传送速度（Double Data Rate），传输速度更快
* 多通道：多个内存汇整在一起提供更大的数据宽度

### 显卡

VGA（Video Graphics Array），显示图形影像

* 分辨率
* 色彩深度

#### 格式

* DVI：常见于液晶屏幕的链接， 标准规格主要有： 1920x1200px @60Hz、 2560x1600px @60Hz 等
* HDMI：同时传送影像与声音，被广泛的使用于电视屏幕中

### 硬盘

Hard Disk Drive, HDD

#### 组成

* 圆形盘片：存储数据
* 磁头：读写数据
* 机械手臂：使磁头运动
* 主轴马达：让盘片转动

![硬盘组成](https://raw.githubusercontent.com/wchaochao/images/master/gitbook-linux-base/disk.jpg)

* 扇区：最小的物理储存单位，主要有512Bytes 与4K 两种格式
* 磁道：扇区组成的同心圆
* 柱面：多个盘片的同一磁道组成柱面
* 分区：按柱面分区和扇区分区

#### 缓冲内存

可以将硬盘内常使用的数据高速缓存起来，以加速系统的读取性能

#### 传输接口

* SATA接口
* SAS接口
* USB接口

#### 固态硬盘

Solid State Disk, SSD，没有马达不需要转动，而是通过内存直接读写的特性，因此除了没数据延迟且快速之外，还很省电

### 电源供应器

向CPU/RAM/主板/硬盘供电

### 周边设备接口

![主板接口](https://raw.githubusercontent.com/wchaochao/images/master/gitbook-linux-base/device.jpg)

### 外接卡接口

PCI -> AGP -> PCI-X -> PCIe

## 软件

* 系统软件
* 应用程序

### 操作系统

#### 组成

* 操作系统核心：管理电脑的所有活动以及驱动系统中的所有硬件
* 系统调用层：提供一整组的开发接口给工程师来开发软件

#### 功能

* 系统调用接口
* 程序管理
* 内存管理
* 文件系统管理
* 设备的驱动

#### 常见

* Windows操作系统：开发在x86架构上的操作系统之一
* Mac操作系统：苹果公司为Mac系列产品开发的专属操作系统

### 应用程序

封装在操作系统之上的程序

## 计量单位

### 容量单位

1 bit = 1个二进制
1 Byte = 8 bits

| 进位制 | Kilo | Mega | Giga | Tera | Peta | Exa | Zetta |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 二进制 | 1024 | 1024K | 1024M | 1024G | 1024T | 1024P | 1024E |
| 十进制 | 1000 | 1000K | 1000M | 1000G | 1000T | 1000P | 1000E |

### 速度单位

* CPU运算速度：GHz, 1秒运行多少次
* 网络传输速度：Mbps, 1秒传输多少bit
