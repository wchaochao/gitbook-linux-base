# 文件备份

标签（空格分隔）： Linux基础

---

## 文件系统备份

### xfsdump

备份已挂载的xfs文件系统

```bash
# 全量备份
xfsdump -L 0 -f <dest> <mountpoint>

# 增量备份
# n为1 - 9，表示第几次增量备份
xfsdump -L <n> -f <dest> <mountpoint>

# 设置session label
xfsdump -L 0 -l <label> -f <dest> <mountpoint>

# 设置media label
xfsdump -L 0 -M <label> -f <dest> <mountpoint>

# 查看备份信息
xfsdump -I
```

### xfsrestore

还原xfsdump备份的文件

```bash
# 复原整个备份
xfsrestore -f <dump> <dest>

# 复原备份中的某些目录
xfsrestore -f <dump> -s <dir> <dest>

# 进入交互模式
xfsrestore -f <dump> -i <dest>

# 查看备份信息
xfsrestore -I
```

## 光盘写入

### mkisofs

创建iso镜像文件

```bash
# 创建iso镜像文件
mkisofs -o <isofile> <source>

# 显示详细信息
mkisofs -o <isofile> -v <source>

# 支持更广的文件名，默认只支持<8个字符.3个字符>
mkisofs -o <isofile> -r <source>

# 设置卷名
mkisofs -o <isofile> -V <Volume> <source>

# 排除某些文件
mkisofs -o <isofile> -m <file> <source>

# 设置对应关系 镜像内目录=原目录
mkisofs -o <isofile> graft-point <isodir>=<systemdir> <source>
```

示例

```bash
mkisofs -r -V 'linux_file' -o /tmp/system.img -m /root/etc -graft-point /root=/root /home=/home /etc=/etc
```

### wodim

光盘烧录

```
# 查看光驱信息
wodim --devices dev=/dev/sr0

# 抹除原始内容
wodim -v dev=/dev/sr0 blank=fast

# 烧录
wodim -v dev=/dev/sr0 <isofile>
```

## 其他

### dd

读取扇区内容备份成一个文件

```bash
# 读取设备内容备份成一个文件
# if - 输入， of - 输出，bs - block大小，count - block数目
dd if=<input-file> of=<output-file> bs=<block-size> count=<number>
```

示例

```bash
# 备份文件
dd if=/etc/passwd of=/tmp/passwd.back

# 备份光盘
dd if=/dev/sr0 of=/tmp/system.iso

# 烧录到U盘
dd if=/tmp/system.iso of=/dev/sda

# 备份整个文件系统
dd if=/dev/vda2 of=/tmp/vda2.img
```

### cipo

备份任意文件

```bash
# 备份
find <path> | cpio -ocvB > <dest>

# 还原
cpio -idvc < <source>
```
