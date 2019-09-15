# Shell

标签（空格分隔）： Linux基础

---

## 介绍

与操作系统核心沟通的壳

* `sh`: Bourne Shell，第一个Shell
* `csh`: C Shell，语法类似C语言
* `bash`: Bourne Again Shell，sh的增强版本
* `tcsh`: csh的增强版本

```bash
# 查看可使用的shells
cat /etc/shells

# 查看登录后取得的shell
cat /etc/passwd
```

## 命令

```bash
$ command [options] ...arg
```

* `command`: 命令或可执行文件（绝对路径/相对路径），如`cd`
* `options`: 选项，可多个，如`-h, --help`
* `arg`: 参数，可多个
* `Space`: 分隔命令、选项、参数
* `\Enter`: 多行命令
* `Enter`: 执行命令
 * 输出执行结果
 * 进入执行程序工作环境

## 快捷键

* `Tab`: 自动补全
* `Ctrl + A`: 跳转到命令开头
* `Ctrl + E`: 跳转到命令结尾
* `Ctrl + U`: 删除光标到命令开头的字符
* `Ctrl + K`: 删除光标到命令结尾的字符
* `Ctrl + C`: 中断命令
* `Ctrl + D`: 退出
* `Shift + PageUp`: 向前翻页
* `Shift + PageDown`: 向后翻页

## $PATH

可执行文件路径

* 冒号分开的一堆目录路径
* 执行命令时会去$PATH下按目录顺序搜寻命令的可执行文件并执行
* 不同使用者的默认$PATH不同

```bash
# 显示可执行文件路径
echo $PATH

# 添加目录到$PATH
PATH="${PATH}:<dir>"
```

## 帮助文档

### 命令帮助

```bash
<command> --help
```

### man page

操作说明文档

#### 位置

```bash
# man page存放位置
/usr/share/man
/usr/local/man

# man page配置文件
/etc/man_db.conf
```

#### 类型

| 代号 | 代表内容 |
| --- | --- |
| 1	| 使用者在shell环境中可以操作的指令或可可执行文件 |
| 2	| 系统核心可调用的函数与工具等 |
| 3	| 一些常用的函数（function）与函数库（library），大部分为C的函数库（libc） |
| 4	| 设备文件的说明，通常在/dev下的文件 |
| 5	| 配置文件或者是某些文件的格式 |
| 6	| 游戏（games） |
| 7	| 惯例与协定等，例如Linux文件系统、网络协定、ASCII code等等的说明 |
| 8	| 系统管理员可用的管理指令 |
| 9	| 跟kernel有关的文件 |

#### 命令

```bash
# 进入man环境，显示man page
man <command>

# 搜索所有man page（完全匹配）
man -f <command>

# 搜索所有man page（关键字匹配）
man -k <command>
```

#### 快捷键

| 按键 | 描述 |
| --- | --- |
| 空白键 | 向下翻页 |
| [Page Down] | 向下翻页 |
| [Page Up] | 向上翻页 |
| [Home] | 去到第一页 |
| [End] | 去到最后一页 |
| d | 向下翻半页 |
| u | 向上翻半页 |
| /string | 向下搜寻string字符串 |
| ?string | 向上搜寻string字符串 |
| n | 下一个搜索到的字符串 |
| N | 上一个搜索到的字符串 |
| q | 退出 |

### info page

info文档

#### 位置

```bash
# info page存放位置
/usr/share/info
```

#### 命令

```bash
# 进入info环境，显示info文档
info <command>
```

### 快捷键

| 按键 | 描述 |
| --- | --- |
| 空白键 | 向下翻页 |
| [Page Down] | 向下翻页 |
| [Page Up] | 向上翻页 |
| [tab] | 在node之间移动，有node的地方，通常会以*显示 |
| [Enter] | 当光标在node上面时，按下Enter可以进入该node |
| b | 移动光标到该info画面当中的第一个node处 |
| e | 移动光标到该info画面当中的最后一个node处 |
| n | 前往下一个node |
| p | 前往上一个node |
| u | 前往父node |
| s（/） | 搜索字符串 |
| h, ? | 显示帮助说明 |
| q | 退出 |

### 其他文档

额外的说明文档

#### 位置

```bash
# 存放位置
/usr/share/doc
```

## nano编辑器

```javascript
# 文件不存在，新建文件
# 文件存在，打开文件
# 进入nano编辑器环境
nano <file>
```

### 快捷键

* `Ctrl + G`: 获取帮助
* `Ctrl + X`: 离开
* `Ctrl + O`: 保存
* `Ctrl + W`: 搜索

## 基本命令

### 语言命令

```bash
# 查看支持的语言
locale -a

# 查看当前的语言变量
locale

# 修改为英文
LANG=en_US.utf8 # 主语言环境
export LC_ALL=en_US.utf8 # 整体语系的环境
```

### 日期命令

```bash
# 显示当前时间
date

# 日期格式化
date +%Y/%m/%d
date +%H:%M
```

### 日历命令

```bash
# 显示当前月日历
cal

# 显示某年某月日历
cal [month] [year]
```

### 计算器命令

```bash
# 进入计算机工作环境，默认输出整数
bc

# 输出number位小数点
scale=<number>

# 运算符
+ - * / ^ %

# 退出
quit
```
