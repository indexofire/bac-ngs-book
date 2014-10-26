## 细菌基因组组装

#### 数据转换与QC

1. 安装 sra toolkit (for ubuntu x64 version)
```
~/tmp$ curl -O http://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/2.4.1/sratoolkit.2.4.1-ubuntu64.tar.gz
~/tmp$ tar zxvf sratoolkit.2.4.1-ubuntu64.tar.gz -C ~/app
~/tmp$ cd ~/app/sratoolkit.2.4.1-ubuntu64
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

#### 拼接

物种的核酸特性以及测序技术的发展，不断有针对新技术优化的拼接软件出现。这里比较几个微生物拼接评测中比较优秀的工具，看看针对 Listeria monocytogenes 的拼接结果。

**1. SPAdes**

1.1 下载并安装 SPAdes
```
~/tmp$ curl -O http://spades.bioinf.spbau.ru/release3.1.1/SPAdes-3.1.1-Linux.tar.gz
~/tmp$ tar zxvf SPAdes-3.1.1-Linux.tar.gz -C ~/app
~/tmp$ cd ~/app
~/app$ sudo ln -s `pwd`/SPAdes-3.1.1-Linux/bin/* /usr/local/sbin
```

1.2 拼装基因

```
~/app$ spades.py -t 2 -1 SRR95386_1.fastq -2 SRR955386_2.fastq -o spades_output
```

SPAdes会尝试不同的Kmer，因此拼装时间也会根据Kmer选择数量成倍增加。

**2. MaSuRCA**

2.1 下载并安装 MaSuRCA

```
~/tmp$ curl -O ftp://ftp.genome.umd.edu/pub/MaSuRCA/MaSuRCA-2.3.0.tar.gz
~/tmp$ tar zxvf MaSuRCA-2.3.0.tar.gz -C ~/app
~/tmp$ cd ~/app
~/app$ ./MaSuRCA-2.3.0/install.sh
~/app$ sudo ln -s `pwd`/MaSuRCA-2.3.0/bin/masurca /usr/local/sbin
```

2.2 建立拼装配置文件 `~/data/SRR955386_masurca.config`

```
~/data$ touch SRR955386_masurca.config
~/data$ nano SRR955386_masurca.config
```

文件内容修改如下
```
DATA
PE= p1 500 50 SRR955386_1.fastq SRR955386_2.fastq
END

PARAMETERS
GRAPH_KMER_SIZE=auto
USE_LINKING_MATES=1
LIMIT_JUMP_COVERAGE = 60
KMER_COUNT_THRESHOLD = 1
NUM_THREADS= 2
JF_SIZE=100000000
DO_HOMOPOLYMER_TRIM=0
END
```

设置`GRAPH_KMER_SIZE=auto`，软件会调用Kmer=31来进行拼装。对于MiSeq PE250以上的插片，可以考虑手动设置使用更大的KmerS

2.3 开始拼装
```
~/app$ masurca SRR955386_masurca.config
~/app$ ./assemble.sh
```

**3. A5-miseq**
A5-miseq是一个用 perl 开发的针对细菌基因组 de novo assembly 的 pipeline 工具。它本身不参与组装，而是通过组合一套工具来完成工作。

3.1 下载并安装

```
~/tmp$ wget http://downloads.sourceforge.net/project/ngopt/a5_miseq_linux_20140604.tar.gz
~/tmp tar zxvf a5_miseq_linux_20140604.tar.gz -C ~/app
```

3.2 开始拼装

```
~/app$ perl bin/a5_pipeline.pl SRR955386_1.fastq SRR955386_2.fastq ~/data/a5_output
```




#### Reference
* http://arxiv.org/pdf/1401.5130v2.pdf
