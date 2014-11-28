## Local Blast 使用说明

Blast 工具拥有序列相似性比对的功能，在 NGS 数据分析中经常会被使用到，特别是一些工具中需要 Blast 来作为第三方工具调用。不过很多人容易搞混`blast`和`blast+`这2个程序集。NCBI 最早在1989年创建`Basic Local Alignment Search Tool`工具，沿用至2009年无论是命令行工具或是在线程序，都称呼其为`blast`。2009年 NCBI 鉴于老程序的一些不足，重新开发了新的`blast+`命令行工具，新的 blast+ 工具在速度上有了提升，在输入输出上也更为灵活。要区分2者也很简单，blast 是通过 blastall -p 的方式调用子程序来比对搜索的，而 blast+ 则是直接使用 blastn 或 blastp 来比对搜索。另外前者用 formatdb 程序来格式化数据库，后者用 makeblastdb 程序来格式化数据。

* **安装**

使用 ubuntu 包安装 Blast。

```
$ sudo apt-get install blast2
```

* **运行 Blast**

在命令行中输入`blastall`，会打印出一份参数列表。你也可以使用 `man blast` 来查看 Blast 工具的用户手册。

```
$ blastall
```

**参数说明：**

**-p**: 指定要运行的 blast 程序。可以调用的有`blastp`, `blastn`, `blastx`, `psitblastn`, `tblastn`, `tblastx` 这些子程序。这与网页版的 Blast 调用的算法是一致的。最常用的`blastp`是将氨基酸序列与蛋白质数据库比对，`blastn`是将核酸序列与核酸数据库比对，`blastx`是将核酸序列翻译成蛋白序列后与非冗余(nr)蛋白质数据库比对。

**-d**: 指定要调用数据库，默认值是非冗余(nr)数据库。本地 Blast 比对一般来说都是调用 formatdb 格式化的数据库。

**-i**: 输入，默认值是终端输入，也可以使用文件的方式，比如`-i seq.fasta`

**-o**: 输出，默认值是终端打印输出，也可以使用文件的方式，比如`-o result.txt`

**-e**: 期望值。

**-m**: 比对结果的输出显示方式，值为0~11，默认是0。各个数字含义见下表。

| options | meaning |
| -- | -- |
| 0 | pairwise |
| 1 | query-anchored showing identities |
| 2 | query-anchored no identities |
| 3 | flat query-anchored, show identities |
| 4 | flat query-anchored, no identities |
| 5 | query-anchored no identities and blunt ends |
| 6 | flat query-anchored, no identities and blunt ends |
| 7 | XML Blast output |
| 8 | tabular |
| 9 | tabular with comment lines |
| 10 | ASN, text |
| 11 | ASN, binary [Integer] |

**-T**: 输出文件格式，默认值为F(False)，想输出HTML格式的可以用`-T T`

**-M**: 矩阵算法选择，默认值是`BLOSUM62`

**-n**: 如果想使用MegaBlast，就设置`-n T`

**-b**: 数据库中的比对结果显示条目数，默认是250条记录。

其他参数参见`man blast`。

**应用举例**:

Local blast例子：首先下载一个基因组文件并格式化作为本地数据库，然后使用 blastn 对序列进行比对。
```
$
```
