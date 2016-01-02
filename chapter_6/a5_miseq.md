## A5-miseq

A5-miseq 是一个用 perl 开发的针对细菌基因组 de novo assembly 的 pipeline 工具。从名字就可以看出开发者做的主要针对 illumina miseq 产出的测序数据进行拼接的工具。

#### 1. 下载并安装

```
~/tmp$ wget http://downloads.sourceforge.net/project/ngopt/a5_miseq_linux_20140604.tar.gz
~/tmp$ tar -zxf a5_miseq_linux_20140604.tar.gz -C ~/app
```

#### 2. 开始拼装

```
~/app$ perl bin/a5_pipeline.pl SRR955386_1.fastq SRR955386_2.fastq ~/data/a5_output
```

#### Reference

* http://arxiv.org/pdf/1401.5130v2.pdf