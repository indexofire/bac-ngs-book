## Genome Visulization

基因组数据可视化是一种将基因组数据转换为便于理解和使用的图表的艺术和技术。随着大数据技术的广泛使用，精通数据可视化艺术的人才


#### 1. IGV

##### 用 samtools 转换数据格式

```
~/data$ samtools faidx NC_012967.1.fasta
~/data$ samtools view -b -S -o bowtie/SRR030257.bam bowtie/SRR030257.sam
~/data$ samtools sort bowtie/SRR030257.bam bowtie/SRR030257.sorted
~/data$ samtools index bowtie/SRR030257.sorted.bam
```

##### IGV下载及安装

```
~/tmp$ wget -O ~/app/readseq http://iubio.bio.indiana.edu/soft/molbio/readseq/java/readseq.jar
~/tmp$ java -cp ~/app/readseq/readseq.jar run NC_012967.1.gbk -f GFF -o NC_012967.1.gff
~/tmp$ wget https://github.com/broadinstitute/IGV/archive/v2.3.37.zip
~/tmp$ unzip IGV_2.3.37.zip -d ~/app
~/tmp$ cd ~/app/IGV_2.3.37
~/app$ java -Xmx2g -jar igv.jar
```

弹出的IGV图形界面，下拉菜单选择`Genomes`/`Create .genome File...`,在弹出的窗口中`FASTA file`选项选择基因组fasta文件，在`Gene File`选项选择转换的文件。

1. Online: UCSC, NCBI Ensembl, GBrowse
2. GUI: IGV, IGB, BamView, Savant, Tablet, GenoViewer, MochiView, SeqMonk, inGAP
3. Web2.0: Anno-J, JBrowse
