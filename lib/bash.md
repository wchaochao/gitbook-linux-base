# Bash

标签（空格分隔）： Linux基础

---

Bourne Again Shell，Linux的默认Shell

## 特点

* 历史记录：在内存中记录本次登录执行过的命令，登出时会记录到~/.bash_history中
* Tab补全：使用Tab补全命令或文件名
* 别名设置：使用alias查看和设置命令别名
* 后台任务：可将当前任务丢到后台去执行
* shell脚本：使用sell脚本执行一系列指令
* glob匹配：可使用glob进行匹配

```bash
# 查看别名
alias

# 设置别名
alias <name>="command [options] ...arg"
```

## 内置命令

bash中内置了一些命令

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

## 子bash

```bash
# 进入子bash
bash

# 退出子bash
exit
```

## 变量

以一组文字或符号等，来取代一些设置或者是一串保留的数据

* 环境变量：系统默认变量，全局变量，全大写，如PATH、HOME、MAIL、SHELL等
* 自定义变量

```bash
# 显示变量值
echo $<variable>
echo ${<variable>}

# 设置变量
<variable>=<value> # 变量名有字母和数字组成且数字不开头，等号左右无空格，value中无空格
<variable>="<value>" # 特殊字符仍保留原特性，如$LANG为LANG变量，可用\转义
<variable>='<value>' # 特殊字符为一般字符，如$LANG为$LANG
<variable>=$<variable1>value # 累加
<variable>="$<variable1>"value # 累加
<variable>=${<variable1>}value # 累加
<variable>=$(<command [options] ...arg>) # 设置变量为command输出

# 设为环境变量
export <variable> # 将变量变为环境变量

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
```
