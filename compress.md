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

* 打包：将多个文件打包成一个文件
* 压缩：将一个文件压缩

## 操作

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

bzip2压缩，用于取代gzip压缩
