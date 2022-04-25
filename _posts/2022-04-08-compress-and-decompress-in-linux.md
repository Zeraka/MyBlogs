---
layout: post
title: Linux下不同压缩格式文件的解压方式
subtitle: 
tags: [linux]
comments: true
---
# Linux下不同压缩格式文件的解压方式

Linux下的压缩文件的后缀名有 `*.tar, *.tar.gz, *.tgz, *.gz, *.Z, *.bz2`。这代表这些文件被不同的压缩技术所压缩，因此解压缩时便使用不同的压缩命令。Linux文件的扩展名并不会改变文件性质，而只是让你清楚这是什么格式文件，因此如果随便乱改Linux文件扩展名会造成肉眼判断的障碍。

```
*.Z compress 命令压缩的文件；
*.gz gzip 命令压缩的文件
*.bz2 bzip2 程序压缩的文件
*.tar tar程序打包的数据，并没有压缩过
*.tar.gz tar程序打包的文件，并且经过gzip压缩过
*.tar.bz2 tar程序打包的文件，并且经过bzip2压缩过
*.taz *.tar.Z 用tar打包的文件，经过compress压缩
```

tar文件格式是posix标准，tar命令常用在Linux中文件打包压缩与解压缩。

如何记住这些扩展名及其对应的解压缩命令？

必须首先记住tar的命令行参数， 可用 tar --help查看。

```
-c   即 create, 创建新的文档， 用于压缩命令， 结合 j/z/Z， 可以生产压缩文件。
-v	 显示详细的tar处理文件的信息， view, 
-f   file, 即要操作的文件名
-j   --bzip2,  解压bzip2文件。
-z   --gzip   --gunzip --ungzip  用于gzip文件
-x	 解压必选参数，  extract之意。  
-Z 大写Z, 用于compress命令压缩的文件
```

gzip就是gnu zip。所以解压tar.gz文件，可以用 `tar xzvf file.tar.gz`

如果是tar.bz2文件，则用 `tar xjvf file.tar.bz2`

如果要压缩一个文件， 可以 `tar cjvf file.tar.bz2  file`

如果解压缩到特定目录，可以在以上命令行末尾 加 `-C path`参数， 大写C。

解压文件中特定的文件，可以

`tar zxvf file.tar.gz  *.txt`

## 参考

[tar压缩](https://www.cnblogs.com/nuo010/p/16012283.html)

[Linux下压缩的命令以及对应扩展名](https://blog.csdn.net/lc522108813/article/details/45601861?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1.pc_relevant_antiscanv2&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1.pc_relevant_antiscanv2&utm_relevant_index=1)
