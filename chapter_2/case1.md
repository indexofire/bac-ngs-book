# 案例分析一

利用 bwa 等比对软件将目标基因组reads比对到参考基因组，将SNPs提取出来进行分析。

## 铜绿假单胞菌临床株与环境株溯源

一例从基因组学角度，利用高通量测序技术对铜绿假单胞菌的临床分离株与环境分离株在基因组水平上进行溯源与耐药分析的实例。可以作为今后实验室开展工作的一个技术指导依据。工作流程将从以下几个方面对于这个Case开展数据分析：

- 测序质量：reads的长度，插入片段的长度
- 将测序数据比对到参考基因组ST17
 - 每个样品reads的分布情况
 - 每个样品的基因组测序覆盖度
 - 不同样品之间的差异，特别是当设置不同的过滤参数时的情况：
	     - Quality filter: QUAL > 100
         - Likelihood filter: QUAL / AO > 10
         - Variant frequency filter: AO / DP > 0.9 (90% allele frequency)
- 哪一个水龙头水样的分离株最有可能被认为是感染源。(方法: 用`vcfsamplediff`比较环境株与临床株SNPs数量)
- 2个病人分离株之间使用了大量的抗生素治疗，哪一个取得了耐药性？
 - 用 joint calling 来比较2个临床样品数据，并用`vcfsamplediff`分析
 - 对于获得的大量SNPs，是否需要过滤以及如何过滤
 - 了解SNPs的分布规律
 - 重点查看SNPs

### 分析流程

#### 1. 下载数据

