## Evaluate assembly

1. GAEMR
2. QUAST

### GAEMR

GAEMR 是 BroadInstitute 用 Python 开发的对拼接结果提供分析报告的工具。

#### 1. GAEMR 安装

安装 GAEMR 分析依赖的工具和相关依赖库，其出具完整报告所需安装的第三方软件：

* samtools
* bwa
* bowtie2
* ncbi-blast+
* Picard
* RNAmmer
* RDP_Classifier
* biopython
* numpy
* matplotlib
* pysam

##### 1.1 安装 RNAmmer
先安装 RNAmmer 的依赖软件 hmmer。但是不要安装 hmmer3，否则不能使用。建议使用低版本的 hmmer2.3.2。
```
~/tmp$ wget ftp://selab.janelia.org/pub/software/hmmer/2.3.2/hmmer-2.3.2.bin.intel-linux.tar.gz
~/tmp$ tar -zxvf hmmer-2.3.2.bin.intel-linux.tar.gz -C ~/app
~/tmp$ cd ~/app/hmmer-2.3.2.bin.intel-linux
~/app$ ./configure
~/app$ make
~/app$ sudo make install
```

RNAmmer需要有edu的信箱去申请才能免费下载使用。
```
~/app$ uncompress rnammer-1.2.tar.Z
~/app$ mkdir -p rnammer-1.2
~/app$ tar -xvf rnammer-1.2.tar -C rnammer-1.2
~/app$ vim rnammer-1.2/rnammer
```

修改 rnammer文件以适应本机使用, 下面是 diff 修改前后差异，大家可以根据自己实际情况修改

```
~/app$ diff rnammer-1.2/rnammer rnammer-1.2/rnammer.orig
```
主要配置好自己机器上 hmmer 和 rnammer 的路径即可。结果如下：

```
35c35
< my $INSTALL_PATH = "/home/ubuntu/apps/rnammer-1.2";
---
> my $INSTALL_PATH = "/usr/cbs/bio/src/rnammer-1.2";
50c50
< 	$HMMSEARCH_BINARY = "/usr/local/bin/hmmsearch";
---
> 	$HMMSEARCH_BINARY = "/usr/cbs/bio/bin/linux64/hmmsearch";
```

由于 RNAmmer 需要安装 perl 的 Getopt::Long 和 XML::Simple 模块 模。具体安装方法有很多了，可以自行搜索 Perl 模块安装。因为 Bioperl 几乎是必装的工具，所以直接安装 bioperl 偷懒完事。
```
~tmp/$ sudo apt-get install bioperl
```

##### 1.2 安装 rdp_classifier
```
~tmp/$ wget http://jaist.dl.sourceforge.net/project/rdp-classifier/rdp-classifier/rdp_classifier_2.9.zip
~tmp/$ unzip rdp_classifier_2.9.zip -d ~/apps
```

##### 1.3 安装 Python 依赖包
```
~tmp/$ sudo apt-get install python-biopython python-matplotlib python-numpy python-pip
~tmp/$ sudo pip install pysam
```

##### 1.4 安装GAEMR
```
~/tmp$ wget http://www.broadinstitute.org/software/gaemr/wp-content/uploads/2012/12/GAEMR-1.0.1.tar.gz
~/tmp$ tar zxvf GAEMR-1.0.1.tar.gz -C ~/app/GAEMR-1.0.1
~/tmp$ cd ~/app/GAEMR-1.0.1
~/app$ sudo python setup.py install
```

##### 1.5 安装其他依赖程序

samtools, ncbi-blast+, bwa, bowtie2, Picard的安装参见Chapter5。

#### 2. GAEMR 的使用

将上一节用不同拼装工具拼装出基因组数用 GAEMR.py 进行分析

```
~/data$ GAEMR.py -s assembly.fasta
```

### QUAST

QUAST是St. Petersburg大学开发的基因组拼接评估软件。软件内建了 MUMmer, GeneMark.hmm, MetaGeneMark, GlimmerHMM, GAGE。因此只要下载安装 QUAST 即可使用。

#### 1. QUAST 的安装

```
~/tmp$ wget http://softlayer-sng.dl.sourceforge.net/project/quast/quast-2.3.tar.gz
~/tmp$ tar zxvf quast-2.3.tar.gz -C ~/app
```

#### 2. QUAST 的使用

```
~/app$ cd quast-2.3
~/app$ python quast.py
```



#### 软件评估

下面我们用沙门菌参考序列来尝试一下对几个主要的拼接软件做结果评价。本身没有多大意义，主要在于增进相应软件操作的熟练程度。
