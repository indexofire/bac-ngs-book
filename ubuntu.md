## Ubuntu 热身

Linux 有多个不同的发行版，最主流的几个如 Debian/Ubuntu, CentOS, Archlinux，SuSE Linux等等。每个发行版的系统管理设计会有些差别，特别是程序包管理工具会有些不同，本文全部是以 Ubuntu 为例开展的。其他 Linux 发行版或者 Mac 系统请参考该系统相应文档。

[Ubuntu][] 作为最主流的 [Linux][] 发行版之一，有一套自身完善的软件管理命令，因经常会用到，所以来熟悉一下。安装一些必备工具

```bash
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get install curl python-dev build-essential gcc g++ \
    biopython bioperl libbz2-dev
```

本书中所涉及到的默认配置，需要先新建这些文件夹，以免运行命令是出错。数据统一放在 `~/data` 目录；应用程序统一安装在 `~/app` 目录；下载等操作的临时目录在 `~/tmp` 目录建立操作所需的文件夹

```bash
# 进入当前用户主目录
$ cd

# 在当前用户主目录下新建目录 data, app 和 tmp
$ mkdir -p data app tmp
```

对于的病原微生物研究人员，掌握一点最基本的 [Linux][] 命令会有很大帮助。注意一下内容 `$` 后面的是输入命令，`#` 后的是解释，在后面的使用中可能会接触到。用 `man 命令名` 可以查看 manual，直接输入 `命令名 -h` 可以查看参数帮助

有兴趣的微生物学研究者可以阅读相关资料，增强对 Unix 的相关知识：
* [Unix and Perl Primer for Biologists](http://korflab.ucdavis.edu/Unix_and_Perl/current.html)
* [The Linux Command Line](http://billie66.github.io/TLCL/book/zh/)

[Ubuntu]: http://www.ubuntu.com/ "Ubuntu"
[Linux]: http://www.linux.com/ "Linux"
