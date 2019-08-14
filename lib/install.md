# 安装

标签（空格分隔）： Linux基础

---

## Centos7安装

* U盘刻录Centos7镜像，进入BIOS，选择U盘开机或者使用虚拟机安装
* 选择安装模式，使用GPT分区
* 设置语言、时间、键盘
* 软件选择
* 磁盘分区
* 网络设置
* 设置root密码，添加用户

![centos7-gpt](https://raw.githubusercontent.com/wchaochao/images/master/gitbook-linux-base/centos7_gpt.jpg)

## 术语

* 开机：启动Centos7
* 关机：关闭Centos7
* 登录：账号登录，远程登录使用`ssh <user>@<ip> -p <port>`命令
* 登出：账号登出，使用`exit`命令
* `Shell`: 和使用者沟通的程序，默认为`bash`
* 终端切换：`Ctrl + Alt + [F1 - F6]`，默认提供tty1~tty6的终端界面
* `~`: 当前账号的主文件夹
* 提示符：默认root为`#`, 普通账号为`$`

## 账号

```bash
# 账号记录
/etc/passwd

# 密码记录
/etc/shadow

# 群组记录
/etc/group
```

### 账号命令

```bash
# 远程登录
ssh -p <port> user@host

# 切换管理员账号
su -

# 退出
exit

# 使用管理员身份执行命令
sudo <command>
```

## 关机

不正常关机，可能会造成文件系统的毁损

* 观察系统的使用状态
* 通知线上使用者关机的时刻
* 正确的关机命令使用

```bash
# 查看线上使用者
who

# 查看网络情况
netstat -a

# 查看后台进程
ps -aux
```

### sync

将内存中的数据同步到硬盘中

```bash
# 数据同步写入硬盘中
sync
```

### shutdown

关机命令

* 可以自由选择关机模式
* 可以设置关机时间
* 可以自订关机讯息
* 可以仅发出警告讯息

```bash
# shutdown命令
shutdown [-krhc] [时间] [警告信息]
-k     ： 只发送警告信息，不关机
-r     ： 在将系统的服务停掉之后重新开机
-h     ： 在将系统的服务停掉之后立即开机
-c     ： 取消已经在进行的shutdown命令
时间   ： 指定shutdown执行的时间，默认为1分钟
警告   ： 给线上使用者发送警告信息

# example
shutdown -h now // 立即关机
shutdown -h 20:25 // 20.25关机
shutdown -h +10 // 10分钟后关机
shutdown -k now 'This system will reboot' // 发送警告信息
```

### 其他关机指令

```bash
# 系统停止
halt

# 系统关机
poweroff

# 系统重启
reboot
```

### systemctl关机

```bash
# 使用systemctl关机
systemctl [指令]
halt       进入系统停止的模式，屏幕可能会保留一些讯息，这与你的电源管理模式有关
poweroff   进入系统关机模式
reboot     直接重新开机
suspend    进入休眠模式
```
