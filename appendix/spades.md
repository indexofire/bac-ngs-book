## SPAdes

#### 1. 下载并安装 SPAdes

```
~/tmp$ curl -O http://spades.bioinf.spbau.ru/release3.1.1/SPAdes-3.1.1-Linux.tar.gz
~/tmp$ tar zxvf SPAdes-3.1.1-Linux.tar.gz -C ~/app
~/tmp$ cd ~/app
~/app$ sudo ln -s `pwd`/SPAdes-3.1.1-Linux/bin/* /usr/local/sbin
```

#### 2. 拼装基因组

```
~/app$ spades.py -t 2 -1 SRR95386_1.fastq -2 SRR955386_2.fastq -o spades_output
```

SPAdes会尝试不同的Kmer，因此拼装时间也会根据Kmer选择数量成倍增加
