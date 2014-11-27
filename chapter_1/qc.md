## 质量控制

去除序列接头后，就要对测序质量做个QC。

#### 1. FastQC

```
~/tmp$ wget http://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.2.zip
~/tmp$ unzip fastqc_v0.11.2.zip -d ~/app
~/tmp$ cd ~/app/FastQC
~/app$ ./fastqc ~/data/*.fastq
```

生成\*.html报告文件和对应的\*.zip压缩，scp传输到本地后用浏览器打开查看。如果只输入`./fastqc`则会调用图形界面显示结果。FastQC结果由11个模块组成，对于结果报告各个模块的说明可以参见 [FastQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/) 文档。

| Module | Explanation |
| -- | -- |
| **Basic Statistics:** | fastq文件的基本信息，可以看到序列数量和读长，fastq文件版本，GC含量等。 |
| **Per base sequence quality:** | 最重要的结果模块之一，你可以从这个模块的图示中了解序列中各个碱基的质量。 |
| **Per sequence quality score:** | ... |
| **Per base sequence content:** | ... |

#### 2. Picard

[Picard][] 是 [BroadInstitute][] 使用 Java 语言开发的针对 BAM 等高通量测序数据的处理工具，特别是针对 Illumina 平台的数据。它可以提供一个测序质量和数据基本特性的报告。

```
~/tmp$ wget https://github.com/broadinstitute/picard/releases/download/1.123/picard-tools-1.123.zip
~/tmp$ unzip -n picard-tools-1.123.zip -d ~/app
```

[Picard]: http://broadinstitute.github.io/picard/ "Picard"
[BroadInstitute]: https://www.broadinstitute.org/ "Broad Institute"




#### khmer

```
~/app$ git clone https://github.com/ged-lab/khmer.git && cd khmer
~/app$ git checkout v1.1
~/app$ sudo make install
```

#### Reference ###

1. [Fastx_toolkit](http://hannonlab.cshl.edu/fastx_toolkit/)
2. [The Picard Pipeline](https://www.broadinstitute.org/files/shared/mpg/plathumgen/plathumgen_fennell.pdf)
