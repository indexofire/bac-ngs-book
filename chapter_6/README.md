## 常用软件的安装与设置

* [Assembly](assembly.md)
* [Mapping](mapping.md)
* [Visualization](visualization.md)
* [Alignment](alignment.md)
* [Phylogeny](phylogeny.md)
* [Annotation](annotation.md)

**Ubuntu软件安装**

Linux 主流发行版一般都会开发自己的软件管理工具，不同发行版的软件管理工具不同，容易给初学者带来困惑。在基于 Ubuntu 操作系统的服务器或是自己PC机上安装的桌面版本 Ubuntu 上安装软件是有多种方式的，这里就根据 Ubuntu 来举几个例子，当大家遇到要安装书中没有提到的相关软件后，也能得心应手。

**用 Ubuntu 软件管理工具来安装: **
日常管理系统中安装软件最常见的方式，Ubuntu的软件包管理者们已经编译好适用于各版本Ubuntu的软件包，你只需要通过几行命令就可以让系统完成安装和配置工作。
```
# 更新Ubuntu软件数据库
~$ sudo apt-get update
~$ sudo apt-get install pkg_name
```

如果你不知道在 Ubuntu 软件数据库里你所要安装的软件名称是什么，你可以用正则表达式来匹配。不过这样做有时候会出现匹配结果过多，且结果之间有相互依赖性不符的问题。所以当你不清楚软件包名称时可以先搜索。
```
# 确保软件仓库为最新
~$ sudo apt-get update
# 搜索软件包中汉fastq信息的软件
~$ sudo apt-cache search fastq

# 打印出结果，你可以看到一些常见的fastq格式文件处理软件，ubuntu软件仓库中都已经包含，可以直接安装。

fastqc - quality control for high throughput sequence data
fastx-toolkit - FASTQ/A short nucleotide reads pre-processing tools
librubberband-dev - audio time-stretching and pitch-shifting library (development files)
mapsembler2 - bioinformatics targeted assembly software
mira-assembler - Whole Genome Shotgun and EST Sequence Assembler
picard-tools - Command line tools to manipulate SAM and BAM files
seqtk - Fast and lightweight tool for processing sequences in the FASTA or FASTQ format
sra-toolkit - utilities for the NCBI Sequence Read Archive
trimmomatic - flexible read trimming tool for Illumina NGS data
```

当然生物软件发展也很快，版本会经常更性，所以ubuntu不同发行版，比如LTS发行版为了稳定，软件版本的更新不会那么快。对于想要知道软件仓库中fastqc是哪个版本（因为老版本的fastqc缺少一些分析模块），可以用下面命令：
```
~$ sudo apt-cache show fastqc | grep 'Version'

# 打印结果：

Version: 0.10.1+dfsg-2
```

这样就可以决定是否要安装的自己需要的那个版本fastqc。对于要安装新版本软件，一种方法是添加别人设置的含有顶起更新的最新软件源，另一种就是直接从官方网站上下载预编译包或者源代码。下面各小节中的安装方式，基本都是以下载源代码自行编译或者下载安装预编译包的方式来完成的。