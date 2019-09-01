# Bash

标签（空格分隔）： Linux基础

---

Bourne Again Shell，Linux的默认Shell

* 历史记录：在内存中记录本次登录执行过的命令，登出时会记录到~/.bash_history中
* Tab补全：使用Tab补全命令或文件名
* 别名设置：使用alias查看和设置命令别名
* 后台任务：可将当前任务丢到后台去执行
* shell脚本：使用sell脚本执行一系列指令
* glob匹配：可使用glob进行匹配

## 变量

以一组文字或符号等，来取代一些设置或者是一串保留的数据

* 环境变量
* 接口变量
* 自定义变量

### 环境变量

* 系统默认变量，通常被设置为大写字符
* 全局变量，可被子程序访问

```bash
# 查看所有环境变量
env
export
```

* `HOME`: 当前使用者的主文件夹
* `PATH`: 可执行文件路径
* `MAIL`: 邮箱地址
* `LANG`: 语言
* `RANDOM`: 随机数，[0, 32767]的整数
* `SHELL`: 当前终端的Shell程序
* `HISTSIZE`：历史命令的最大条数

示例

```bash
# 产生[0-9]的整数
declare -i number=$RANDOM*10/32768 ; echo $number
```

### 接口变量

与shell操作接口有关的变量，通常被设置为大写字符

```bash
# 查看所有环境变量和接口变量
set
```

* `PS1`：命令提示符
 * `\u`: 使用者的账号
 * `\h`: 主机名第一个小数点前的名称
 * `\H`: 完整主机名
 * `\w`: 完整的工作目录
 * `\W`: 工作目录的basename
 * `\d`：日期"星期月日"
 * `\t`: 24小时的"HH:MM:SS"
 * `\T`: 12小时的"HH:MM:SS"
 * `\A`: 24小时的"HH:MM"
 * `\@`: 12小时的"am/pm"
 * `\v`: bash的版本
 * `\#`: 下达的第几个指令
 * `\$`: 提示字符，root为#，user为$
* `$`: Shell的PID（线程ID）
* `?`: 上一个指令的回传值
 * 成功回传0
 * 失败回传错误代码

示例

```bash
# PS1
[dmtsai@study ~]$ cd /home
[dmtsai@study home]$ PS1='[\u@\h \w \A #\#]\$ '
[dmtsai@study /home 17:02 #85]$

# $
echo $$
```

### 自定义变量

当前bash程序的变量，不能被子程序访问

```bash
# 将自定义变量设为环境变量
export <variable>
```

## 变量操作

### echo

输出变量值

```bash
# 输出变量值
echo $<variable>
echo ${<variable>}
```

示例

```bash
echo $PATH
echo ${PATH}
```

### 设置

设置变量值

```bash
# 设置变量
<variable>=<value> # 变量名由字母和数字组成且数字不开头，等号左右无空格，value中无空格
<variable>="<value>" # 特殊字符仍保留原特性，如$LANG为LANG变量，可用\转义
<variable>='<value>' # 特殊字符为一般字符，如$LANG为$LANG
<variable>=$<variable1>value # 使用变量
<variable>=${<variable1>}value # 使用变量
<variable>="$<variable1>"value # 使用变量
<variable>=$(<command [options] ...arg>) # 设置变量为command输出

# 取消设置
unset <variable>
```

示例

```bash
myname=Vbird
myname="$PATH"
myname="\$PATH"
myname='$PATH'
myname=$PATH
myname=$PATH:home/bin
myname="$myname"yes
myname=${myname}yes
myname=$(uname -r)
cd /lib/modules/$(uname -r)/kernel
```

### read

读取用户输入置为变量值

```bash
# 读取用户输入置为变量值
read <variable>

# 设置提示语
read -p <prompt> <variable>

# 设置等待秒数
read -t <n> <variable>
```

### declare

声明变量类型，默认为字符串

```bash
# 查看所有变量
declare

# 查看变量声明
declare -p <variable>

# 变量声明为整数
declare -i <variable>

# 变量声明为数组
declare -a <variable>

# 变量声明为只读
declare -r <variable>

# 变量声明为环境变量
declare -x <variable>

# 取消声明为环境变量
declare +x <variable>
```

示例

```bash
sum=100+300+50
echo ${sum} # 100+300+50

declare -i sum=100+300+50
echo ${sum} # 450
```

### 过滤

过滤开头或结尾

```bash
# 从开头开始过滤最短匹配
${<variable>#<match>}

# 从开头开始过滤最长匹配
${<variable>##<match>}

# 从结尾开始过滤最短匹配
${<variable>%<match>}

# 从结尾开始过滤最长匹配
${<variable>%%<match>}
```

示例

```bash
mail=/var/spool/mail/dmtsai

echo ${mail#/*/} # spool/mail/dmtsai
echo ${mail##/*/} # dmtsai
echo ${mail%/*} # /var/spool/mail
echo ${mail%%/spool/*} # /var
```

### 替换

```bash
# 替换第一个
${<variable>/old/new}

# 替换所有
${<variable>//old/new}
```

示例

```bash
mail=/var/spool/mail/spool/dmtsai

echo ${mail/spool/aaa} # /var/aaa/mail/spool/dmtsai
echo ${mail//spool/aaa} # /var/aaa/mail/aaa/dmtsai
```

### 条件

```bash
# var2存在时，var1为var2的值
# var2不存在时，var1为expr
var1=${var2-expr}

# var2存在且不为空字符串时，var1为var2的值
# var2不存在或为空字符串时，var1为expr
var1=${var2:-expr}

# var2存在时，var1为var2的值
# var2不存在时，输出expr
var1=${var2?expr}

# var2存在且不为空字符串时，var1为var2的值
# var2不存在或为空字符串时，输出expr
var1=${var2:?expr}

# var2存在时，var1为var2的值
# var2不存在时，var1、var2为expr
var1=${var2=expr}

# var2存在且不为空字符串时，var1为var2的值
# var2不存在或为空字符串时，var1、var2为expr
var1=${var2:=expr}

# var2存在时，var1为expr
# var2不存在时，var1为空字符串
var1=${var2+expr}

# var2存在且不为空字符串时，var1为expr
# var2不存在或为空字符串时，var1为空字符串
var1=${var2:+expr}
```

## 别名

```bash
# 查看别名
alias

# 设置别名
alias <name>='command [options] ...arg'

# 取消别名
unalias <name>
```

示例

```bash
alias lm='ls -la | more'
alias rm='rm -i'
alias vi='vim'
alias cls='clear'
```

## 历史命令

### history

* 登录时会读取~/.bash_history中的命令到当前Shell的历史命令中
* 登出时会将dq

```
# 查看历史命令
history

# 查看最近n条
history <n>

# 清除历史命令
history -c

# 将histfile的内容读入到历史命令
history -r [histfile]

# 将历史命令写入指定文件中，默认为~/.bash_history
history -w [histfile]
```

## 常用命令

### type

查看命令信息

```bash
# 显示命令介绍（内置命令、外部命令、命令别名）
type <command>

# 显示命令类型
# builtin - 内置命令，file - 外部命令，alias - 命令别名
type -t <command>

# 查看外部命令的完整文件名
type -p <command>

# 查看命令的所有使用情况
type -a <command>
```

### bash

子bash程序

```bash
# 进入子bash
bash

# 退出子bash
exit
```

### ulimit

资源配额

```bash
# 查看当前bash的所有配额
ulimit -a

# 设置配额
# -a中有说明如何设置，如-f <size>，设置新建文件的最大容量
ulimit <limit>
```
