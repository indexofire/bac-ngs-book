# Fastq 剪切与修饰

高通量测序单个 Run 的数据量比较大，以 Miseq 为例v3版本的测序芯片上 Flow Cell 上可以约25M的 Cluster，如果做 PE300 的测序就可以拿到15G左右的测序数据。而微生物基因组比较小，所以测微生物一般都是混样，添加 barcodes 来区分不同样本。对于公司提供的测序服务的，数据到手一般都已经去除接头分样好了。但如果是实验室自己的测序仪，拿到的机器数据需要先做一下处理。

illumina的测序数据 raw data 含有2个文件：
1. 序列文件（`\*_seq.txt`）：储存了测序碱基结果
2. 质量文件（`\*_prb.txt`）：测序过程中每个 cycle 的碱基质量值

一般我们比较熟悉的 Fastq 格式文件则是包含了这2个内容的文件。

我们可以采用 `Trimmomatic` 来去除接头序列。

## trimmomatic 工具处理测序数据

**1.1 用 trimmomatic 自带接头文件处理**

```
~/data$ java -jar ~/apps/Trimmomatic-0.32/trimmomatic-0.32.jar PE \
> -threads 20 -phred33 reads1.fastq reads2.fastq \
> reads1.clean.fastq reads1.unpaired.fastq \
> reads2.clean.fastq reads2.unpaired.fastq \
> ILLUMINACLIP:~/apps/Trimmomatic-0.32/adapters/TruSeq3-PE.fa:2:30:10 \
> LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:50
```

**1.2 用自定义序列来处理**

如果文库制备用的是非官方的第三方试剂，或者是自己设计合成的引物接头，那接头序列文件需要自己定义。

```
~/data$ mkdir -p adaptors
~/data$ echo '>Prefix/1
> GATCGGAAGAGCGGTTCAGCAGGAATGCCGAG
> >Prefix/2
ACACTCTTTCCCTACACGACGCTCTTCCGATCT' > adaptors/custom.fa
~/data$ echo 'alias java -jar /usr/local/bin/trimmomatic-0.32.jar=trim'
~/data$ trim PE ~/data/ecoli_ref-5m_s1.fq ~/data/ecoli_ref-5m_s2.fq \
> s1_pe s1_se s2_pe s2_se \
> ILLUMINACLIP:~/data/adaptors/custom.fa:2:30:10
```

## Fastx_toolkit

#### 常用测序数据去除接头软件

* Trimmomaic
* Fastx_toolkit
* trimAL
* Picard
* PRINSEQ
* BMTagger



#### Reference ###
1. [Trimmomatic: a flexible trimmer for Illumina sequence data](http://bioinformatics.oxfordjournals.org/content/early/2014/04/12/bioinformatics.btu170.full.pdf)
2. [Fastx_toolkit](http://hannonlab.cshl.edu/fastx_toolkit/)
3. [The Picard Pipeline](https://www.broadinstitute.org/files/shared/mpg/plathumgen/plathumgen_fennell.pdf)
4. [NGS Training](http://data.bits.vib.be/pub/trainingen/NGSIntro/NGSbasics.pdf)
