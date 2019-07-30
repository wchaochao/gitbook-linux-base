# 文件

标签（空格分隔）： Linux基础

---

## 身份

* `user`: 文件拥有者
* `group`: 文件所属群组（拥有者也在该群组中）
* `others`: 其他成员

## 权限

文件 - 存放实际数据

* `read`: 读取文件内容的权限（最基础的权限）
* `write`: 修改文件内容的权限
* `execute`: 文件可以被系统执行的权限

目录 - 记录文件名清单

* `read`: 读取目录结构清单的权限（需要有execute才能显示目录里的文件信息）
* `write`: 修改目录结构清单的权限（需要有execute才能增删改）
* `execute`: 进入目录了解目录结构的权限（最基础的权限）

能否读取到某个文件内容，跟该文件所在的目录权限也有关系

### 常用权限

前提：祖先目录至少需要具有x的权限

进入目录

* 目录所需权限：使用者对这个目录至少需要具有x的权限

ls查看目录

* 目录所需权限：使用者对这个目录至少需要具有r, x的权限

创建文件

* 目录所需权限：使用者在该目录要具有w, x的权限

读取文件

* 目录所需权限：使用者对这个目录至少需要具有x权限
* 文件所需权限：使用者对文件至少需要具有r的权限

修改文件

* 目录所需权限：使用者在该文件所在的目录至少要有x权限
* 文件所需权限：使用者对该文件至少要有r, w权限

执行文件

* 目录所需权限：使用者在该目录至少要有x的权限
* 文件所需权限：使用者在该文件至少需要有x的权限

删除文件

* 目录所需权限：使用者在该目录要具有w, x的权限

复制文件

* 源文件：能读取文件
* 目标文件：能创建文件

### 默认权限

* 新建文件的默认权限 = `-rw-rw-rw-` - umask
* 新建目录的默认权限 = `drwxrwxrwx` - umask

```bash
# 查看umask
umask
0022 # user权限减0，group权限减2，others权限减2，即新建文件权限为644，新建目录权限为755

# 设置umask
umask 002
```

### 特殊权限

取代执行权限

* `SUID`: Set UID，当s出现在文件拥有者的 x 权限上时
 * 文件为二进制程序文件时，使用者在执行文件时会暂时拥有文件拥有者的权限
* `SGID`: Set UID，当s出现在文件所属群组的 x 权限上时
 * 文件为二进制程序文件时，使用者在执行该文件时会暂时拥有文件所属群组的权限
 * 文件为目录时，使用者在此目录下的有效群组为文件所属群组
* `SBIT`: Sticky Bit，当s出现在其他人的 x 权限上时
 * 文件为目录时，仅有该目录下的文件/目录创建者与 root 能够删除自己的目录或文件

```bash
# SUID 账号设置密码
ls -l /usr/bin/passwd
-rwsr-xr-x. 1 root root 27832 Jun 10  2014 /usr/bin/pass

# SGID
ls -l /usr/bin/locate
-rwx--s--x. 1 root slocate 40496 Jun 10  2014 /usr/bin/locate

# SBIT
ls -l /temp
```

### 设置特殊权限

* SUID - 4, SGID - 2, SBIT - 1
* SUID - u+s, SGID - g+s, SBIT - o+t

```bash
chmod 4755 test; ls -l test
-rwsr-xr-x 1 root root 0 Jun 16 02:53 test

chmod 6755 test; ls -l test
-rwsr-sr-x 1 root root 0 Jun 16 02:53 test

chmod 1755 test; ls -l test
-rwxr-xr-t 1 root root 0 Jun 16 02:53 test

 chmod 7666 test; ls -l test
 -rwSrwSrwT 1 root root 0 Jun 16 02:53 test # 无执行权限，使用大写表示空

 chmod u=rwxs,go=x test1; ls -l test1
 -rws--x--x 1 root root 0 Jun 16 02:53 test1
```

## 文件属性

