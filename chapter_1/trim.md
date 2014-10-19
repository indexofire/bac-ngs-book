## QC 质量控制
测序数据质量控制类软件
* Picard
* FastQC
* Trimmoma@c
* trimAL
* PRINSEQ
* BMTagger

---

#### FastQC的安装和使用
```
~/tmp$ wget http://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.2.zip
~/tmp$ unzip fastqc_v0.11.2.zip -d ~/app
~/tmp$ cd ~/app/FastQC
~/app$ ./fastqc ~/data/*.fastq
```
生成\*.html报告文件和对应的\*.zip压缩，scp传输到本地后用浏览器打开查看。如果只输入`./fastqc`则会调用图形界面显示结果。FastQC结果由11个模块组成，对于结果报告各个模块的说明可以参见 [FastQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/) 文档。

**Basic Statistics:**

fastq文件的基本信息，可以看到序列数量和读长，fastq文件版本，GC含量等。

**Per base sequence quality:**

最重要的结果模块之一，你可以从这个模块的图示中了解序列中各个碱基的质量。

**Per sequence quality score:**

**Per base sequence content:**

####Fastx_toolkit安装
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

#### 安装trimmomatic工具
```
~/tmp$ wget http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/Trimmomatic-0.32.zip
~/tmp$ unzip -d ~/app Trimmomatic-0.32.zip
```

用trimmomatic工具过滤接头
```
~/data$ java -jar ~/apps/Trimmomatic-0.32/trimmomatic-0.32.jar PE \
> -threads 20 -phred33 reads1.fastq reads2.fastq \
> reads1.clean.fastq reads1.unpaired.fastq reads2.clean.fastq reads2.unpaired.fastq \
> ILLUMINACLIP:~/apps/Trimmomatic-0.32/adapters/TruSeq3-PE.fa:2:30:10 \
> LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:50
```

相关

#### Reference ###
1. *[Trimmomatic: a flexible trimmer for Illumina sequence data](http://bioinformatics.oxfordjournals.org/content/early/2014/04/12/bioinformatics.btu170.full.pdf)*
2. *[Fastx_toolkit](http://hannonlab.cshl.edu/fastx_toolkit/)*
