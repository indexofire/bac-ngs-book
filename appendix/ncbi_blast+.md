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
| NC_ | Genomic | 2:3 |
| NG_ | Genomic | 2:4 |
| NT_ | Genomic | 2:5 |
| NW_ | Genomic | 2:6 |
| NS_ | Genomic | 2:7 |
| NZ_b | Genomic | 2:8 |
| NM_ | mRNA | 2:9 |
| NR_ | RNA | 2:10 |
| XM_c | mRNA | 2:11 |
| XR_c | mRNA | 2:12 |
| AP_| 1:13 | 2:13 |
| NP_ | 1:14 | 2:14 |
| YP_c | 1:15 | 2:15 |
| XP_c | 1:15 | 2:15 |
| ZP_c | 1:15 | 2:15 |
