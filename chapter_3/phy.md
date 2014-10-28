## SNP 分析

#### 1. 使用 Wombac 获得细菌基因组 SNP

[wombac][] 是由 [Victorian Bioinformatics Consortium](http://www.vicbioinformatics.com/) (以下简称 VBC )开发的用于获取细菌基因组 SNP 的工具。[wombac][] 使用 perl 开发，本质上是一个 perl 工作流脚本，使用的工具是 bwa, samtools 和 freebayes.

[wombac][] 会寻找碱基替换，但不会搜索 indels，精确度上来说会有一些 SNP 的丢失，但其精度已足够用来进行基因组的快速分析。

##### 1.1 安装并运行

```
~/tmp$ wget http://www.vicbioinformatics.com/wombac-1.3.tar.gz
~/tmp$ tar zxvf wombac-1.3.tar.gz -C ~/app
~/tmp$ cd ~/app/wombac-1.3/bin
~/app$ perl wombac --outdir ~/data/wombac --ref ~/data/egd.fasta --run /data/draft
```

##### 1.2 绘制SNP进化树树




[wombac]: http://www.vicbioinformatics.com/software.wombac.shtml "wombac"



