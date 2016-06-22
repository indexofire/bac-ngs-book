# 处理下机数据进行处理

Miseq 下机数据一般生成 .fastq 文件。进行质控后一般需要去除接头，污染，低质量序列等。

Miseq 的 Flow Cell 上约有 25M 的 Cluster，如果做 PE300 的测序就可以拿到15G左右的测序数据。而微生物基因组比较小，所以做微生物基因组 de novo sequencing 如果为了追求时间就降低读长，为了成本就采用混样测序，通过添加 barcodes 序列来区分样本。对于公司提供测序服务获得的数据或者是公共数据库下载的数据一般都是 Clean Data，已经去除低质量碱基，剪切接头，并分样完成。实验室自己的 Miseq 测序仪上机时填写好 Sample Sheet，Miseq Reporter 做2级分析时可以自动完成 Trimming adaptors 和 Demultiplexing 的过程。但有些情况下还是需要手动 Trimming adaptors 的，比如做文库目标序列特别短或者文库制备时打断出问题，插入序列过短导致测到 adaptors 序列等。

Miseq Report 完成 Demultiplexing 过程后就会产出各个样品的 FastQ 测序数据，如果这些数据含有接头序列，需要先用软件去除。

## 常用测序数据去除接头软件

有许多工具可以用来做接头序列去除的工作，本节介绍几个最常用的工具：

* [Trimmomaic](http://www.usadellab.org/cms/?page=trimmomatic)
* [Fastx_toolkit](http://hannonlab.cshl.edu/fastx_toolkit)
* [PRINSEQ](http://prinseq.sourceforge.net)
* BMTagger
* cutadaptor
* seqtk
* sickle

## 去除 reads 接头序列

### 1. 用 trimmomatic 工具去除接头

如果你的建库试剂盒采用的是illumina原厂试剂，那么 adaptors 的序列在 Trimmomatic 的接头文件数据库中存在，可以直接去除。

**安装 Trimmomatic**

软件安装很简单，直接下载 Trimmomatic 的软件包解压缩即可使用。

```bash
# 下载软件
~/tmp$ wget http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/Trimmomatic-0.36.zip
~/tmp$ unzip Trimmomatic-0.36.zip -d ~/app
# 简化运行
~/tmp echo 'alias trimm="java -jar ~/apps/Trimmomatic-0.36/trimmomatic-0.36.jar"' >> ~/.bashrc
~/tmp source ~/.bashrc
```

#### 1.1 用 Trimmomatic 自带接头文件处理

```bash
~/data$ trimm PE -threads 20 -phred33 \
> reads1.fastq.gz reads2.fastq.gz \
> reads1.clean.fastq.gz reads1.unpaired.fastq.gz \
> reads2.clean.fastq.gz reads2.unpaired.fastq.gz \
> ILLUMINACLIP:TruSeq3-PE.fa:2:30:10 \
> LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:50
```

#### 1.2 用自定义序列来处理

如果文库制备用的是非官方的第三方试剂，或者是自己设计合成的引物接头，那接头序列不包含在软件数据库中，则需要自己定义。

```bash
# 自定义 Trimmomatic 的adapotrs序列
~/data$ mkdir -p adaptors
~/data$ echo -e '>Prefix/1\nGATCGGAAGAGCGGTTCAGCAGGAATGCCGAG\n>Prefix/2\nACACTCTTTCCCTACACGACGCTCTTCCGATCT' > adaptors/custom.fa

# 安装了并行工具parallel的也可以这样创建自定义序列
~/data$ parallel -k echo ::: \>Prefix/1 GATCGGAAGAGCGGTTCAGCAGGAATGCCGAG \>Prefix/2 ACACTCTTTCCCTACACGACGCTCTTCCGATCT > adaptors/custom.fa

# 去除用自定义接头序列
~/data$ java -jar /usr/local/bin/trimmomatic-0.32.jar PE \
> -threads 20 -phred33 reads1.fastq reads2.fastq \
> reads1.clean.fastq reads1.unpaired.fastq \
> reads2.clean.fastq reads2.unpaired.fastq \
> ILLUMINACLIP:~/data/adaptors/custom.fa:2:30:10 \
> LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:50
```

### 2.fastx_clipper

```bash
# 假设接头序列为ATCGGGCGGAATATTACCCA
~$ fastx_clipper -a ATCGGGCGGAATATTACCCA
```

### 3.cutadapt

```bash
# -a 为去除3'端接头，-g 为去除5'接头
~$ cutadapt -a ATCGGGCGGAATATTACCCA -g CTAACCAGGGAGA input.fastq output.fastq
```

## 去除reads中低质量序列

### 1. Seqtk 清除碱基

```bash
# seqtk
~$ seqtk trimfq -b 5 s1_1.fastq > s1_1_trimmed.fastq
~$ seqtk trimfq -b 5 s1_2.fastq > s1_2_trimmed.fastq
```


### 2. sickle

```bash
~$ sickle pe -f s1_1.fastq -r s1_2.fastq -n 5  -t sanger \
> -o s1_1_trimmed.fastq -p s1_2_trimmed.fastq \
> -s s1_trimmed.fastq
```

需要清楚自己的测序文件是那个版本的质量评分体系，目前用的illumina测序仪均为1.8+版本，所以用 `-t sanger` 即可。

```bash
# SE 序列文件
~$ sickle se -f example.fastq -t sanger -o example_trimmed.fastq
# PE 序列文件
~$ sickle pe -f example_1.fastq -r example_2.fastq -o example_1_trimmed.fastq -p example_2_trimmed.fastq -
```


## Reference

1. [Trimmomatic: a flexible trimmer for Illumina sequence data](http://bioinformatics.oxfordjournals.org/content/early/2014/04/12/bioinformatics.btu170.full.pdf)
2. [Fastx_toolkit](http://hannonlab.cshl.edu/fastx_toolkit/)
3. [The Picard Pipeline](https://www.broadinstitute.org/files/shared/mpg/plathumgen/plathumgen_fennell.pdf)
4. [NGS Training](http://data.bits.vib.be/pub/trainingen/NGSIntro/NGSbasics.pdf)
5. [Flow Cell](http://www.uvm.edu/medicine/vtcancercenter/documents/Illumina-flowcell-whitesheet.pdf)