数据可以从[这里](http://www.microbesng.uk/filedist/pseudomonas-practical/)下载获得，可以用`wget -m`递归下载路径下所有文件。文件包括：

- TAP1 - 病房1号水龙头水样中分离的菌株
- TAP2 - 病房2号水龙头水样中分离的菌株
- TAP3 - 病房3号水龙头水样中分离的菌株
- PATIENT1 - 服用抗生素后的病人分离株
- PATIENT2 - 一周内进一步服用抗生素的病人分离株

```bash
~$ mkdir -p pse_data && cd pse_data
~$ wget -m http://www.microbesng.uk/filedist/pseudomonas-practical/
~$ mv www.microbesng.uk/filedist/pseudomonas-practical/* .
```

#### 2. 建立 ST17 型参考菌株索引

**Mapping** 软件使用`bwa`来对参考基因组ST17建立索引，以便后续比对。

```bash
~$ bwa index ST17.fasta
```

#### 3. 将测序 reads 比对到 ST17 并排序索引

用`bwa mem`进行比对，输出的结果通过管道由samtools转换成BAM文件并进行排序和索引

```bash
~$ bwa mem -t 4 -R '@RG\tID:TAP1\tSM:TAP1' ST17.fasta \
> TAP1_R1_paired.fastq.gz TAP1_R2_paired.fastq.gz | \
> samtools view -bS - | samtools sort - TAP1.sorted
~$ samtools index TAP1.sorted.bam
```

> 参数介绍：
> * -t 4: 表示使用4线程，根据自己的计算机CPU资源设置。
> * -R '...': 设置reads的ID，\t为制表符会转义成tab键。

对其他2株环境株和2株临床株重复以上步骤。


#### 4. 绘制测序 reads 的覆盖度

```bash
~$ samtools stats TAP1.sorted.bam | grep "maximum length"
~$ samtools stats TAP1.sorted.bam | grep "insert size"
```


```bash
~$ samtools stats TAP1.sorted.bam | grep "^COV" > TAP1.coverage.txt
```

```r
library(ggplot2)
cov=read.table("TAP1.coverage.txt", sep="\t")
cov[1,]
ggplot(cov, aes(x=V3, y=V4)) + geom_histogram(stat="identity") + xlab("Coverage") + ylab("Count")
```

#### 5. 比较1号临床株与环境株的SNPs差异

```bash
~$ freebayes --ploidy 1 -C 5 -f ST17.fasta TAP1.sorted.bam PATIENT1.sorted.bam  > compare_tap1.vcf
```

分别比较3株环境株。

```bash
~$ vcffilter -f "QUAL / AO > 10" compare_tap1.vcf | vcffilter -f "NS = 2" | wc -l 
```

获得临床株与环境株之间的SNP差异数量

```bash
~$ vcfsamplediff SAME TAP1 PATIENT1 compare_tap1.vcf | vcffilter -f "QUAL / AO > 10" | vcffilter -f "NS = 2" | vcffilter -f "! ( SAME = germline ) " | grep -v "^#" | wc -l 
```

查看SNPs差异的基因

```bash
~$ vcfsamplediff SAME PATIENT1 TAP1 compare_tap1.vcf  | vcffilter -f "QUAL / AO > 10" | vcffilter -f "NS = 2" | vcffilter -f "! ( SAME = germline ) " > tap_differences.vcf 
~$ bedtools intersect -a ST17.gff -b tap_differences.vcf
```

#### 6. 2个临床株的耐药变迁

了解2个临床株的SNPs差异，并查看这些SNPs所影响的gene

```bash
~$ freebayes --ploidy 1 -C 5 -f ST17.fasta PATIENT1.sorted.bam PATIENT2.sorted.bam > compare_patient.vcf
~$ vcfsamplediff SAME PATIENT1 TAP1 compare_patient.vcf  | vcffilter -f "QUAL / AO > 10" | vcffilter -f "NS = 2" | vcffilter -f "! ( SAME = germline ) " > patient_differences.vcf
~$ bedtools intersect -a ST17.gff -b patient_differences.vcf
```

---

### Snippy

日常工作中我们需要更简便的软件或者pipelines来帮助完成整个分析工作。因此这里我们使用[snippy](https://github.com/tseemann/snippy/blob/master/bin/snippy)来帮助我们完成分析SNPs。


#### 1. 安装 Snippy

Snippy是比较适合新手的工具，它提供了All in One的工具套装。虽然Snippy需要以下软件支持：

- Perl >= 5.6
- BioPerl >= 1.6
- bwa mem >= 0.7.12
- samtools >= 1.1
- GNU parallel > 2013xxxx
- freebayes >= 0.9.20
- freebayes sripts ( freebayes-parallel,fasta\_generate\_regions.py)
- vcflib (vcffilter, vcfstreamsort, vcfuniq, vcffirstheader)
- vcftools (vcf-consensus)
- snpEff >= 4.1

但这些工具在Snippy安装包里都已经提供了，我们只需要根据自己的系统设置将以来工具添加到系统路径中去即可。


```bash
~$ wget https://github.com/tseemann/snippy/archive/v2.9.tar.gz
~$ tar xzf v2.9.tar.gz -C ~/app
~$ echo "export PATH=$PATH:$HOME/app/snippy-2.9/bin:$HOME/app/snippy-2.9/binaries/linux/" >> ~/.bashrc
~$ source ~/.bashrc

```

#### 2. 使用 Snippy

Snippy 不仅可以获得SNP(包括MultiSNP)，也可以获得insertion和indeletion。查看结果文件中的snps.tab。

```bash
~$ snippy --cpus 4 --outdir output --ref ST17.fasta \
> --R1 TAP1_R1_paired.fastq.gz --R2 TAP1_R2_paired.fastq.gz
>$ head output/snps.tab
```

Snippy还可以生成多个基因组的编码区SNPs的比对文件。

```bash
~$ snippy-core --prefix core output1 output2 output3 ...
~$ head core.aln
```

## 参考资料

1. [Jupyter Notebook for Pseudomonas Practical](http://nbviewer.jupyter.org/github/nickloman/nickloman.github.com/blob/master/tutorials/Pseudomonas-practical.ipynb)
2. [Seeking the source of Pseudomonas aeruginosa infections in a recently opened hospital: an observational study using whole-genome sequencing.](http://bmjopen.bmj.com/content/4/11/e006278.full)
3. [Vcflib Git Repository](https://github.com/ekg/vcflib)
4. [Freebayes Git Repository](https://github.com/ekg/freebayes)
5. [Snippy](https://github.com/tseemann/snippy)