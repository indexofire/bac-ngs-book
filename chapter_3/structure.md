## Population Genomics

#### srst2

做基因组 in silico MLST 实验和 gene screening 实验,用 srst2 是个不错的选择。它可以实现直接从 reads 数据中获得，经过尝试发现准确率还是不错的。srst2 是一个由墨尔本大学学院开发的工具，第一代 srst 工具主要从 finished 或 draft 中 scanning，srst2 的好处在于直接用 bowtie2 来实现未拼接数据的 ST 与基因数据抓取。

##### 1. 安装 srst2

```
~/tmp$ wget ...
~/app$
```

##### 2. 从一个菌株fastq数据了解 ST 型别和毒力与耐药基因



##### 3. 用 Python 脚本批量处理多个数据

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




#### PHYLOViZ

##### 1. 安装 PHYLOViZ 以及依赖库

运行 PHYLOViZ 需要安装 JRE 环境，方便期间直接用 Ubuntu 自带的 openjdk-7。如果你的电脑上安装的是其它 JRE 环境而无法使用的，可以尝试换成 openjdk 试一下。PHYLOViZ 是 Java 开发的带有图形化 GUI 界面的程序，操作比较简便，需要安装在支持 GUI 图形操作系统的个人电脑上使用。这里以安装 XWindows 的 Ubuntu 为例。

```
~/tmp$ sudo apt-get install openjdk-7-jre
~/tmp$ wget http://www.phyloviz.net/dist/phyloviz-linux.sh
~/tmp$ chmod +x phyloviz-linux.sh
~/tmp$ ./phyloviz-linux.sh
```

##### 2. 打开一个已有的 MLST 公共数据库

##### 3. 合并基因组 MLST 数据

##### 4. 创建基于基因组的基因位点差异数据库
