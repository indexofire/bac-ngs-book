## SRA 数据处理

本节基本内容介绍：

1. SRA 数据库简介
2. SRA 数据下载
3. SRA 数据转换
4. 从基因组数据模拟短序列数据

#### 1. SRA 数据库简介

**SRA** 是 **S**equence **R**ead **A**rchive 的首字母缩写。

SRA 与 Trace 最大的区别是将实验数据与元数据分离。元数据现在可以划分为以下几类。

* Study：study 包含了项目的所有 metadata，并有一个 NCBI 和 EBI 共同承认的项目编号（universal project id），一个 study 可以包含多个实验（experiment）。
* Experiment：一个实验记载实验设计（Design），实验平台（Platform）和结果处理
（processing）三部分信息，并同时包含多个结果集（run）。
* Run：一个结果集包括一批测序数据。
* Submission：一个 study 的数据，可以分多次递交至 SRA 数据库。比如在一个项目启动前期，就可以把 study，experiment 的数据递交上去，随着项目的进展，逐批递交 run 数据。study 等同于项目，submission 等同于批
次的概念。

#### 2. SRA 数据下载

对于单个SRA数据，带图形界面的电脑可以通过浏览器下载。对于只有命令行的服务器端的操作，终端环境下无法使用GUI软件，所以要通过命令工具来下载SRA数据。

**1. 用 edirect 下载数据**

edirect 是 NCBI 最近发布的 entrez 数据操作命令行工具。过去往往要通过 biopython 之类的第三方脚本工具来实现查找，索引以及抓取 entrez 的数据。现在可以用 edirect 来很方便的实现。

```
~/tmp$ wget ftp://ftp.ncbi.nlm.nih.gov/entrez/entrezdirect/edirect.tar.gz
~/tmp$ tar zxvf edirect.tar.gz -C ~/app
~/tmp$ sudo ln -s ~/app/edirect/*
```

**2. 用 sra toolkit 下载数据**

选择美国 FDA 提交的 SRR955386 数据。该数据是在Illumina Miseq平台上PE250的数据。生物样本是一株分离自奶酪的单增李斯特菌 CFSAN006122。如果你已经安装好 NCBI 的 sra_toolkit 工具，可以直接用里面的 prefetch 下载。

```
~/data$ prefetch -v SRR955386
~/data$ mv ~/.ncbi/public/sra/SRR955386.sra .
```

**Notes:** *2.3.x版本的 sra_toolkit 会有些配置问题，建议使用最新版。不过2.4.x版本的程序调用ascp时，比如 prefetch 调用 ascp 时选择的 ssh 密钥是老版 `asperaweb_id_dsa.putty`, 而目前新版的ascp在linux里是使用 `asperaweb_id_dsa.openssh`。*

**3. 用aspera connect下载**

用 aspera connect 工具下载 NCBI 的数据速度非常快，而且不光可以下载 SRA 数据库里的数据，还可以下载 NCBI FTP 里的其他资源。

```
~/data$ ascp -i ~/.aspera/connect/etc/asperaweb_id_dsa.openssh \
> --user=anonftp --host=ftp.ncbi.nlm.nih.gov --mode=recv -l100m -T -k 1 \
> /sra/sra-instant/reads/ByRun/sra/ERR/ERR175/ERR175655/ERR175655.sra .
```

**4. 用 ascp 批量下载 sra 数据**

从 NCBI SRA 数据库检索到需要的序列，比如搜索关键词`Listeria monocytogenes miseq`，左侧filters定义为DNA和Genome。

用 python 脚本批量转换路径。

```python
# filename: format_path.y
with open('acc_list.txt', 'r') as f:
    data = f.read().split('\n')

list=[]

for i in data:
    path = '/sra/sra-instant/reads/ByRun/sra/%s/%s/%s/%s.sra\n' % (i[0:3], i[0:6], i, i)
    list.append(path)

with open('acc_list_full.txt', 'w') as f:
   d = f.writelines(list)
```

下载数据

```
~/data$ touch acc_list_full.txt
~/data$ python format_path.py
~/data$ mkdir -p sra
~/data$ ascp -i ~/.aspera/connect/etc/asperaweb_id_dsa.openssh --user=anonftp --host=ftp-private.ncbi.nlm.nih.gov --mode=recv --file-list acc_list_full.txt sra/
```

#### 3. SRA 数据转换

1. 安装 sra toolkit (for ubuntu x64 version)
```
~/tmp$ curl -O http://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/2.4.3/sratoolkit.2.4.3-ubuntu64.tar.gz
~/tmp$ tar zxvf sratoolkit.2.4.3-ubuntu64.tar.gz -C ~/app
~/tmp$ cd ~/app/sratoolkit.2.4.3-ubuntu64
~/tmp$ sudo ln -s `pwd`/bin/* /usr/local/sbin/
```

