## 关于本笔记

随着高通量测序技术快速发展，小型台式测序仪的出现降低了高通量测序的门槛。对于开展微生物特别是病原微生物的测序工作的需求，在各种临床，科研以及政府的小型实验室中迅速扩展。目前已经上市的比较适合小型实验室的测序仪如 `Illumina Miseq`，`Illumina NextSeq500`，`Ion S5`，`Ion PGM` 等，即将上市的测序仪如 `Illumina MiniSeq`，`Qiagen GeneReader` 。除了传统的二代测序仪，不需要扩增的基于单分子测序技术的三代测序技术的 `PacBio Sequel` 和最近开展埃博拉实时测序项目的基于纳米孔技术的 `Oxford MinION` 产品都是非常适合开展病原微生物测序的平台。这些平台各有自身的优缺点，这里不对仪器做过多评价。本笔记主要针对 Miseq 机器的产出数据开展分析，由于原理类似分析软件也打都可以应用在 Illumina 其他型号的仪器数据上。

测序数据的不断产出，需要大量的生物信息学人才来参与到测序数据转换成可读数据以便用于各项工作中。不同于由测序公司负责开展的生物信息学分析，对于许多小型实验室来说，招募到优秀的生物信息学人员开展专业的数据分析是非常困难的事情。但除了科研工作外，大部分基本测序数据的分析已经有许多工具流来帮助实现。对于非专业背景的人来说，命令行以及软件的安装可能是学习曲线中较为陡峭的部分。

本书内容主要是介绍微生物，特别是病原细菌的高通量测序数据分析。目的也是为了能通过学习，帮助原来从事湿实验的同事能较好的掌握高通量测序数据分析，将学习曲线变得平缓。同时也希望能对从事该领域的其他工作者提供便利，让大家在需要分析微生物基因组数据时，能有一个资料与实例汇集的手册可以快速参考和学习。

内容上主要以台式测序仪 Illumina Miseq 以及其数据为基础。例子都是运行在安装了 Ubuntu 发行版的 Linux PC服务器以及个人电脑上。

![](assets/img/miseq.jpg) ![](assets/img/ubuntu.jpg)

本笔记使用开源工具 [Gitbook][] 创作，介绍的工具也几乎均为开源软件。笔记中的许多内容都是来源于网络上的资料，部分来源可能有误或记忆不全，如果原作者发现没有内容链接，或者链接错误，请发送 [电子邮件](mailto:indexofire@gmail.com) 通知修改。同时希望 [Open Source][] 思想能对科研工作和科研工作者有所帮助。

本笔记代码托管在 [Github][], 所有内容可以在 http://github.com/indexofire/bac-ngs-book.git 获得。笔者接触 NGS 时间不长，内容中肯定会有许多错误之处，希望能有NGS领域的专家帮助指正。欢迎大家提交 [issues](https://github.com/indexofire/bac-ngs-book/issues)，帮助完善笔记内容。

#### Fork笔记方法

1. 在GitHub上访问本书源码，并fork成为自己的仓库。如下图所示，点击fork按钮，然后用 `git clone` 到本地，设置好代码仓库用户信息。就可以修改并提交代码了。
![](assets/img/fork.png)
```
~$ git clone git@github.com:your_github_username/bac-ngs-book.git
~$ cd bac-ngs-book
~/bac-ngs-book$ git config user.name "your github username"
~/bac-ngs-book$ git config user.email your_email@something.com
```

2. 对需要的内容做修改后提交，并`git push`到之前`fork`的仓库。
```
~/bac-ngs-book$ git add -A
~/bac-ngs-book$ git commit -am "Fix issue #1: change typo: helo to hello"
~/bac-ngs-book$ git push
```

3. 这样你自己的代码分支`master`与源代码分支就会出现差异，要合并这些差异，就要在GitHub网站上提交`pull request`。如下图所示如果有代码。
![](assets/img/pull_request.png)

4. 由于源代码仓库也会经常更新或者合并其他pull request代码，需要定期使用源项目仓库内容来更新自己仓库内容。
```
~/bac-ngs-book$ git remote add upstream https://github.com/indexofire/bac-ngs-book.git
~/bac-ngs-book$ git fetch upstream
~/bac-ngs-book$ git checkout master
~/bac-ngs-book$ git merge upstream/master
~/bac-ngs-book$ git push origin master
```

## 版本更新

v0.0.1: 2016.01.01

 * 对原有内容做了适当调整，逐步补充完整原计划的章节内容。

[Linux]: http://www.linux.com/ "Linux"
[Illumina]: http://www.illumina.com/ "Illumina"
[MiSeq]: http://www.illumina.com/search.ilmn?search=MiSeq&Pg=1&ilmn_search_btn.x=1 "MiSeq"
[gitbook]: http://www.gitbook.io/ "Git Book"
[Open Source]: http://opensource.org/ "开源思想"
[Linux]: http://www.linux.com/ "Linux"
[Github]: https://www.github.com/ "Github"
