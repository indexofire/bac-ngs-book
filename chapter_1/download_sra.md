## 测序数据的获得

对于单个SRA数据，本地电脑可以通过浏览器下载。这里讨论在服务器端的操作。细菌基因组数据以 illumina MiSeq PE250 的测序 数据为例，其他测序数据格式将在附录中探讨。

#### 认识 SRA 数据库



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

**Notes:** *2.3.x版本的 sra_toolkit 会有些配置问题，建议使用最新版。不过2.4.x版本的程序调用ascp时，比如 prefetch 调用 ascp 时选择的 ssh 密钥是老版 `asperaweb_id_dsa.putty`, 而目前新版的ascp在linux里是使用 `asperaweb_id_dsa.openssh`。*

##### 3. 用aspera connect下载
用aspera connect工具下载NCBI的数据速度非常快，而且不光可以下载SRA数据库里的数据，还可以下载NCBI FTP里的其他资源。
```
~/data$ ascp -i ~/.aspera/connect/etc/asperaweb_id_dsa.openssh --user=anonftp --host=ftp-private.ncbi.nlm.nih.gov --mode=recv /sra/sra-instant/reads/ByRun/sra/ERR/ERR175/ERR175655 .
```

#### 批量下载数据的方法

1. 使用 aspera connect
2. 编写脚本
