## Core Genome MLST

1. 从fasta序列文件从获得 MLST 型别数据
2. 从fastq测序数据中获得 MLST 型别数据

从 fasta 序列文件中获得 MLST 型别很简单，只需要 blast 工具就可以实现。比如将你要研究的物种 MLST 型别与序列数据下载

如果你研究的物种没有公开的 MLST 数据库而你想从已测序的基因组原始数据中建立自己的 MLST 数据库；或者想从基因组测序原始数据中挖掘 MLST 型别结果，就要通过类似 srst2 这样的工具来实现。


#### 1. 从基因组 reads 数据获取 MLST 型别

本节介绍如果用 srst2 做基因组 in silico MLST 实验和 gene screening 实验。

srst2 是一个 由墨尔本大学学院开发的 pipeline 工具集，用以实现直接从 ngs reads 数据获得 MLST 型别或毒力与耐药基因分布。原理是将 reads mapping 到自定义的数据库中，比较适合 miseq/hiseq 这样的短 reads。第一代 srst 工具主要从 finished 或 draft 中 scanning，srst2 的好处在于直接用 bowtie2 来实现未拼接数据的 ST 与基因数据抓取。

**1.1 安装 srst2**

```
~/tmp$ wget ...
~/app$
```

**1.2 从一个菌株fastq数据了解 ST 型别和毒力与耐药基因**

**1.3 批量筛选 SRA 数据库数据**

用 aspera 批量下载 SRA 数据，用 Python 脚本执行批量处理。

```python
import subprocess

with open('accession.txt', 'r') as f:
    accs = f.read().split("\n")

for acc in accs:
    subprocess.call('fastq-dump --split-files ../sra/%s.sra' % acc, shell=True)
    subprocess.call('srst2 --input_pe %s_1.fastq %s_2.fastq --mlst_delimiter _ \
        --mlst_definitions listeria_monocytogenes.fasta --mlst_db Listeria.fasta \
        --gene_db gene.db --log --output %s' % (acc, acc, acc), shell=True)
    subprocess.call('rm *.fastq', shell=True)
```

#### 2. 绘制 cgMLST 最小生成树

**2.1 安装 PHYLOViZ 以及依赖库**

运行 PHYLOViZ 需要安装 JRE 环境，方便期间直接用 Ubuntu 自带的 openjdk-7。如果你的电脑上安装的是其它 JRE 环境而无法使用的，可以尝试换成 openjdk 试一下。PHYLOViZ 是 Java 开发的带有图形化 GUI 界面的程序，操作比较简便，需要安装在支持 GUI 图形操作系统的个人电脑上使用。

```
~/tmp$ sudo apt-get install openjdk-7-jre
~/tmp$ wget http://www.phyloviz.net/dist/phyloviz-linux.sh
~/tmp$ chmod +x phyloviz-linux.sh
~/tmp$ ./phyloviz-linux.sh
```

**2.2 打开一个已有的 MLST 公共数据库**

**2.3 合并基因组 MLST 数据**

**2.4 创建基于基因组的基因位点差异数据库**

#### 3.
