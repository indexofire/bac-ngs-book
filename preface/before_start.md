## Linux 常用的 shell commands

笔记中使用许多 Linux 命令行，这里做一个快速笔记以备查询。以 bash 为例。其他的 shell 比如 csh, tcsh 等大部分命令和参数都是一致的。

- `ls`: 列出文件夹的文件

```bash
# 显示 '~/data' 文件夹里的文件
$ ls ~/data

# 显示包括隐含文件的 '~/data' 文件夹里的所有文件
$ ls -la ~/data

# 文件按照时间排序
$ ls -t ~/data

# 查看所有 fastq 文件
$ ls ~/data/*.fastq
```

- `cd`: 改变文件夹

```bash
# 从当前文件夹 '~/data' 更换到 '~/app' 文件夹
~/data$ cd ~/app

# 从任何位置切换回用户主目录 '/home/User'
$ cd

# 回退上一个访问目录
$ cd -

# 返回上一级目录
$ cd ..

# 返回多级目录冰前往另一个文件夹，这里 '\' 是转义符，将空格键正确转义
$ cd ../../../new\ directory
```

- `pwd`: 显示当前目录

```bash
# 当不知道当前处于什么路径时，可以用这个命令显示
$ pwd
```

- `mkdir`: 建立新文件夹

```bash
# 当前路径下新建一个名叫 'new' 的文件夹
$ mkdir new
```

- `rmdir`: 删除文件夹

```bash
# 删除当前路径的文件夹 'new'
$ rmdir new
```

- `rm`: 删除文件

```bash
# 删除当前文件夹的所有后缀是 '.sra' 的文件
$ rm *.sra

# 删除文件夹 'new' 以及 'new' 下的所有子文件与子目录
$ rm -R new

# 不弹出删除确认提示，删除所有 '.tmp' 文件
$ rm -f *.tmp
```

- `cp`: 复制文件

```bash
# 复制 'test.txt' 文件到文件夹 '~/abc' 中
$ cp test.txt ~/abc
```

#### `mv`: 移动文件或文件夹

```bash
# 移动 'test.txt' 文件到文件夹 '~/abc' 中并改名叫 'test1.txt'
$ mv test.txt ~/abc/test1.txt
```

#### `which`: 查找可执行文件的系统路径

```bash
# 打印出系统带的 python 程序的路径
$ which python
```

#### `wc`: 统计一个文件的行，字符和字节数

```bash
# 输出文件 'text.txt' 的行数，字符数和字节数。
$ wc text.txt
```

#### `cat`: 输出文件内容

```bash
# 显示文件 'text.txt' 内容
$ cat text.txt
```

#### `grep`: 截取输入字符的选定 pattern 并输出所在的行

```bash
# 显示文件 'text.txt' 中含有字符 'abc' 的行
$ cat text.txt | grep 'abc'
```

#### `head`: 输出文件头部内容

```bash
# 输出文件前5行内容
$ head -5 text.txt
```

#### `tail`: 输出文件尾部内容

```bash
# 输出文件的最后5行内容
$ tail -5 text.txt
```

#### `ps`: 查看系统进程

ps会在终端打印系统进程，各列的含义是:

* USER: 运行该进程的用户
* PID: 运行着的命令(CMD)的进程编号
* %CPU: CPU占用
* %MEM: 内存占用
* VSC:
* RSS:
* TTY: 命令所运行的位置（终端）
* STAT:
* TIME:
* TIME: 运行着的该命令所占用的CPU处理时间
* COMMAND: 该进程所运行的命令

```bash
# 显示详细的进程信息
$ ps -waux

# 过滤用户root的进程
$ ps -u root

# 根据不同参数使用来排序进程，并只现实排名前10的进程
$ ps -aux --sort -pcpu | head -n 11
$ ps -aux --sort -pmem | head -n 11
$ ps -aux --sort -pcpu,+pmem | head -n 11

# 过滤进程名
$ ps -f -C chrome

# 根据PID过滤
$ ps -L 1000

# 树形现实进程
$ ps -axjf
```

#### Reference

1. http://linoxide.com/how-tos/linux-ps-command-examples/
