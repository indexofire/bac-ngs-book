# Meta-BEETL

`BEETL` 是一套做 `Burrows-Wheeler Transform` 的工具集，[Meta-BEETL] 是 `BEETL` 下的一个子项目，用来分析基于 Shotgun metagenomics 的微生物数据。

## Meta-BEETL Metagenomics

### 下载数据库

#### 1. 已整理的数据库

作者已经将研究所用到的数据库整理后提交到 Amazon S3 服务器上，可以直接下载。总数据量在24G左右。

```bash
~$ mkdir BeetlMetagenomeDatabase && cd BeetlMetagenomeDatabase
~/BeetlMetagenomDatabase$ for i in `curl https://s3.amazonaws.com/metaBEETL | grep -oP "[^>]*bz2"` ; \
> do wget https://s3.amazonaws.com/metaBEETL/$i & done
```

静态文件的 URL 地址，也可以直接下载。

```
https://s3.amazonaws.com/metaBEETL/filecounter.csv.bz2
https://s3.amazonaws.com/metaBEETL/headerFile.csv.bz2
https://s3.amazonaws.com/metaBEETL/names.dmp.bz2
https://s3.amazonaws.com/metaBEETL/ncbiFileNumToTaxTree.bz2
https://s3.amazonaws.com/metaBEETL/ncbiMicros-B00.bz2
https://s3.amazonaws.com/metaBEETL/ncbiMicros-B01.bz2
https://s3.amazonaws.com/metaBEETL/ncbiMicros-B02.bz2
https://s3.amazonaws.com/metaBEETL/ncbiMicros-B03.bz2
https://s3.amazonaws.com/metaBEETL/ncbiMicros-B04.bz2
https://s3.amazonaws.com/metaBEETL/ncbiMicros-B05.bz2
https://s3.amazonaws.com/metaBEETL/ncbiMicros-B06.bz2
https://s3.amazonaws.com/metaBEETL/ncbiMicros-C00.bz2
https://s3.amazonaws.com/metaBEETL/ncbiMicros-C01.bz2
https://s3.amazonaws.com/metaBEETL/ncbiMicros-C02.bz2
https://s3.amazonaws.com/metaBEETL/ncbiMicros-C03.bz2
https://s3.amazonaws.com/metaBEETL/ncbiMicros-C04.bz2
https://s3.amazonaws.com/metaBEETL/ncbiMicros-C05.bz2
https://s3.amazonaws.com/metaBEETL/ncbiMicros-C06.bz2
```
下载完成后解压缩。

```
~/BeetlMetagenomDatabase$ bunzip2 *.bz2
~/BeetlMetagenomDatabase$ export METAGENOME_DATABASE_PATH=`pwd`
```

#### 2. 自己建立数据库

处于实际目的，有时候我们不需要这么大的数据库，或者需要更多其他的数据加入到数据库，那就需要自行建立数据库。

##### 2.1 安装 SeqAn

[SeqAn](www.seqan.de) 是一个高效的 C++ 算法库，访问 [SeqAn](www.seqan.de) 下载并安装。

Creating a new metagenomic database requires an installation of the SeqAn library (www.seqan.de).  
   You can set the location of your SeqAn installation before compiling BEETL by using the configure parameter --with-seqan.  
   If you do this, all executables mentioned below will be compiled automatically and copied into /installPath/bin.

```bash
# 安装依赖包
~$ sudo apt-get install g++ cmake python zlib1g-dev libbz2-dev libboost-dev
~$ cd ~/apps
~/apps$ git clone https://github.com/seqan/seqan.git && cd seqan
~/apps/seqan$ git checkout -b develop origin/develop

