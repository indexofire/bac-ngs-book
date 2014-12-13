## 开始之前

本笔记采用滚动更新方式，每天坚持写一点，长期坚持下来，能够对微生物基因组数据分析有个比较充分的认识。将来可能还会进一步加入转录组甚至蛋白组数据分析的内容。

PS：年底事情多，慢慢学习，慢慢填坑。

## 关于本笔记

本笔记主要是介绍微生物，特别是病原细菌的高通量测序数据分析。目前的发展来看，个人认为小型台式测序仪很快在各个科研院所以及事业单位普及，比如像 Miseq 和 Nextseq 500 这样的测序仪。以后对于数据分析的工具也会越来越多，越来越方便。本笔记的目的也是为了能通过学习，帮助笔者比较全面的掌握微生物基因组（加上转录组，未来也许还有蛋白质组学）数据分析的原理，操作以及细节，同时希望也能帮助一些希望能从高通量数据中挖掘更多信息的从事病原细菌的研究人员。

本笔记使用开源工具 [gitbook][] 创作，介绍的工具也几乎均开源软件。如果阅读者有一定 [Linux][] 基础，对工具的安装和操作理解起来会更为方便一些。笔记中的许多内容都是来源于网络上的资料，部分来源可能有误或记忆不，如果原作者发现原文链接是错误的，请通知我修改。同时希望 [Open Source][] 思想能对科研工作和科研工作者有所帮助。

本笔记代码托管在 [Github][], 所有内容可以从 [这里](http://github.com/indexofire/bac-ngs-book.git) 获得。笔者接触 NGS 时间不长，内容中肯定会有许多错误之处，希望能有专家指正，大家多多提交 [issues](https://github.com/indexofire/bac-ngs-book/issues)，帮助完善笔记内容。一个人的眼界毕竟有限，测序技术也在快速发展，欢迎大家原创或者收集更好的内容也能加入到本笔记中。

#### Fork笔记

1. 在GitHub上fork本书作为自己的仓库，如`indexofire/bac-ngs-book`，然后`git clone`到本地，并设置用户信息。

```
$ git clone git@github.com:your_github_username/bac-ngs-book.git
$ cd bac-ngs-book
$ git config user.name "your github username"
$ git config user.email your_email@something.com
```

2. 修改内容后提交，并`git push`到之前`fork`的仓库。

```
$ git commit -am "Fix issue #1: change typo: helo to hello"
$ git push
```

3. 在GitHub网站上提交`pull request`。

4. 定期使用项目仓库内容更新自己仓库内容。

```
$ git remote add upstream https://github.com/indexofire/bac-ngs-book.git
$ git fetch upstream
$ git checkout master
$ git merge upstream/master
$ git push origin master
```

## 版本更新

##### v0.0.6: 2014.12.01

 * 修改章节结构
 * 添加 local blast 说明

##### v0.0.5: 2014.10.24

 * 添加Amazon EC2教程

##### v0.0.4: 2014.10.20

 * 增加拼装报告部分内容
 * 增加fastq文件修饰内容

##### v0.0.3: 2014.10.15

 * 修正一些错误
 * 增加了一些资源内容

##### v0.0.2: 2014.10.10

 * 添加网上资源，名词解释等内容
 * 添加笔记封面

##### v0.0.1: 2014.10.05

 * 创建本书
 * 添加单基因组数据下载，QC，组装等内容

[Linux]: http://www.linux.com/ "Linux"
[Illumina]: http://www.illumina.com/ "Illumina"
[MiSeq]: http://www.illumina.com/search.ilmn?search=MiSeq&Pg=1&ilmn_search_btn.x=1 "MiSeq"
[gitbook]: http://www.gitbook.io/ "Git Book"
[Open Source]: http://opensource.org/ "开源思想"
[Linux]: http://www.linux.com/ "Linux"
[Github]: https://www.github.com/ "Github"
