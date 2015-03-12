## SPAdes

[SPAdes][] 是由俄罗斯科学院 St. Petersburg Academic University 与美国科学家合作开发的主要应用于小型基因组如细菌，真菌等基因组测序数据的拼接软件。目前的最新版本 v3.5.0 可以支持常见的 illumina miseq/hiseq 和 ion torrent 测序数据，对单分子测序平台的 pacbio 和 nanopore 的测序数据也能进行拼装。

#### 1. 下载并安装 SPAdes

直接下载预编译安装包，并添加路径到环境变量中。

```
~/tmp$ curl -O http://spades.bioinf.spbau.ru/release3.5.0/SPAdes-3.5.0-Linux.tar.gz
~/tmp$ tar zxvf SPAdes-3.5.0-Linux.tar.gz -C ~/app
~/tmp$ echo 'export PATH=$PATH:~/app/SPAdes-3.5.0-Linux/bin' >> ~/.bashrc
~/tmp$ source ~/.bashrc
```

`spades.py --test` 命令可以测试安装是否成功，当终端看到如下提示时，表示安装成功可以使用了。

```
========= TEST PASSED CORRECTLY.

SPAdes log can be found here: /home/ubuntu/spades_test/spades.log

Thank you for using SPAdes!
```

也可以下载源码自行编译安装。编译需要安装一些依赖库和工具:

* g++ (version 4.7 or higher)
* cmake (version 2.8.8 or higher)
* zlib
* libbz2

首先我们需要先确保系统中已经安装这些依赖。

```
~/$ sudo apt-get install cmake g++ zlib1g-dev libbz2-dev
```

然后下载源代码，自行编译安装。

```
~/tmp$ curl -O http://spades.bioinf.spbau.ru/release3.5.0/SPAdes-3.5.0.tar.gz
~/tmp$ tar zxvf SPAdes-3.5.0.tar.gz -C ~/app
~/tmp$ cd ~/app/SPAdes-3.5.0
~/app$ sudo PREFIX=/usr/local ./spades_compile.sh
```

#### 2. 拼装基因组

**illumina数据**

对于PE数据，命令行支持最多5组PE，如果要使用更多PE数据，可以使用YAML文件来配置。

以1株甲型副伤寒沙门菌基因组测序数据为例，SRA Run Accession:ERR571271。如果安装了`sra toolkit`和`ascpera`，可以直接`prefetch`抓取数据，否则可以用`wget`等命令下载ftp上的数据，速度会慢一些。

```
~/data$ prefetch -v ERR571271
# or using: curl -O ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByRun/sra/ERR/ERR571/ERR571271/ERR571271.sra

~/data$ fastq-dump ERR571271.sra --gzip
~/data$ spades.py --12 ERR571271.fastq.gz -o ERR571271_output/
~/data$ spades.py -1 SRR95386_1.fastq -2 SRR955386_2.fastq -o spades_output
```

SPAdes会尝试不同的Kmer，因此拼装时间也会根据Kmer选择数量成倍增加。一般一个大肠杆菌基因组拼接，最好在4核/16G内存的机器上运行，大约要几个小时左右的时间。虽然相对于velvet等固定kmer的工具来说，开销会大一些，但是出来的拼接结果在GAGB中评价是比较高的，特别是针对PE250的Miseq平台，目前Hiseq也可以PE250。

对于多组PE数据，可以用以下参数来组装：

```
~/data spades.py --pe1-1 input-1_forward.fastq.gz --pe1-2 input-1_reverse.fastq.gz --pe2-1 input-2_forward.fastq.gz --pe2-2 input-2_reverse.fastq.gz -o output
```

**ion torrent数据**

#### 3. 结果输出

输出目录中的文件：

corrected 目录 BayesHammer 矫正过的 reads 文件
config.fasta/config.fastg 是 contig 结果文件
scaffolds.fasta/scaffolds.fastg 是 scaffold 结果文件
params.txt 拼接参数信息
spades.log 运行日志
dataset.info 内部配置文件
input_dataset.yaml YAML格式的配置文件
K<##>/ 目录包含不同kmer运行结果产出的结果

[SPAdes]: http://spades.bioinf.spbau.ru/ "SPAdes"