![文件属性](https://raw.githubusercontent.com/wchaochao/images/master/gitbook-linux-base/file-attr.gif)

### 文件类型

* `d`: 目录
* `-`: 文件，包括纯文本文件、二进制文件、数据文件
* `l`: 软链接文件
* `b`: 区块设备文件，如硬盘、软盘
* `c`: 字符设备文件，如键盘、鼠标
* `s`: 数据接口文件，如socket
* `p`: 数据传输文件， FIFO，即first-in-first-out，用于解决多个程序同时存取一个文件所造成的错误问题

### 文件权限

每个身份都有 r - 读，w - 写，x - 可执行 三种权限

### 链接数

当前文件的链接数

### 容量大小

单位默认为Byte

### 文件时间

* `mtime`: modification time，内容更新时间
* `ctime`: status time，状态更新时间，如权限、属性
* `atime`: access time，读取时间

```bash
date; ll bashrc; ll --time=atime bashrc; ll --time=ctime bashrc
```

### 文件名

* 长度限制：最大长度为255Bytes
* 名称限制：避免特殊字符
* `.`开头的文件为隐藏文件

### 扩展名

没有扩展名

* 是否可执行需要具有可执行的权限
* 是否执行成功需要文件具有可执行的程序码

可用适当的扩展名表明文件种类

* `.sh`: 脚本或批处理文件
* `.tar, .tar.gz, .zip, .tgz`: 压缩文件
* `.html`: 网页文件

### 隐藏属性

如文件只能增加、文件不能修改

* 在Ext2/Ext3/Ext4的Linux文件系统上可以使用lsattr, chattr查看和设置隐藏属性

## 修改文件属性

### chgrp

修改文件所属群组

```bash
# 修改文件所属群组
chgrp <group> <file>

# 递归修改
chgrp -R <group> <file>
```

### chown

修改文件拥有者

```bash
# 修改文件拥有者
chown <user> <file>

# 修改文件拥有者和所属群组
chown <user>:<group> <file>

# 递归修改
chown -R <user> <file>
```

### chmod

修改文件权限

* 数字类型权限：r-4, w-2, x-1, 权限为三者相加
* 符号类型：u-user, g-group, o-others, a-all, -减去, +添加, =设置， r-读，w-写，x-可执行

```bash
# 修改文件权限
chmod <mode> <file>

# 递归修改
chmod -R <mode> <file>
```

示例

```bash
chomd 755 test.sh
chmod u=rwx,go=rx .bashrc
chmod a+w .bashrc
```

## 文件操作

### ls

查看文件或目录信息

```bash
# 查看当前目录下的文件
# 默认 只显示非隐藏文件、以文件名排序、蓝色为目录白色为文件
ls

# 查看指定文件信息
ls <...file>

# 查看全部文件，包括隐藏文件
ls -a

# 查看全部文件，包括隐藏文件，不包括.和..目录
ls -A

# 只显示目录
ls -d

# 递归显示所有文件
ls -R

# 列表显示
ls -l
ll

# 文件容量大小以适当形式显示
ls -h

# 显示inode号
ls -i

# 显示UID和GID
ls -n

# 显示完整时间
ls --full-time

# 显示其他时间，默认显示mtime内容更新时间
# atime - 读取时间， ctime - 状态更新时间
ls --time={atime, ctime}

# 按文件大小排序
ls -S

# 按时间排序
ls -t

# 设置文件类型颜色的显示方式
# never - 不显示, always - 显示, auto - 自动设置
ls --color={never, always, auto}

# 显示表示文件类型的符号
# *:代表可可执行文件； /:代表目录； =:代表 socket 文件； |:代表 FIFO 文件；
ls -F
```

示例

```bash
ls -la
ls -la .bash*
ls -ld test*
```

### touch

创建文件或修改文件时间

```bash
# 文件不存在时创建文件
# 文件存在时更新mtime/ctime/atime为当前时间
touch <file>

# 文件不存在时不创建文件
# 文件存在时更新mtime/ctime/atime为当前时间
touch -c <file>

# 修改mtime为当前时间
touch -m <time> <file>

# 修改atime为当前时间
touch -a <time> <file>

# 修改mtime/atime为指定时间
touch -d <time> <file>
touch -t <YYYYMMDDhhmm> <file>
```

示例

```bash
touch -d "2 days ago" bashrc
touch -t 201406150202 bashrc
```

### cp

复制文件或目录

```bash
# 复制文件
# 默认复制原始文件，即复制软链接文件时复制其对应的真实文件
cp <source> <dest>

# 复制多个文件
cp <...source> <dir>

# 复制目录，递归复制
cp -r <dir> <dest>

# 源文件为软连接文件时复制软连接文件
cp -d <source> <dest>

# 复制成软链接文件
cp -s <source> <dest>

# 复制成硬链接文件
cp -l <source> <dest>

# 文件属性一起复制
cp -p <source> <dest>

# 所有属性一起复制，包括文件属性、 SELinux属性、links、xattr 等
cp --preserve=all <source> <dest>

# 备份，相当于-dr --preserve=all
cp -a <source> <dest>

# 覆盖时询问
cp -i <source> <dest>

# 目标文件不存在或比源文件旧时才复制
cp -u <source> <dest>
```

### mv

移动文件或目录

```bash
# 移动文件或目录
mv <source> <dest>

# 移动多个文件或目录
mv <...source> <dir>

# 覆盖时询问
mv -i <source> <dest>

# 强制移动，覆盖时不询问
mv -f <source> <dest>

# 目标文件不存在或比源文件旧时才移动
mv -u <source> <dest>
```

### rm

移除文件或目录

```bash
# 删除文件
rm <file>

# 删除目录，递归删除
rm -r <dir>

# 删除时询问
rm -i <file>

# 强制删除
rm -f <file>
```

示例

```bash
rm -i bashrc*
```

### file

查看文件基本信息

```bash
# 查看文件基本信息
file <file>
```

示例

```bash
# 纯文本文件
file ~/.bashrc
/root/.bashrc: ASCII text

# 二进制文件
file /usr/bin/passwd
/usr/bin/passwd: setuid ELF 64-bit LSB shared object, x86-64, version 1 （SYSV）, dynamically
linked （uses shared libs）, for GNU/Linux 2.6.32,
BuildID[sha1]=0xbf35571e607e317bf107b9bcf65199988d0ed5ab, stripped

# 数据文件
file /var/lib/mlocate/mlocate.db
/var/lib/mlocate/mlocate.db: data
```

## 文件内容查阅

### cat

从第一行开始显示文件内容

```bash
# 从第一行开始查阅文件内容
cat <file>

# 所有行列出行号
cat -n <file>

# 非空白行列出行号
cat -b <file>

# 断行符显示$
cat -E <file>

# Tab符显示^I
cat -T <file>

# 显示特殊字符
cat -v <file>

# 显示所有字符，相当于-vET
cat -A <file>
```

### tac

从最后一行开始显示文件内容

```bash
# 从最后一行开始查阅文件内容
cat <file>
```

### nl

显示文件内容时显示行号

```bash
# 从第一行开始查阅文件内容，并显示行号
nl <file>

# 设置何处显示行号，默认空白行不显示行号
# a - 所有行，t - 非空白行
nl -b {a, t} <file>

# 设置行号显示方式，默认左对齐
# ln - 左对齐，rn - 右对齐且不加0, ，rz - 右对齐且加0
nl -n {ln, rn, rz} <file>

# 设置行号字段占的字符数，默认占6个字符
nl -w <num> <file>
```

### more

按页显示文件内容，不可以向前翻页

```bash
# 进入more翻页查阅环境
more <file>
```

* `Space`: 向下翻页
* `Enter`: 向下翻一行
* `/<string>`: 向下搜索字符串
* `:f`: 显示翻页信息，即当前文件名和行数
* `q`: 退出

### less

按页显示文件内容，可以向前翻页

```bash
# 进入more翻页查阅环境
more <file>
```

* `Space`: 向下翻页
* `PageDown`: 向下翻页
* `PageUp`: 向上翻页
* `/<string>`: 向下搜索字符串
* `?<string>`: 向上搜索字符串
* `n`: 下一个搜索
* `N`: 上一个搜索
* `g`: 跳到第一行
* `N`: 跳到最后一行
* `q`: 退出

### head

显示头几行

```bash
# 显示头几行，默认为10行
head <file>

# 设置显示的行数，加-为显示到最后num行
head -n <num> <file>
```

### tail

显示尾几行

```bash
# 显示尾几行，默认为10行
tail <file>

# 设置显示的行数，加+为显示到前num行
tail -n <num> <file>

# 持续侦测文件
tail -f <file>
```

示例

```bash
head -n 20 index.js | tail -n 10 | cat -n
```

### od

以二进制的方式读取内容

```bash
# 以二进制方式读取并按指定方式输出
# a - 默认字符输出，c - ASCII字符输出，d - 十进制整数输出，f - 浮点数输出，o - 八进制输出，x - 十六进制输出
od -t {a, c, d, f, o, x} <file>
```

示例

```bash
echo password | od -t xCc
```

## 文件搜索

### which

在$PATH下搜索命令对应的可执行文件

```bash
# 搜索$PATH下命令对应的可执行文件
which <command>

# 搜索$PATH下命令对应的所有可执行文件
which -a <command>
```

### whereis

在特定目录下搜索文件或目录

```bash
# 列出查找的特定目录
whereis -l

# 查找特定目录下的文件或目录
whereis <file>

# 只找二进制文件
whereis -b <file>

# 只找说明文档
whereis -m <file>

# 只找源文件
whereis -s <file>

# 查找二进制文件、说明文档、源文件之外的文件
whereis -u <file>
```

### locate

在数据库`/var/lib/mlocate/`下搜索文件（每天更新一次）

```bash
# 输出数据库文件信息
locate -S

# 更新数据库
updatedb

# 在/var/lib/mlocate/里搜索文件
locate <keyword>

# 忽略大小写
locate -i <keyword>

# 正则匹配
locate -r <keyword>

# 显示前几行
locate -l <keyword>

# 显示找到的文件数量
locate -c <keyword>
```

### find

在硬盘下搜索文件

```bash
# 搜索指定目录下的文件
find <path>

# 搜索多个目录下的文件
find <...path>

# 按文件类别查找
# type: d - 目录，f - 普通文件，l - 软连接，b - 块设备，c - 字符设备，s - 接口文件，p - 传输文件
find <path> -type <type>

# 按权限查找
# <mode> - 文件权限等于mode, -<mode> - 文件权限囊括全部mode, <mode> - 文件权限囊括任一身份mode
find <path> -perm {<mode>, -<mode>, /<mode>}

# 按uid查找
find <path> -uid <n>

# 按用户名查找
find <path> -user <name>

# 查找文件拥有者不在/etc/passwd中的文件
find <path> -nouser

# 按gid查找
find <path> -gid <n>

# 按群组名查找
find <path> -group <name>

# 查找文件所属群组不在/etc/group中的文件
find <path> -nogroup

# 按文件尺寸查找
# +代表大于size，-代表小于size
# size: c - Bytes, k - kB, M - MB
find <path> -size [+-]<size>

# 按时间查找
# mtime - 更改时间，ctime - 属性更改时间，atime - 访问时间
# num: -n - n天内，n - 前n+1天到前n天，+n - n+1天前
find <path> {-mtime, -ctime, -atime} <num>

# 查找比指定文件新的文件
find <path> -newer <file>

# 按文件名查找
find <path> -name <file>

# 对搜索到的文件进行处理
# {} - 找到的文件，\; - 处理结束
find <path> -exec <command>
```

示例

```bash
find /run -type s
find / -perm /7000
find /home -user dmtsai
find / -nouser
find / -size +1M
find / -mtime 0
find /etc -newer /etc/passwd
find / -name passwd
find /etc -name '*httpd*'
find /usr/bin /usr/sbin -perm /7000 -exec ls -l  {} \;
```
