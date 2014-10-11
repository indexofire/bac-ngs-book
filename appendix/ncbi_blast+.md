### 本地 NCBI blast+

1. 下载安装 NCBI Blast+

```
~/tmp$
```

2. 构建自己的数据库

```
~$ makeblastdb -in data/database.fasta -dbtype nucl -parse_seqids
```


### Accession 前缀含义

| Accession 前缀| 分子类型 | 含义 |
| --            | --       | --   |
| AC_ | Genomic | 2:2 |
| NC_ | Genomic | 完整的基因组 |
| NG_ | Genomic | 不完整的基因组 |
| NT_ | Genomic | 2:5 |
| NW_ | Genomic | 2:6 |
| NS_ | Genomic | 环境序列 |
| NZ_b | Genomic | 未完成的WGS |
| NM_ | mRNA | 2:9 |
| NR_ | RNA | 2:10 |
| XM_c | mRNA | 2:11 |
| XR_c | mRNA | 假设的模型 |
| AP_| Protein | 2:13 |
| NP_ | Protein | 2:14 |
| YP_c | Protein | 2:15 |
| XP_c | Protein | 2:15 |
| ZP_c | Protein | 2:15 |

**http://www.personal.psu.edu/iua1/courses/files/2014/lecture-12.pdf**
