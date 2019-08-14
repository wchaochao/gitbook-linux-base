# 磁盘管理

标签（空格分隔）： Linux基础

---

* 分区：创建可用的 partition
* 格式化：创建可用的 filesystem
* 检验：对 filesystem 进行检验
* 挂载：将 filesystem 挂载到目录

## 查看

### lsblk

列出block设备列表

```bash
# 列出block设备列表
lsblk

# 仅列出磁盘
lsblk -d

# 显示完整设备名
lsblk -p

# 显示文件系统类型
lsblk -f

# 显示权限
lsblk -m

# 显示详细信息
lsblk -t

# 查看核心分区信息
cat /proc/partitions
```

### blkid

列出block设备UUID

```bash
# 列出设备的UUID
blkid
```

## 分区

分区之后需要更新分区表

### gdisk

GPT分区

```bash
# 进入GPT分区环境
gdisk <disk>
```

* `?`: 帮助
* `p`: 输出磁盘分区信息
* `n`: 新增分区
* `d`: 删除分区
* `q`: 离开
* `w`: 保存并离开

### fdisk

MBR分区

```bash
# 进入MBR分区环境
fdisk <disk>
```

* `m`: 帮助
* `p`: 输出磁盘分区信息
* `n`: 新增分区
* `d`: 删除分区
* `q`: 离开
* `w`: 保存并离开

### parted

分区，同时指出GPT、MBR

```bash
# 查看分区信息
parted <device>

# 新增分区
parted <device> [primary | extended | logical] [ext4 | vfat | xfs] <start> <end>

# 删除分区
parted <device> rm <partition>
```

示例

```bash
parted /dev/vda rm 4
```

### partprobe

更新分区表

```bash
# 更新分区表并显示
partprobe -s
```

## 格式化

### mkfs.xfs

格式化为xfs文件系统

```bash
# 格式化为xfs文件系统
mkfs.xfs <device>

# 设置data section
# agcount - 存储群组个数，agsize - 每个存储群组容量，size - 总容量
# su - 磁盘阵列文件分块大小，sw - 磁盘阵列存储文件的磁盘数量
# sunit - 磁盘阵列文件分块占的512Bytes扇区数目，swidth - sunit * sw
mkfs.xfs -d {agcount=数值, agsize=数值, size=数值, su=数值, sw=数值, sunit=数值, swidth=数值} <device>

# 设置inode
# isize - 每个inode容量，size - inode总容量
mkfs.xfs -i {isize=数值, size=数值} <device>

# 设置每个block容量
mkfs.xfs -b <bsize> <device>

# 设置realtime区
# extsize - 每个extend block容量
mkfs.xfs -r {extsize=数值} <device>

# 设置标签
mkfs.xfs -L <label> <device>

# 强制格式化
mkfs.xfs -f <device>
```

### mkfs.ext4

格式化为ext4文件系统

```bash
# 格式化为ext4文件系统
mkfs.ext4 <device>

# 设置每个block容量
mkfs.ext4 -b <bsize> <device>

# 设置标签
mkfs.ext4 -L <label> <device>
```

### mkfs

格式化为指定文件系统

```bash
# 格式化为指定文件系统
mkfs -t <filesystem> <device>
```

## 检验

### xfs_repair

处理xfs文件系统错乱

```bash
# 检测和修复xfs文件系统
xfs_repair <device>

# 仅检测xfs文件系统
xfs_repair -n <device>
```

### fsck.ext4

处理ext4文件系统错乱

```bash
# 检测和修复ext4文件系统
fsck.ext4 <device>

# 自动回复yes
fsck.ext4 -y <device>

# 强制检查
fsck.ext4 -f <device>
```

## 修改

### mknod

创建设备文件

```bash
# 创建区块设备文件
mknod <file> b

# 创建字符设备文件
mknod <file> c

# 创建FIFO文件
mknod <file> p

# 设置设备代码
mknod <file> [bcp] <Major> <Minor>
```

### xfs_admin

修改xfs文件系统的UUID和Lable

```bash
# 显示label
xfs_admin -l <device>

# 设置label
xfs_admin -L <label> <device>

# 显示UUID
xfs_admin -u <device>

# 生成UUID
uuidgen

# 设置UUID
xfs_admin -U <uuid> <device>
```

### tune2fs

修改ext4文件系统的UUID和Lable

```bash
# 显示superblock信息
tune2fs -l <device>

# 设置label
tune2fs -L <label> <device>

# 设置UUID
tune2fs -U <uuid> <device>
```

## 挂载

### mount

对空目录进行挂载

```bash
# 显示目前挂载信息
mount

# 显示挂载信息并显示标签
mount -L

# 依照配置文件/etc/fstab进行挂载
mount -a

# 挂载
mount <device> <mountpoint>
mount UUID="<uuid>" <mountpoint>
mount LABEL="<label>" <mountpoint>

# 挂载并设置
mount -o <setting> <device> <mountpoint>
```

示例

```bash
# 单人维护模式根目录重新挂载
mount -n -o remount,rw /

# 挂载光盘

```

### 开机挂载

在配置文件中/etc/fstab配置挂载信息

* `Device`: 设备文件名或UUID="<uuid>"或LABEL="<label>"
* `Mount Point`: 挂载点
* `filesystem`: 文件系统
* `parameters`: 文件系统参数
* `dump`: 能否被dump备份，填0
* `fsck`: 是否以fsck检查，填0

文件系统参数

| 参数 | 说明 |
| --- | --- |
| async/sync | 是否同步，默认为async |
| auto/noauto | 是否自动测试挂载，默认为auto |
| rw/ro | 是否可读写，默认为rw |
| exec/noexec | 是否可以执行文件，默认为exec |
| user/nouser | 是否允许使用者挂载，默认为nouser |
| suid/nosuid | 是否允许SUID，默认为suid |
| dev/nodev | 是否有设备文件名，默认为dev |

### loop挂载

挂载光盘

```bash
mount -o loop <iso> <mountpoint>
```

挂载大文件

```bash
# 格式化大文件
mkfs.xfs -f <file>

# 挂载格式化后的大文件
mount -o loop UUID="<uuid>" <mountpoint>
```

## 卸载

```bash
# 卸载
unmount <device | mountpoint>

# 强制卸载
unmount -f <device | mountpoint>
```

## swap

内存交互空间

* 内存不足时，会将内存中暂不使用的程序与数据挪入磁盘中的swap中

### 使用分区创建swap

```bash
# 从磁盘中分出一个分区作为swap, type为Linux Swap（8200）
gdisk /dev/vda

# 启用分区
partprobe

# 格式化为swap格式
mkswap <swap-device>

# 启动swap
swapon <swap-device>

# 查看swap
swapon -s
```

### 使用文件创建swap

```bash
# 使用dd创建128M的大文件
dd if=/dev/zero of=<swap-file> bs=1M count=128

# 格式化为swap格式
mkswap <swap-file>

# 启动swap
swapon <swap-file>

# 查看swap
swapon -s

# 设置开机挂载
nano /etc/fstab
<swap-file>  swap  swap  defaults  0  0

# 关掉swap
swapoff <swap-file>
```
