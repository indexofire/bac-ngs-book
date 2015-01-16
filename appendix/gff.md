## GFF 文件

GFF格式是Sanger研究所定义，是一种简单的、方便的对于DNA、RNA以及蛋白质序列的特征进行描述的一种数据格式，比如序列的那里到那里是基因，已经成为序列注释的通用格式，比如基因组的基因预测，许多软件都支持输入或者输出gff格式。目前格式定义的最新版本是版本3。gff是纯文本文件，由tab键隔开的9列组成，以下是各列的说明：

**表1. GFF文件各列含义及说明**

| 列 | 名称 | 含义 |
|----|------|------|
|**Column 1**| seqid | 序列的编号，编号的有效字符[a-zA-Z0-9.:^*$@!+_?-] |
|**Column 2**|source|注释信息的来源，比如”Genescan”、”Genbank”等，可以为空，为空用”.”点号代替|
|Column 3**|type|注释信息的类型，比如Gene、cDNA、mRNA等，或者是SO对应的编号|
|**Column 4**| start | 序列开始位置|
|**Column 5**| end | 序列结束位置|
|**Column 6**| score |得分，数字，是注释信息可能性的说明，可以是序列相似性比对时的E-values值或者基因预测是的P-values值。”.”表示为空。|
|**Column 7**|strand|序列的方向， +表示正义链, -反义链 , ? 表示未知.|
|**Column 8**|phase|仅对注释类型为 “CDS”有效，表示起始编码的位置，有效值为0、1、2。|
|**Column 9**|attributes|以多个键值对组成的注释信息描述，键与值之间用”=“，不同的键值用”;“隔开，一个键可以有多个值，不同值用”,“分割。注意如果描述中包括tab键以及”,=;”，要用URL转义规则进行转义，如tab键用 %09代替。键是区分大小写的，以大写字母开头的键是预先定义好的，在后面可能被其他注释信息所调用。|

**表2. 属性attributes列中各注释信息键值名称及含义**

|键值|含义|
|--|--|
|**ID**|注释信息的编号，在一个GFF文件中必须唯一；|
|**Name**|注释信息的名称，可以重复；|
|**Alias**|别名|
|**Parent Indicates**|该注释所属的注释，值为注释信息的编号，比如外显子所属的转录组编号，转录组所属的基因的编号。值可以为多个。|
|**Target Indicates**|核酸或蛋白比对的目标位置|
|**Gap**|The alignment of the feature to the target if the two are not collinear (e.g. contain gaps).|
|**Derives_from**|Used to disambiguate the relationship between one feature and another when the relationship is a temporal one rather than a purely structural “part of” one. This is needed for polycistronic genes.|
|**Note**|备注|
|**Dbxref**|数据库索引|
|**Ontology_term**|A cross reference to an ontology term.|

**GFF文件模板**

```
SEQ1  EMBL  atg   103  105  .  +  0
SEQ1  EMBL  exon  103  172  .  +  0
SEQ1  EMBL  splice5  172  173  .  +  .
SEQ1  gene  splice5  172  173  0.94  +  .
SEQ1  genie  sp5-20  163  182  2.3  +  .
SEQ1  genie  sp5-10  168  177  2.1  +  .
SEQ2  grail  ATG  17  19  2.1  -  0
```

#### Reference

1. http://www.sanger.ac.uk/resources/software/gff/spec.html
2. http://boyun.sh.cn/bio/?p=1602
3. http://www.sequenceontology.org/gff3.shtml