```

##### 2.2 下载目标序列

下载需要的序列，比如NCBI上细菌的基因组 ref 数据。

```bash
~/BeetlMetagenomDatabase$ wget ftp://ftp.ncbi.nlm.nih.gov/genomes/Bacteria/all.fna.tar.gz -P downloads
~/BeetlMetagenomDatabase$ mkdir all.fna
~/BeetlMetagenomDatabase$ tar xzf downloads/all.fna.tar.gz -C all.fna/
```

##### 2.3
3. Create single sequence files with the reverse complement of all genomes but exclude plasmids. For this, use metabeetl-db-genomesToSingleSeq. Do NOT change the names of the generated files.
```
~$ metabeetl-db-genomesToSingleSeq -f all.fna/*/*.fna -s singleSeqGenomes
~$ cd singleSeqGenomes
```

4. Open the script metabeetl-db-arrayBWT.sh and adapt the paths. To create the BWTs for all the G\_\* and G\_\*\_rev files using a Grid Engine cluster, submit `qsub -t n metabeetl-db-arrayBWT.sh`, where n should be 1-500 if you have the files G\_1 till G\_500. As an alternative you can also run metabeetl-db-makeBWTSkew for each of the files individually.
```
~$ cp `which metabeetl-db-arrayBWT.sh` .
~$ vim metabeetl-db-arrayBWT.sh  # Adjust paths in this file
~$ qsub -t n metabeetl-db-arrayBWT.sh
```
 OR
```
~$ (echo -n "all: " ; for i in G_*; do echo -n " bwt_${i}-B00"; done ; echo -e "\n" ; \
> for i in G_*; do echo "bwt_${i}-B00: ${i}"; echo -e "\tmetabeetl-db-makeBWTSkew ${i} ${i}\n" ; done ) > Makefile
~$ make -j
```

5. Login on a machine with enough memory to load all sequences in RAM (~60GB) and run mergeBacteria on all files. You will have to call this once for each of the piles created by BEETL which means six times overall e.g.  
```
~$ for pileNum in `seq 0 5`; do metabeetl-db-mergeBacteria $pileNum ncbiMicros <( ls G_* ) ; done
```  
   For each pile this will create 3 files and one fileCounter.csv (which doesn't change)  
   The output files will be called: ncbiMicros-A0\*, -B0\* and -C0\*  
   - -A0\* contain, for each position in the BWT, the suffix position in the file where this BWT came from.  
   - -B0\* are the BWTs for all files.  
   - -C0\* contain, for each position in the BWT, the file number where the char at this position came from.  
  
   Files -B0\* and -C0\* are needed for the countWords algorithm. 

6. Optionally, convert the ASCII BWT files to RLE BWT, to make subsequent processing faster:  
```
   for pileNum in `seq 0 5`; do \  
     mv ncbiMicros-B0${pileNum} ncbiMicros-B0${pileNum}.ascii ;  \
     beetl-convert \  
       --input-format=bwt_ascii \  
       --output-format=bwt_rle \  
       -i ncbiMicros-B0${pileNum}.ascii \  
       -o ncbiMicros-B0${pileNum} ; \  
   done
```

7. Download the NCBI taxonomy from ftp://ftp.ncbi.nih.gov/pub/taxonomy/  
   You will need the files names.dmp, nodes.dmp and the gi\_taxid\_nucl.dmp, which are packaged as taxdump.tar.gz and gi\_taxid\_nucl.dmp.gz:  
```
   cd downloads
   wget ftp://ftp.ncbi.nih.gov/pub/taxonomy/taxdump.tar.gz         # 26 MB
   wget ftp://ftp.ncbi.nih.gov/pub/taxonomy/gi_taxid_nucl.dmp.gz   # 800 MB

   tar xzf taxdump.tar.gz names.dmp nodes.dmp
   gunzip gi_taxid_nucl.dmp.gz
   cd ..
```
   Use the metabeetl-db-findTaxa script to find the taxonomic tree corresponding to the file numbers in the database.  
   You will need the headerFile produced by running "metabeetl-db-genomesToSingleSeq" and 
   fileCounter created during the merging of the bacterial reference genomes.  
   Finally, you get for each file number in the database a taxonomic tree with the taxonomic ids.  
   There will be some 0 in the taxonomic tree. This is a taxonomic id which could not be 
   matched to: Superkingdom, Phylum, Order, Class, Family, Genus, Species or Strain.  
   Sometimes there are just missing taxa in the taxonomy. We supplement this with the file `metaBeetlExtraNames.dmp` below. 
```
   metabeetl-db-findTaxa \
     -nA downloads/names.dmp \
     -nO downloads/nodes.dmp \
     -nG downloads/gi_taxid_nucl.dmp \
     -h singleSeqGenomes/headerFile.csv \
     -f singleSeqGenomes/filecounter.csv \
     > ncbiFileNumToTaxTree

   ( grep scientific downloads/names.dmp ; cat ${BEETL_INSTALL_DIR}/share/beetl/metaBeetlExtraNames.dmp ) > metaBeetlTaxonomyNames.dmp
```

8. (Experimental, but nice) Run meta-BEETL on each of the genomes to calculate the normalisation factors
```
   mkdir normalisation
   cd normalisation
   for genome in ../singleSeqGenomes/G_*; do
     (
      genomeNum=`basename ${genome}`
      mkdir ${genomeNum}
      cd ${genomeNum}
      for i in ../../singleSeqGenomes/bwt_${genomeNum}-B0?; do ln -s ${i} ; done
      for i in `seq 0 6`; do touch bwt_${genomeNum}-B0${i}; done
      time beetl-compare --mode=metagenomics -a bwt_${genomeNum} -b ../../singleSeqGenomes/ncbiMicros -t ../../ncbiFileNumToTaxTree -w 20 -n 1 -k 50 --no-comparison-skip -v &> out.step1
      rm -f BeetlCompareOutput/cycle51.subset*
                  touch empty.txt
      time cat BeetlCompareOutput/cycle*.subset* | metabeetl-convertMetagenomicRangesToTaxa ../../ncbiFileNumToTaxTree ../../singleSeqGenomes/ncbiMicros ../../metaBeetlTaxonomyNames.dmp empty.txt 20 50 - &> out.step2
      cd ..
     ) &
   done ; wait

   for i in `seq 1 2977`; do echo "G_$i"; X=`grep -P "G_${i}$" ../singleSeqGenomes/filecounter.csv |cut -f 1 -d ','`; TAX=`grep -P "^${X} " ../ncbiFileNumToTaxTree | tr -d "\n\r"` ; echo "TAX(${X}): ${TAX}."; TAXID=`echo "${TAX}" | sed 's/\( 0\)*$//g' |awk '{print $NF}'`; echo "TAXID=${TAXID}"; COUNTS=`grep Counts G_${i}/out.step2 | head -1`; echo "COUNTS=${COUNTS}"; MAIN_COUNT=`echo "${COUNTS}  " | sed "s/^.* ${TAXID}:\([0-9]*\) .*$/\1/ ; s/Counts.*/0/"` ; echo "MAIN_COUNT=${MAIN_COUNT}" ; SUM=`echo "${COUNTS}  " | tr ' ' '\n' | sed 's/.*://' | awk 'BEGIN { sum=0 } { sum+=$1 } END { print sum }'` ; echo "SUM=$SUM"; PERCENT=`echo -e "scale=5\n100*${MAIN_COUNT}/${SUM}" | bc` ; echo "PERCENT=${PERCENT}" ; echo "FINAL: G_${i} ${TAXID} ${MAIN_COUNT} ${SUM} ${COUNTS}" ; done > r2977

   grep FINAL r2977 > ../normalisation.txt
