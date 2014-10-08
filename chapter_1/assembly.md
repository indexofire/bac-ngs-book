# 细菌基因组组装工具

### de novo assembly 流程图


1. 安装 sra toolkit (for ubuntu x64 version)

```
~tmp $ curl -O http://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/2.4.1/sratoolkit.2.4.1-ubuntu64.tar.gz
~tmp $ tar zxvf sratoolkit.2.4.1-ubuntu64.tar.gz -C ~/app
~tmp $ cd ~/app/sratoolkit.2.4.1-ubuntu64
~tmp $ sudo ln -s `pwd`/bin/* /usr/local/sbin/
```

2. QC

3. trimming

### 组装工具的选择

原核生物的组装工具最常见的例如 velvet 和 等等。随着目前 MiSeq 读长的不断增加，其他拼接工具大量涌现，针对日常工作的一些问题做了优化。

一套reads数据往往不同的拼接软件会产生不同的拼接结果，甚至彼此之间的差异也会比较大


#### 1. SPAdes 的安装

```
~tmp $
```

##### *Reference*


