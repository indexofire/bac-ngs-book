## SNP Phylogenomics

病原的溯源，进化以及种群结构是公共卫生研究领域一个非常重要内容。过去基于一代测序的方法，对一个基因（如毒力基因序列）或几个基因（如MLST序列）构建进化树，数据量的不足带来的精度问题，容易造成结果与实际产生偏差。而随着WGS的发展，产生了一门新的学科 **Phylogenomics基** 因组种系学。通过全基因组的数据来构建绘制系统发生树。

#### 目前常见的方法有：

##### 1. 基于距离：

计算全基因矩阵距离，通过距离来构建系统发生树。

##### 2. 基于核心基因(core genes)：

通过将同种细菌基因组同源基因（core genes）的异同构建进化树。

##### 3. 基于参考基因组：

通过将测序获得的短片段mapping到参考基因组上，找到核心基因组上的variants位点，连锁这些位点进行多重比对，再构建系统发生树。

#### Wombac

[wombac][] 是由 [Victorian Bioinformatics Consortium](http://www.vicbioinformatics.com/) 开发的用于获取细菌基因组 SNP 的工具。[wombac][] 使用 perl 开发，本质上是一个 perl 工作流脚本，使用的工具是 bwa, samtools 和 freebayes，这3个工具 Linux 版 [wombac][] 已经自动提供了，要正常运行的话系统中还必须安装vcfutils.pl, bgzip, tabix。

[wombac][] 会寻找碱基替换，但不会搜索 indels，精确度上来说可能会有一些 SNP 的丢失，但其精度已足够用来进行基因组的快速分析。

**1.1 安装并运行**

```
~/tmp$ wget http://www.vicbioinformatics.com/wombac-1.3.tar.gz
~/tmp$ tar zxvf wombac-1.3.tar.gz -C ~/app
~/tmp$ cd ~/app/wombac-1.3/bin
~/app$ perl wombac --outdir ~/data/wombac --ref ~/data/egd.fasta --run /data/draft
```

**1.2 绘制SNP进化树树**

[wombac][] 对每个基因组产生BAM，VCF文件，同时产生一个整体多重比对的ALN文件用于绘制进化树。

```
~/app$ SplitsTree -i Tree/snps.aln
```

#### kSNP


## Reference
1. https://github.com/apetkau/microbial-informatics-2014/
2. ...

[wombac]: http://www.vicbioinformatics.com/software.wombac.shtml "wombac"
[CSI Phylogeny]: http://cge.cbs.dtu.dk/services/CSIPhylogeny/ "CSI Phylogeny"
[GenoBox]: https://github.com/srcbs/GenoBox "GenoBox"
[REALPHY]: http://realphy.unibas.ch/fcgi/realphy "REALPHY"
[kSNP]: http://sourceforge.net/projects/ksnp/ "kSNP v2"



