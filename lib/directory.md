# 目录

标签（空格分隔）： Linux基础

---

## 配置

FHS（Filesystem Hierarchy Standard）标准

* `/`: root, 根目录，与开机系统有关
* `/usr`: unix software resource, 与软件安装/执行有关
* `/var`: variable, 与系统运行过程有关

### 根目录

必须存在

* `/bin`: 一般的可执行文件
* `/boot`: 开机配置文件
* `/dev`: 设备文件
* `/etc`: 系统配置文件
* `/lib`: 函数库
* `/media`: 可移除设备
* `/mnt`: 暂时挂载设备
* `/opt`: 第三方软件
* `/run`: 运行信息
* `/sbin`: 系统管理员常用指令集
* `/srv`: 网络服务
* `/tmp`: 临时文件
* `/usr`: 软件资源
* `/var`: 系统运行数据

建议存在

* `/root`: 管理员主文件夹
* `/home`: 使用者主文件夹
* `/lib<qual>`: 与 /lib 不同格式的二进制函数库

其他

* `/lost+found`: ext2/ext3/ext4文件系统格式的发生错误时的遗失片段
* `/proc`: 虚拟文件系统（在内存中），记录系统核心信息
* `/sys`: 与proc类似

### /usr

必须存在

* `/usr/bin/`: 一般的可执行文件
* `/usr/lib/`: 函数库
* `/usr/local/`: 自行安装的软件
* `/usr/sbin/`: 系统管理员常用指令集
* `/usr/share/`: 共享文件

建议存在

* `/usr/games/`: 游戏相关
* `/usr/include/`: c/c++程序文件
* `/usr/libexec/`: 不常用的可执行文件
* `/usr/lib<qual>/`: 与 /lib 不同格式的二进制函数库
* `/usr/src/`: 源代码

### /var

必须存在

* `/var/cache/`: 程序缓存
* `/var/lib/`: 程序执行需要的文件
* `/var/lock/`: 锁定文件
* `/var/log/`: 日志
* `/var/mail/`: 电子邮箱
* `/var/run/`: 程序运行信息
* `/var/spool/`: 等待程序使用的数据

## 目录树

* 目录树的启始点为根目录
* 每一个目录不止能使用本地端的 partition 的文件系统，也可以使用网络上的 filesystem
* 每一个文件在此目录树中的文件名（包含完整路径）都是独一无二的

![根目录](https://raw.githubusercontent.com/wchaochao/images/master/gitbook-linux-base/rootdir.jpg)

## 路径

* 绝对路径：由根目录（/）开始写起的文件名或目录名
* 相对路径：相对于目前路径的文件名或目录名

### 特殊路径

* `.`: 当前目录
* `..`: 父目录
* `-`: 上一个工作目录
* `~`: 当前使用者的主目录
* `~<account>`: account的主目录

### dirname

获取路径的目录名

```bash
# 获取路径的目录名
dirname <path>
```

### basename

获取路径的文件名

```bash
# 获取路径的目录名
basename <path>
```

## 目录操作

### pwd

显示当前工作目录（Print Working Directory）

```bash
# 显示当前工作目录
pwd

# 显示真实路径，而非链接文件
pwd -P
```

### cd

更换工作目录（Change Directory）

```bash
# 进入主目录
cd
cd ~

# 进入某个使用者的主目录
cd ~<count>

# 进入父目录
cd ..

# 进入上一个工作目录
cd -

# 进入绝对路径目录
cd <absolute url>

# 进入相对路径目录
cd <relative url>
```

### mkdir

创建空目录（make directory）

```bash
# 创建空目录
mkdir <dir>

# 上级目录未创建时递归创建父目录
mkdir -p <dir>

# 创建目录时设置目录权限
mkdir -m <mode> <dir>
```

### rmdir

删除空目录(remove directory)

```bash
# 删除空目录
rmdir <dir>

# 连上层空目录一起删除
rmdir -p <dir>
```
