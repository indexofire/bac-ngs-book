# 病原细菌基因组组装工具

### de novo sequencing

全基因组 de novo sequencing 也叫做基因组从头测序，是指不依赖于任何已知基因组序列信息对某个物种的基因组进行测序，然后应用生物信息学手段对测序序列进行拼接和组装，最终获得该种基因组序列图谱。

### de novo assembly

由于细菌基因组的特点，一般都是采用 de novo sequencing 的方式来测细菌基因组。因此采用的拼接方法就称其为 de novo assembly。

##### 1. 从 NCBI 下载单个基因组数据
**方法1** 直接用浏览访问 [NCBI](http://) 网站的 SRA 数据库，找到所要下载的基因组测序原始数据，点击 Download 即可。

**方法2** 使用 NCBI edirect 工具。

```
$ wget ftp://ftp.ncbi.nlm.nih.gov/entrez/entrezdirect/edirect.tar.gz
$ tar zxvf edirect.tar.gz ~/Apps
$ sudo ln -s ~/Apps/edirect/* /usr/local/bin/
```

用 edirect 在 SRA 数据库中搜索

```
$ esearch | efetch
```
1. 安装 sra toolkit

```
$ wget ftp://ftp.ncbi.nlm.nih.gov/entrez/entrezdirect/edirect.tar.gz
$ tar zxvf edirect.tar.gz ~/Apps
```

2.



##### *Reference*

1. *[Courses at Penn State](http://www.personal.psu.edu/iua1/lectures.html)*

2. *new*
