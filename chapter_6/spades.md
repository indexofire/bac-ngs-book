## SPAdes

[SPAdes][] 是由俄罗斯和美国科学家合作开发的主要针对小基因组如细菌或真菌等生物的拼接软件。目前最新版本 v3.5.0 支持常见的 illumina 和 ion torrent 数据外，还支持三代测序平台的 pacbio 和 nanopore 等测序数据。

#### 1. 下载并安装 SPAdes

```
~/tmp$ wget http://spades.bioinf.spbau.ru/release3.5.0/SPAdes-3.5.0-Linux.tar.gz
~/tmp$ tar zxvf SPAdes-3.5.0-Linux.tar.gz -C ~/app
~/tmp$ echo 'export PATH=$PATH:~/app/SPAdes-3.5.0-Linux/bin' >> ~/.bashrc
```

注销用户并重新登录后，测试安装是成功

```
~/$ spades.py --test
```

当终端看到如下提示时，表示安装成功可以使用了。

```
...
========= TEST PASSED CORRECTLY.

SPAdes log can be found here: /home/ubuntu/spades_test/spades.log

Thank you for using SPAdes!
```

#### 2. 拼装基因组

**illumina数据**

对于PE数据，命令行支持最多5组PE，如果要使用更多PE数据，可以使用YAML文件来配置。

以1株甲型副伤寒沙门菌基因组测序数据为例，SRA Run Accession:ERR571271

```
~/data$ curl -O ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByRun/sra/ERR/ERR571/ERR571271/ERR571271.sra
~/data$ fastq-dump ERR571271.sra --gzip
~/data$ spades.py --12 ERR571271.fastq.gz -o ERR571271_output/
~/data$ spades.py -1 SRR95386_1.fastq -2 SRR955386_2.fastq -o spades_output
```

SPAdes会尝试不同的Kmer，因此拼装时间也会根据Kmer选择数量成倍增加。一般一个大肠杆菌基因组拼接，最好在4核/16G内存的PC机上运行，大约要几个小时左右的时间。虽然开销比较大，但是出来的拼接结果对于miseq平台在各类评价中基本都是最好的。

对于多组PE数据，可以用一下参数来组装：

```
~/data spades.py --pe1-1 input-1_forward.fastq.gz --pe1-2 input-1_reverse.fastq.gz --pe2-1 input-2_forward.fastq.gz --pe2-2 input-2_reverse.fastq.gz -o output
```

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
