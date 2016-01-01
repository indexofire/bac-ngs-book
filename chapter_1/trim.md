## Fastq 剪切与修饰

高通量测序单个 RUN 的数据量比较大，而微生物基因组比较小，所以测微生物一般都是混样，添加 barcodes 来区分不同样本。对于公司提供的测序服务的，数据到手一般都已经分样好了。对于smallRNA测序或者实验过程中出现一些问题比如文库被污染了接头等情况时，往往需要对数据进行去除污染序列的处理。

#### 常用测序数据去除接头软件

* Trimmomaic
* trimAL
* Picard
* PRINSEQ
* BMTagger

#### 1. Trimmomatic

**Trimmomatic 安装**

```
~/tmp$ wget http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/Trimmomatic-0.32.zip
~/tmp$ unzip Trimmomatic-0.32.zip -d ~/app
```

**用 trimmomatic 工具处理测序数据**

1.1 用 illumina secret clip 来处理

```
~/data$ mkdir -p adaptors
~/data$ wget https://s3.amazonaws.com/public.ged.msu.edu/illuminaClipping.fa adaptors/
~/data$ echo 'alias java -jar /usr/local/bin/trimmomatic-0.32.jar=trim'
~/data$ trim PE ~/data/ecoli_ref-5m_s1.fq ~/data/ecoli_ref-5m_s2.fq \
> s1_pe s1_se s2_pe s2_se \
> ILLUMINACLIP:~/data/adaptors/illuminaClipping.fa:2:30:10
```

1.2 用 trimmomatic 自带接头文件处理

```
~/data$ java -jar ~/apps/Trimmomatic-0.32/trimmomatic-0.32.jar PE \
>     -threads 20 -phred33 reads1.fastq reads2.fastq \
>     reads1.clean.fastq reads1.unpaired.fastq reads2.clean.fastq reads2.unpaired.fastq \
>     ILLUMINACLIP:~/apps/Trimmomatic-0.32/adapters/TruSeq3-PE.fa:2:30:10 \
>     LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:50
```

如果文库制备用的是非官方的第三方试剂，那污染序列需要自己定义。

#### 2. Fastx_toolkit

`fastx_toolkit` 提供了一套 fasta/q 数据转换与处理的方式，其中最常用的几个工具如 `fastx_clipper`, `fastx_trimmer` 等，灵活运用可以为我们数据前处理提供方便。

** fastx_toolkit 安装**

```
~/tmp$ wget http://cancan.cshl.edu/labmembers/gordon/files/libgtextutils-0.7.tar.bz2
~/tmp$ wget http://cancan.cshl.edu/labmembers/gordon/files/fastx_toolkit-0.0.14.tar.bz2
~/tmp$ tar zxvf libgtextutils-0.7.tar.bz2
~/tmp$ cd libgtextutils-0.7
~/tmp$ ./configure
~/tmp$ make
~/tmp$ sudo make install
~/tmp$ cd ..
~/tmp$ tar xjf fastx_toolkit-0.0.14.tar.bz2 -C ~/app
~/tmp$ cd ~/app/fastx_toolkit-0.0.14
~/app$ ./configure
~/app$ make
~/app$ sudo make install
~/app$ sudo ldconfig
```


#### Reference ###
1. [Trimmomatic: a flexible trimmer for Illumina sequence data](http://bioinformatics.oxfordjournals.org/content/early/2014/04/12/bioinformatics.btu170.full.pdf)
2. [Fastx_toolkit](http://hannonlab.cshl.edu/fastx_toolkit/)
3. [The Picard Pipeline](https://www.broadinstitute.org/files/shared/mpg/plathumgen/plathumgen_fennell.pdf)
