# 概述

标签（空格分隔）： Linux基础

---

开源的操作系统

* 可移植性：程序码可以被修改成适合在各种机器上面运行
* GPL授权：通用公共许可证
* POSIX设计规范：相容于Unix操作系统

## 特性

* 多用户：允许多人同时连上主机，每个使用者皆有其各人的使用环境，并且可以同时使用系统的资源
* 多任务：CPU与其他例如网络资源可以同时进行多项工作
* 文件系统：一切皆文件

## 核心版本

```bash
# 查看核心版本
uname -r
3.10.0-123.el7.x86_64 // 主版本.次版本.发布版本-修改版本

# 查看操作系统的位版本
uname -m
x86_64
```

## 发行版

Linux核心 + 自由软件 + 工具

* 使用RPM方式安装软件的系统：Red Hat, Fedora, SuSE等
* 使用dpkg方式安装软件的系统：Debian, Ubuntu, B2D等

## 文件系统

以根目录为主，然后向下呈现分支的目录结构

![目录树](https://raw.githubusercontent.com/wchaochao/images/master/gitbook-linux-base/dirtree.gif)

### 挂载

利用一个目录当成进入点，将磁盘分区的数据放置在该目录下

![挂载](https://raw.githubusercontent.com/wchaochao/images/master/gitbook-linux-base/mount.png)
