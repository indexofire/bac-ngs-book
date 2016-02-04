# Mapping

公共卫生领域以往已经建立起了比较完整的病原菌分型与溯源的网络体系，如PulseNet,FoodNet等。PFGE可以说是其中的技术精标准。随着NGS的快速发展，用基因组测序数据来追踪病原菌的关系越来越成为一种主流发展方向。

从100k pathogen project，到德国O104:H4追踪，H7N9禽流感溯源，埃博拉病毒基因组流行病学研究以及最近的寨卡病毒流行等案例都大量的运用了NGS技术。

大部分项目都是通过对样品基因组数据进行比对获得SNPs，根据SNPs来构建ML进化树，通过对树的拓扑结构分析以及SNPs数量差异等因素来进行分型与溯源，并可以了解种群结构与进化。

也有用距离法来分析的项目，比如100k pathogen project由于菌株数量太大，数据需要实时更新，且分析意义主要不在于进化关系，因此采用的是计算kmer距离，Jaccard distance的方法，并用FastME来构建进化树。

## 构建SNPs的方法

一般有2种技术路线：

- 先对基因组测序数据进行 de novo assembly，然后比对contigs与reference，构建SNPs。
- 直接用 Reads Mapping 软件将测序短序列比对到 reference 并获得SNPs等结果。

## 基因组 Reads Mapping 软件

- bwa
- bowtie

#### 