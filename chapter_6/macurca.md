## MaSuRCA

#### 1. 下载并安装 MaSuRCA

```
~/tmp$ curl -O ftp://ftp.genome.umd.edu/pub/MaSuRCA/MaSuRCA-2.3.0.tar.gz
~/tmp$ tar zxvf MaSuRCA-2.3.0.tar.gz -C ~/app
~/tmp$ cd ~/app
~/app$ ./MaSuRCA-2.3.0/install.sh
~/app$ sudo ln -s `pwd`/MaSuRCA-2.3.0/bin/masurca /usr/local/sbin
```

#### 2. 建立拼装配置文件

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
设置`GRAPH_KMER_SIZE=auto`，软件会调用Kmer=31来进行拼装。对于MiSeq PE250以上的长插入片段，建议手动设置使用更大的Kmer。

#### 3. 开始拼装

```
~/app$ masurca SRR955386_masurca.config
~/app$ ./assemble.sh
```
