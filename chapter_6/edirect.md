#edirect

##下载安装

```bash
~/tmp$ wget ftp://ftp.ncbi.nlm.nih.gov/entrez/entrezdirect/edirect.tar.gz
~/tmp$ tar zxvf edirect.tar.gz -C ~/app
~/tmp$ sudo ln -s ~/app/edirect/* /usr/local/sbin/
```

##NCBI 数据库列表

##主要工具列表

| 工具名称 | 用途 |
| -------- | -------- |
| **efetch** | 下载 NCBI 数据库中的记录和报告并以相应格式打印输出 |
| **einfo** | 获取目标结果在数据库中的信息 |
| **elink** | 对目标结果在其他数据库中比配结果 |
| **epost** | 上传 UIDs 或者 序列登记号 |
| **esearch** | 在 Entrez 中执行搜索命令 |
| **efilter** | 对之前的检索结果进行过滤或限制 |
| **xtract** | converts XML into a table of data values. |
| **nquire** | sends a URL request to a web page or CGI service. |

**抓取2012年至今发表的霍乱弧菌CTX相关文献摘要**

```bash
~$ esearch -db pubmed -mindate 2012 -maxdate 2016 -datetype PDAT -query "vibrio cholerae CTX" | efetch -format abstract > abstract.txt
```

**查看2016年CDC发布的所有用 miseq 测序的沙门菌文库制备方法**

```bash
~$ esearch -db sra -query "salmonella miseq CDC" -mindate 2016 -maxdate 2016 -datetype PDAT | efetch -format runinfo | cut -d ',' -f 12 > library.txt
```




