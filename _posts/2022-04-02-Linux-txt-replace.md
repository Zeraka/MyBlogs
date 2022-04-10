---
layout: post
title: Linux批量替换文本
subtitle: 脚本运维
tags: [Linux, Performance, automation, 自动化运维]
---
## Linux下快速批量替换文本

有一批配置文件，名字以4xxxvf开头，以dev0,dev1,dev2结尾，形式上例如：4xxxvf_dev0.conf, 4xxxvf_dev1.conf, 一共有100个左右的文件，这些文件中内容绝大部分相同，现在需要把文件中的某一处修改——有一行是

```
ServicesEnabled=sym
```

修改为

```
ServicesEnabled=asym
```

一共100个文件，如果手动添加，时间耗费非常大，因此必须编写脚本批量修改。

## perl命令行

Perl语言非常适合进行文本处理。Perl可以使用命令行快速批量处理文本。并且perl是所有Linux中默认安装的。

```sh
# perl -i -e "s/Old/new/g" file1 file2 file3 file4 file5
```

Perl参数：

```
Usage: perl [switches] [--] [programfile] [arguments]
  -0[octal]         specify record separator (\0, if no argument)
  -a                autosplit mode with -n or -p (splits $_ into @F)
  -C[number/list]   enables the listed Unicode features
  -c                check syntax only (runs BEGIN and CHECK blocks)
  -d[:debugger]     run program under debugger
  -D[number/list]   set debugging flags (argument is a bit mask or alphabets)
  -e program        one line of program (several -e's allowed, omit programfile)
  -E program        like -e, but enables all optional features
  -f                don't do $sitelib/sitecustomize.pl at startup
  -F/pattern/       split() pattern for -a switch (//'s are optional)
  -i[extension]     edit <> files in place (makes backup if extension supplied)
  -Idirectory       specify @INC/#include directory (several -I's allowed)
  -l[octal]         enable line ending processing, specifies line terminator
  -[mM][-]module    execute "use/no module..." before executing program
  -n                assume "while (<>) { ... }" loop around program
  -p                assume loop like -n but print line also, like sed
  -s                enable rudimentary parsing for switches after programfile
  -S                look for programfile using PATH environment variable
  -t                enable tainting warnings
  -T                enable tainting checks
  -u                dump core after parsing program
  -U                allow unsafe operations
  -v                print version, patchlevel and license
  -V[:variable]     print configuration summary (or a single Config.pm variable)
  -w                enable many useful warnings
  -W                enable all warnings
  -x[directory]     ignore text before #!perl line (optionally cd to directory)
  -X                disable all warnings
```

-e代表采用perl命令行。

-i 代表在原文件中编辑。

-p 非常重要，-p是-n的超集，对每一读入的行在经过表达书操作后都会输出。-p -i 结合在一起才会修改原来的文件。

参考 https://zhuanlan.zhihu.com/p/161566950

因此可以执行如下指令：

```
perl -p -i -e "s/ServicesEnabled = sym;dc/ServicesEnabled = asym;dc/g" 4xxxvf_dev*.conf
```

## sed命令行

sed+awk也可以处理Linux中绝大部分的文本和表格。

```
sed -i "s/old/new/g"  files
```

sed的命令行参数为

```
Usage: sed [OPTION]... {script-only-if-no-other-script} [input-file]...

  -n, --quiet, --silent
                 suppress automatic printing of pattern space
  -e script, --expression=script
                 add the script to the commands to be executed
  -f script-file, --file=script-file
                 add the contents of script-file to the commands to be executed
  --follow-symlinks
                 follow symlinks when processing in place
  -i[SUFFIX], --in-place[=SUFFIX]
                 edit files in place (makes backup if SUFFIX supplied)
  -c, --copy
                 use copy instead of rename when shuffling files in -i mode
  -b, --binary
                 does nothing; for compatibility with WIN32/CYGWIN/MSDOS/EMX (
                 open files in binary mode (CR+LFs are not treated specially))
  -l N, --line-length=N
                 specify the desired line-wrap length for the `l' command
  --posix
                 disable all GNU extensions.
  -E, -r, --regexp-extended
                 use extended regular expressions in the script
                 (for portability use POSIX -E).
  -s, --separate
                 consider files as separate rather than as a single,
                 continuous long stream.
      --sandbox
                 operate in sandbox mode (disable e/r/w commands).
  -u, --unbuffered
                 load minimal amounts of data from the input files and flush
                 the output buffers more often
  -z, --null-data
                 separate lines by NUL characters
  --help
                 display this help and exit
  --version
                 output version information and exit

If no -e, --expression, -f, or --file option is given, then the first
non-option argument is taken as the sed script to interpret.  All
remaining arguments are names of input files; if no input files are
specified, then the standard input is read.
```

-i 命令的描述和perl中的一样。只不过sed不需要加-p

对批量文件的操作，如果是文件名有规则，可以像以上用4xxxvf_dev*.conf这样的正则表达式。不过，有时候，文件名未必有这些规则，你需要对不同名字的文件进行批量修改。并且这些文件中的内容也是不规则的，这怎么办？

可以结合grep指令，对当前目录或指定目录进行过滤，找出有特定词的文件。

```
sed -i "s/old/new/g"  `grep asym -rf ./`
```

对当前目录进行递归搜索，寻找任何有asym关键词的文件，并且作为文本类型的结果成为sed 的 第3个参数。

等于 `sed -i "s/old/new/g"  所有包含asym字符串的文件名`

参考来源： [[linux --批量修改文件内容](https://www.cnblogs.com/clairedandan/p/11458947.html)](https://www.cnblogs.com/clairedandan/p/11458947.html)

## Python批量替换处理

Python并未像Perl和sed那样具有方便的命令行操作，因此，需要编写Python脚本文件。
