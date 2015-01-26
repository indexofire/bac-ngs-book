## Fastq 剪切与修饰

二代测序单个 RUN 的测序数据量比较大，而微生物基因组比较小，所以测微生物一般都是混样，通过添加 adaptors 接头来区分不同样本。因此首先要对数据进行去除接头的处理。

#### 常用测序数据去除接头软件

* Trimmomaic
* trimAL


* FastQC
* Picard
* PRINSEQ
* BMTagger


### Remove adaptors

#### 1. Trimmomatic 去除接头

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
>    s1_pe s1_se s2_pe s2_se \
>    ILLUMINACLIP:~/data/adaptors/illuminaClipping.fa:2:30:10
```

1.2 用 trimmomatic 自带接头文件处理

```
~/data$ java -jar ~/apps/Trimmomatic-0.32/trimmomatic-0.32.jar PE \
>     -threads 20 -phred33 reads1.fastq reads2.fastq \
>     reads1.clean.fastq reads1.unpaired.fastq reads2.clean.fastq reads2.unpaired.fastq \
>     ILLUMINACLIP:~/apps/Trimmomatic-0.32/adapters/TruSeq3-PE.fa:2:30:10 \
>     LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:50
```

#### 3. Fastx_toolkit
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
