## 关于本笔记

本笔记主要是简单介绍微生物，特别是病原细菌的高通量测序数据分析。针对的用户主要是拥有自己的台式测序仪（如[Illumina][] [MiSeq][]等）自己测序的，想分析相关NGS数据结果的，或是希望能从高通量数据中挖掘更多信息的从事病原细菌的研究人员。

本笔记使用开源工具 [gitbook][] 创作，介绍的工具也几乎均开源软件。如果阅读者有一定 [Linux][] 基础，会更好的理解笔记中的一些示例。笔记中的许多内容都是来源于网络上的资料。希望 [Open Source][] 思想能对科研工作有所帮助。

本笔记代码托管在 [Github][], 所有内容可以从 [这里](http://github.com/indexofire/bac-ngs-book.git) 获得。笔者接触 NGS 时间不长，内容中肯定会有许多错误之处，欢迎大家提交 issues。一个人的眼界毕竟有限，欢迎大家提交内容到仓库。

** 方法: **

1. 在GitHub上fork本书作为自己的仓库，如indexofire/bac-ngs-book，然后clone到本地，并设置用户信息。
```
$ git clone git@github.com:your_github_username/bac-ngs-book.git
$ cd bac-ngs-book
$ git config user.name "your github username"
$ git config user.email your_email@something.com
```

2. 修改内容后提交，并推送到自己的仓库。
```
$ git commit -am "Fix issue #1: change typo: helo to hello"
$ git push
```

3. 在GitHub网站上提交pull request。

4. 定期使用项目仓库内容更新自己仓库内容。
```
$ git remote add upstream https://github.com/your_github_username/bac-ngs-book
$ git fetch upstream
$ git checkout master
$ git rebase upstream/master
$ git push -f origin master
```

## 版本更新

##### v0.0.2: 2014.10.10
 * 添加网上资源，名词解释等内容
 * 添加Amazon EC2教程

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