2. sra数据转成fastq格式

 SRR955386 这个数据的样本还用 Pacbio SMRT 平台进行了测序，Pacbio 单分子测序技术获得基因组完成图。用该完成图作为模板，考量一下不同拼接软件的拼接结果。

 将 CSFAN006122 的基因组完成图数据下载。
```
~/data$ prefetch -v SRR955386
~/data$ mv ~/.ncbi/public/sra/SRR955386.sra .
~/data$ fastq-dump --split-files SRR955386.sra
```

 完成后可以看到 `data` 目录下新增了2个文件 `SRR955386_1.fastq` 和 `SRR955386_2.fastq`

#### 4. 从基因组数据模拟短序列数据

前面介绍如何从 NCBI SRA 数据库下载 NGS 数据并转换成 fastq 格式的文件。可能有些人做的数据分析或验证需要从基因组逆向模拟成短序列格式数据，这种需求也有人开发了相的软件。

[wgsim](https://github.com/lh3/wgsim) 就是这样一个软件，它是由开发了BWA等软件大牛 Li heng 写的基因组转成短序列的软件。

**安装wgsim**

```
~/app$ git clone https://github.com/lh3/wgsim.git && cd wgsim
~/app$ gcc -g -O2 -Wall -o wgsim wgsim.c -lz -lm
~/app$ sudo ln -s `pwd`/wgsim /usr/local/sbin
~/app$ wsgim -h

Program: wgsim (short read simulator)
Version: 0.3.1-r13
Contact: Heng Li <lh3@sanger.ac.uk>

Usage:   wgsim [options] <in.ref.fa> <out.read1.fq> <out.read2.fq>

Options: -e FLOAT      base error rate [0.020]
         -d INT        outer distance between the two ends [500]
         -s INT        standard deviation [50]
         -N INT        number of read pairs [1000000]
         -1 INT        length of the first read [70]
         -2 INT        length of the second read [70]
         -r FLOAT      rate of mutations [0.0010]
         -R FLOAT      fraction of indels [0.15]
         -X FLOAT      probability an indel is extended [0.30]
         -S INT        seed for random generator [-1]
         -A FLOAT      disgard if the fraction of ambiguous bases higher than FLOAT [0.05]
         -h            haplotype mode
```

**模拟基因组短序列数据**

使用所有参数为默认值，将大肠杆菌基因组数据转换为PE250 fastq 格式数据。

```
~/data$ wget https://raw.github.com/ecerami/samtools_primer/master/tutorial/genomes/NC_008253.fna
~/data$ wgsim -S11 -1250 -2250 NC_008253.fna reads_1.fastq reads_2.fastq
~/data$ head -8 reads_1.fastq

@gi|110640213|ref|NC_008253.1|_3151106_3151623_4:0:0_5:1:0_0/1
AACATAAGTGGTATTAATTCCCCAAGATTCAAGATTACGAATAGTATTATCCGCAAAAATATCATCACCTACTTTCGTCAGCATCAGGACTTTTGAATTCAATTTAGCCGCCGCCACCGCTTGATTAGCACCTTTGCCACCACATCCGATTTTGAAGGCAGGTGCTTCCAGAGTTTCTCCTTCTTTAGGCATCTGATAAGTGTAAGTAATGAGATCCACCATATTGGAACCAATAAGTGCAATGTCCATT
+
2222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222
@gi|110640213|ref|NC_008253.1|_2429297_2429873_8:0:0_7:0:0_1/1
ACGGTATCACCATGATTGCCCGTTCCGTCAACAGCATGGGGCTGGTCATTATAGGCGGTGGATCGCTTGAAGAAGCGTTAACTGAACTGGAAACCGGACGCGGCGACGCGGTGGTGGTGCTGGAAAACGAACTGCATCGTCACGCTTGCGCTACCCGCGTGAATGCTGCGCTGGCTAAAGCGCCGCTGGTGATTGTGGTTGACCATCAACGCACAGCGATTATGGAAAACGCTCATCTGGTACTATCCGC
+
2222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222222
```

#### Reference

1. [SRA数据库帮助](http://download.bioon.com.cn/view/upload/201401/20235821_1284.pdf)
2. [SRA Handbook](http://www.ncbi.nlm.nih.gov/books/NBK47528/)
3. 熊筱晶, NCBI高通量测序数据库SRA介绍, 生命的化学[J]: 2010, 06, 959-962.
