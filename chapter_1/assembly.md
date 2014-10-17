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

 SRR955386 这个数据的样本还用 Pacbio SMRT 平台进行了测序，Pacbio 单分子测序技术获得基因组完成图。用该完成图作为模板，考量一下不同拼接软的拼接结果。将 CSFAN006122 的基因组完成图数据下载。
```
~/data$ prefetch -v SRR955386
~/data$ mv ~/.ncbi/public/sra/SRR955386.sra .
~/data$ fastq-dump --split-files SRR955386.sra
```

 完成后可以看到 `data` 目录下新增了2个文件 `SRR955386_1.fastq` 和 `SRR955386_2.fastq`

3. QC
```
~/data$ fastq SRR955386_1.fastq
```

 使用FastQC软件查看，Adapter Content选项里显示序列无 Adapter。

 *note: 服务器端的QC较好的实现方式？*

4. trimming
```
pass
```
 *note: 需要自动化区分对于提交的数据是否做过trimming*

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

1.2 开始拼装

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

设置`GRAPH_KMER_SIZE=auto`，软件直接调用Kmer=31来进行拼装。对于MiSeq v2以上的试剂，可以考虑使用更大的Kmer

2.3 开始拼装
```
~app$ masurca SRR955386_masurca.config
~app$ ./assemble.sh
```

**3. a5-miseq**

3.1 下载并安装

3.2 开始拼装






