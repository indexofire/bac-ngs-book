# structure

### 从基因组数据获得 MLST 型别，毒力与耐药基因
简单来说就是做 in silico MLST 实验和 gene screening 实验。用 srst2 可以实现直接从 reads 数据中获得，经过尝试发现准确率还是不错的。srst2 是一个由墨尔本大学学院开发的工具，第一代 srst 工具主要从 finished 或 draft 中 scanning，srst2 的好处在于直接用 bowtie2 来实现未拼接数据的 ST 与基因数据抓取。

1. 安装 srst2
```
apps/$ wget ...
apps/$
```

2. 从一个菌株fastq数据了解 ST 型别和毒力与耐药基因

3. 用python脚本批量处理多个菌株
```python
import subprocess
```