```

9. Link (or move) all the useful files to a final directory
```
   mkdir metaBeetlNcbiDb
   cd metaBeetlNcbiDb
   ln -s ../ncbiFileNumToTaxTree
   ln -s ../normalisation.txt
   ln -s ../downloads/metaBeetlTaxonomyNames.dmp
   ln -s ../singleSeqGenomes/filecounter.csv
   ln -s ../singleSeqGenomes/headerFile.csv
   for i in ../singleSeqGenomes/ncbiMicros-[BC]0[0-5]; do ln -s $i ; done
```

Now everything should be ready to run metaBEETL!  :-)

**Downloading the metagenomic input dataset**

The sample SRS013948 from the Human Microbiome project should be available from:

```
http://downloads.hmpdacc.org/data/Illumina/throat/SRS013948.tar.bz2  
```
or  
```
ftp://public-ftp.hmpdacc.org/Illumina/throat/SRS013948.tar.bz2
```

Decompressing this file
```
~$ tar xjf SRS013948.tar.bz2
``` 
should give you:
```
SRS013948.denovo_duplicates_marked.trimmed.1.fastq
SRS013948.denovo_duplicates_marked.trimmed.2.fastq
SRS013948.denovo_duplicates_marked.trimmed.singleton.fastq
```

We need to normalise the lengths of these sequences and put them all in one file:
```
~$ beetl-convert \
> -i SRS013948.denovo_duplicates_marked.trimmed.1.fastq \
> -o paddedSeq1.seq \
> --sequence-length=100
~$ beetl-convert \
> -i SRS013948.denovo_duplicates_marked.trimmed.2.fastq \
> -o paddedSeq2.seq \
> --sequence-length=100
~$ beetl-convert \
> -i SRS013948.denovo_duplicates_marked.trimmed.singleton.fastq \
> -o paddedSeqSingleton.seq \
> --sequence-length=100
~$ cat paddedSeq1.seq paddedSeq2.seq paddedSeqSingleton.seq > SRS013948.seq
```

**Running BEETL in metagenomic mode**

Create the BWT of the input dataset:
```
~$ beetl-bwt -i SRS013948.seq -o bwt_SRS013948
```

Run BEETL metagenomic classification:
```
~$ beetl-compare \
> --mode=metagenomics \
> -a bwt_SRS013948 \
> -b ${METAGENOME_DATABASE_PATH}/ncbiMicros \
> -t ${METAGENOME_DATABASE_PATH}/ncbiFileNumToTaxTree \
> -w 20 \
> -n 1 \
> -k 50 \
> --no-comparison-skip
```

It currently generates many output files in a `BeetlCompareOutput` directory, organised per cycle and containing the information about the BWT ranges of k-mers that start diverging between the input dataset and the database.
Setting k = sequence length gets you the maximal amount of information but the output file will be large.

**Gathering and visualisation of metagenomic results**

The `metabeetl-convertMetagenomicRangesToTaxa` tool converts the BWT ranges of k-mer matches from the previous stage into genome and ancestor IDs and generates text and graphical output files (currently html files using the Krona javascript visualisation library).

Since the algorithm repeatedly looks up the filenumbers for each BWT position we recommend to put these ncbiMicros-C0* files on a disk with fast read access.
Alternatively, if you have enough RAM, you can try `metabeetl-convertMetagenomicRangesToTaxa_withMmap`, which maps these files to RAM.

Run:
```
~$ cat BeetlCompareOutput/cycle*.subset* | \
> metabeetl-convertMetagenomicRangesToTaxa \
> ${METAGENOME_DATABASE_PATH}/ncbiFileNumToTaxTree \
> ${METAGENOME_DATABASE_PATH}/ncbiMicros \
> ${METAGENOME_DATABASE_PATH}/metaBeetlTaxonomyNames.dmp \
> ${METAGENOME_DATABASE_PATH}/normalisation.txt \
> 20 50 - > metaBeetl.log
```

Three TSV (tab-separated values) files are generated (column 1: Taxonomy Id, column 2: Taxonomy level, column 3: k-mer count, column 4: k-mer count including children):
- metaBeetl.tsv: raw k-mer counts for every leaves and ancestors.
- metaBeetl_normalised.tsv: some counts from ancestors are moved towards leaf items. The proportion thereof is pre-computed by aligning each individual genome to the full database.
- metaBeetl_normalised2.tsv: Only leaves
 of the taxonomy tree are kept, and counts are normalised relatively to genome sizes. 

Three Krona files are also generated:
- metaBeetl_krona.html
- metaBeetl_krona_normalised.html
- metaBeetl_krona_normalised2.html

---

#### Mash

```
~$
```

---

## Reference

* http://www.ncbi.nlm.nih.gov/pmc/articles/PMC3622627/
* https://github.com/BEETL/BEETL.git
* https://github.com/marbl/Mash.git
* http://mash.readthedocs.org/en/latest/


[metaBEETL]: https://github.com/BEETL/BEETL