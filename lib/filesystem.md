# 文件系统

标签（空格分隔）： Linux基础

---

每种操作系统的文件属性并不相同，为了存放这些文件所需的数据，需要不同的文件系统

* 传统文件系统：ext2 / MS-DOS / FAT / iso9660
* 日志式文件系统： ext3 / ext4 / xfs / ntfs
* 网络文件系统： NFS

## 索引式文件系统

* `superblock`: 记录整个文件系统的整体信息，包括inode/block的总量、使用量、剩余量， 以及文件系统的格式与相关信息等
* `inode`: 记录文件的属性，一个文件占用一个inode，同时记录此文件的数据所在的block号码
* `data block`: 记录文件的内容，若文件太大时，会占用多个block

![索引式文件系统](https://wizardforcel.gitbooks.io/vbird-linux-basic-4e/content/img/filesystem-1.jpg)

## FAT文件系统

![FAT文件系统](https://wizardforcel.gitbooks.io/vbird-linux-basic-4e/content/img/filesystem-2.jpg)

## EXT2文件系统

区分为多个区块群组（block group） 的，每个区块群组都有独立的inode/block/superblock 系统

![EXT2文件系统](https://wizardforcel.gitbooks.io/vbird-linux-basic-4e/content/img/ext2_filesystem.jpg)

### Superblock

记录文件系统的信息

* block 与 inode 的总量
* 未使用与已使用的inode / block 数量
* block 与 inode 的大小
* 文件系统的挂载时间、最近一次写入数据的时间、最近一次检验磁盘的时间等文件系统的相关信息
* 一个valid bit 数值，若此文件系统已被挂载，则valid bit 为0 ，若未被挂载，则valid bit 为1

### Filesystem Description

文件系统描述说明

* 每个block group的开始与结束的block号码
* 每个区段（superblock, bitmap, inodemap, data block）分别介于哪一个block号码之间

### block bitmap

区块对照表

* 记录block的使用和未使用情况

### inode bitmap

* 记录inode的使用和未使用情况

### inode table

记录文件的属性以及该文件实际数据的block编号

* 该文件的存取模式（read/write/excute）
* 该文件的拥有者与群组（owner/group）
* 该文件的容量
* 该文件的相关时间（mtime/ctime/atime）
* 定义文件特性的旗标（flag），如SetUID
* 该文件真正内容的指向（pointer）

特点

* 每个 inode 大小均固定为 128 Bytes
* 每个文件仅会占用一个inode
 * 文件系统能够创建的文件数量与inode 的数量有关
 * 系统读取文件时需要先找到inode，并分析inode所记录的权限与使用者是否符合，若符合才能够开始实际读取block 的内容

指向block

* 12个直接指向：指向记录内容的block
* 1个间接记录：指向一个block，该block用来记录实际内容block的编号
* 1个双间接记录：指向一个block，该block记录其他block，其他block中记录实际内容block的编号
* 1个三间接记录：比双间接多一个block记录

### block

记录文件实际内容

* block 的大小与数量在格式化完就不能够再改变了
* 每个block 内最多只能够放置一个文件的数据
 * 如果文件大于block的大小，则一个文件会占用多个block 数量
 * 若文件小于block，则该block 的剩余容量就不能够再被使用了
 * 占用多个block且block太过离散时会影响读取效率

### 记录

目录

* inode记录该目录的相关权限与属性
* block记录在这个目录下的文件名与该文件名占用的 inode 号码数据

文件

* inode记录该文件的相关权限与属性
* block记录该文件的内容

### 非同步处理

当系统载入一个文件到内存后

* 如果文件没有被改动过，则标志为clean
* 如果文件被改动过，则标志为Dirty
* 系统会不定时的将内存中标志为Dirty的数据写回磁盘，以保持磁盘与内存数据的一致性
* 可通过`sync`指定强制将内存中标志为Dirty的数据写回磁盘

### 挂载

将文件系统与目录树结合

```bash
# 查看支持的文件系统
ls -l /lib/modules/$（uname -r）/kernel/fs

# 查看已载入到内存中的文件系统
cat /proc/filesystems
```

### 日志

ext3/ext4支持

* 在进行文件写入或修订时记录日志，用于对block bitmap、inode bitmap进行检查，防止与实际不一致的情况

### 缺点

* 一开始就格式化好inode 和 block，容量越大，格式化越慢

### 指令

查询Ext家族superblock信息的指令

```bash
# 列出已格式化的设备
blkid

# 查看superblock和block group信息
dumpe2fs <device>

# 仅查看superblock信息
dumpe2fs -h <device>
```

## XFS文件系统

CentOS 7的默认文件系统

* 较适合大容量磁盘与巨型文件
* 几乎所有 Ext4 文件系统有的功能， xfs 都可以具备

### 数据区

分为多个储存区群组来放置文件系统所需要的数据，每个群组包括

* 整个文件系统的 superblock
* 剩余空间的管理机制
* inode的分配与追踪
* 动态创建inode 与 block，有多种不同的容量可供设置

### 活动登录区

用来纪录文件系统的变化

### 实时运行区

将新创建的文件放置在一个或多个Extend块中，等到分配完毕，再写入到数据区的 inode 与 block 中

### 指令

```bash
# 查看xfs设备信息
xfs_info <device>
```

## Linux VFS

Virtual Filesystem Switch，用于管理挂载的文件系统

![VFS](https://wizardforcel.gitbooks.io/vbird-linux-basic-4e/content/img/centos7_vfs.gif)

## 操作

### df

列出文件系统的磁盘使用量

```bash
# 列出所有文件系统的磁盘使用情况
df
df -a

# 列出文件所在的文件系统的磁盘使用情况
df <file>

# 按kB显示
df -k [file]

# 按MB显示
df -m [file]

# 按合适容量显示
df -h [file]

# 显示文件系统类型
df -T [file]

# 显示inode数
df -i [file]
```

示例

```bash
df -aT
```

### du

列出文件或目录容量

```bash
# 显示文件容量
du <file>

# 显示目录容量，默认为当前目录
# 会列出目录下所有次目录的容量
du <dir>

# 列出目录下所有文件、次目录的容量
du -a

# 列出目录总量
du -s

# 总量不包括各次目录的
du -S

# 按kB显示
du -k

# 按MB显示
du -k

# 按合适容量显示
du -h
```

示例

```bash
du -sm /*
```

### ln

创建链接文件

* `hard link`: 硬链接，指向同一个inode
 * 不能跨文件系统
 * 不能链接目录
 * 修改硬链接文件时，源文件也会改变
 * 删除源文件时，硬链接文件仍能打开
* `symbolic Link`: 软链接，指向一个新inode，该inode对应的block记录源文件位置
 * 修改软链接文件时，源文件也会改变
 * 删除源文件时，软链接文件仍能打开

![hard link](https://wizardforcel.gitbooks.io/vbird-linux-basic-4e/content/img/hard_link1.gif)


```bash
# 创建硬链接
ln <source> <target>

# 创建软连接
ln -s <source> <target>

# 目标文件存在时，强制覆盖
ln -f <source> <target>
```
