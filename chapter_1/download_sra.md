# 测序数据的获得

对于单个SRA数据，本地电脑可以通过浏览器下载。这里讨论在服务器端的操作。细菌基因组数据以 illumina MiSeq PE250 的测序 数据为例，其他测序数据格式将在附录中探讨。

### 认识 SRA 数据库


##### 1. edirect 安装

```
~/tmp$ wget ftp://ftp.ncbi.nlm.nih.gov/entrez/entrezdirect/edirect.tar.gz
~/tmp$ tar zxvf edirect.tar.gz -C ~/app
~/tmp$ sudo ln -s ~/app/edirect/*
```

##### 2. 搜索并下载数据

选择美国 FDA 提交的 SRR955386 数据。该数据是在Illumina Miseq平台上PE250的数据。生物样本是一株分离自奶酪的单增李斯特菌 CFSAN006122。如果你已经安装好 NCBI 的 sra_toolkit 工具，可以直接用里面的 prefetch 下载。

```bash
~/data$ prefetch -v SRR955386
~/data$ mv ~/.ncbi/public/sra/SRR955386.sra .
```

**Notes:** *有些版本的 sra_toolkit 会有设置问题，建议使用最新版。而2.4.1 版本的 prefetch 调用 ascp 时选择的 ssh 密钥也是 windowns 版本的 .putty 而不是 .openssh，希望 bug 在新版本会有所修复。*

### 批量下载数据的方法

1. 使用 aspera connect
2. 编写脚本
