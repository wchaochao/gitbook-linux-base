# 文件压缩

标签（空格分隔）： Linux基础

---

对文件进行压缩，降低文件大小

## 扩展名

```
*.Z         compress 程序压缩的文件；
*.zip       zip 程序压缩的文件；
*.gz        gzip 程序压缩的文件；
*.bz2       bzip2 程序压缩的文件；
*.xz        xz 程序压缩的文件；
*.tar       tar 程序打包的数据，并没有压缩过；
*.tar.gz    tar 程序打包的文件，其中并且经过 gzip 的压缩
*.tar.bz2   tar 程序打包的文件，其中并且经过 bzip2 的压缩
*.tar.xz    tar 程序打包的文件，其中并且经过 xz 的压缩
```

* 压缩：将一个文件压缩
* 打包：将多个文件打包成一个大文件

## 压缩

### gzip

gzip压缩，用于取代compress压缩

```bash
# 将文件压缩成.gz文件，替换原文件
gzip <file>

# 压缩数据显示在屏幕
gzip -c <file>

# 压缩并保留原文件
gzip -c <file> > <file.gz>

# 显示压缩比等信息
gzip -v <file>

# 设置压缩等级，默认为-6，1最快，9最慢
gzip -<n> <file>

# 检验压缩是否正确
gzip -t <file>

# 读取文本文件的压缩文件
zcat/zmore/zless <file>

# 在文本压缩文件中查找
zgrep '<word>' <file>

# 将压缩文件解压缩，替换原压缩文件
gzip -d <file>
```

### bzip2

bzip2压缩，用于取代gzip压缩（时间久些）

```bash
# 将文件压缩成.bz2文件，替换原文件
bzip2 <file>

# 将文件压缩成.bz2文件，不替换原文件
bzip2 -k <file>

# 压缩数据显示在屏幕
bzip2 -c <file>

# 显示压缩比等信息
bzip2 -v <file>

# 设置压缩等级，默认为-6，1最快，9最慢
bzip2 -<n> <file>

# 读取文本文件的压缩文件
bzcat/bzmore/bzless <file>

# 将压缩文件解压缩，替换原压缩文件
bzip2 -d <file>
```

### xz

xz压缩，用于取代bzip2压缩（时间更久）

```bash
# 将文件压缩成.xz文件，替换原文件
xz <file>

# 将文件压缩成.xz文件，不替换原文件
xz -k <file>

# 压缩数据显示在屏幕
xz -c <file>

# 显示压缩比等信息
xz -v <file>

# 显示压缩信息
xz -l <file>

# 设置压缩等级，默认为-6，1最快，9最慢
xz -<n> <file>

# 检验压缩是否正确
xz -t <file>

# 读取文本文件的压缩文件
xzcat/xzmore/xzless <file>

# 在文本压缩文件中查找
xzgrep '<word>' <file>

# 将压缩文件解压缩，替换原压缩文件
xz -d <file>
```

## 打包

### tar

打包文件

```bash
# 打包文件(tarfile)
tar -c -f <dest> <...source>

# 打包文件并显示详细信息
tar -cv -f <dest> <...source>

# 保留原文件的权限和属性
tar -pcv -f <dest> <...source>

# 排除掉某些文件
tar -cv -f <dest> --exclude=<exclude-file> <...source>

# 只打包比某个时间新的文件
tar -cv -f <dest> --newer-mtime=<time> <...source>

# 打包文件到设备上
tar -cv -f <device> <...source>

# 打包并压缩(tarball)
# z - gzip压缩，j - bzip2压缩, J - xz压缩
tar -{z, j, J}cv -f <dest> <...source>

# 查看打包文件
tar -tv -f <source>

# 查看压缩的打包文件
tar -{z, j, J}tv -f <source>

# 解打包
tar -xv -f <source> -C <dest>

# 解打包中的某个文件
tar -xv -f <source> <filename>

# 解压缩
tar -{z, j, J}xv -f <source> -C <dest>

# 边打包边解包
tar -cvf - <source> | tar -xvf - -C <dest>
```
