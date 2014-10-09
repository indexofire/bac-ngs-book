# 关于本书

本书是简单介绍病原微生物，特别是病原细菌的高通量测序数据分析。针对的用户主要是拥有自己的台式测序仪（比如 [Illumina][] [MiSeq][]）想分析自己数据结果，或者是希望能从高通量数据中获取更多信息的从事病原细菌的研究人员。如果阅读者有一定 [Linux][] 基础，会更好的理解本书中的一些示例。

本书代码可以从[这里](http://github.com/indexofire/bac-ngs-book.git)获得，本文内容肯定会有许多错误之处，欢迎大家提交 issues 或者 fork 代码库。

本书采用开源工具 [gitbook][]书写完成，图示采用 graphviz 等工具绘制，书中介绍的工具也几乎均开源软件。科学研究存在许多功利因素，所以很难有 OPEN 的条件。但至少目前有了许多开源的工具，有学习文档和资料。希望 [Open Source][] 的思想能对科研工作有所帮助。

# 关于示例

本书教程中的软件和示例均为运行在 [Linux][] （[Ubuntu][] 发行版） 下实现，如果你的电脑操作系统使用的是 [Windows](http://windows.microsoft.com)，建议你尝试切换到 Linux 来学习。比较简单的方法是通过 [VMware](http://www.vmware.com/) 或 [Virtualbox](https://www.virtualbox.org/) 建立虚拟机，在其中运行 [Linux][]。具体方法请自行搜索相关资料，这里就不具体展开了。

如果你电脑操系统使用的是 [Mac OS X](https://www.apple.com/osx/)，由于一般来说大部分命令行实现是一致的，你只需要开一个终端来运行命令即可。但是一些软件以及依赖库的安装会有一些细微的差异。

最佳的解决方式：在 [Amazon EC2][] 中运行 [Linux][]（亚马逊 EC2 提供一年免费的试用,足够学习使用；也可以购买更好的配置作为日常工作中的生产部署），通过远程 ssh 访问的方式来学习。这种方法避免了安装 [Linux][] 系统过程中的大量学习开销，也能保证系统环境配置的一致性，读者对生物信息学在云计算中的应用也能有个初步的认识。方法参见附录

### Linux 热身

##### 准备工作
[Ubuntu][] 作为最主流的 [Linux][] 发行版之一，有一套自身完善的软件管理命令，因经常会用到，所以来熟悉一下。安装一些必备工具
```bash
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get install curl python-dev build-essential gcc g++ \
    biopython bioperl libbz2-dev
```

本书中所涉及到的默认配置，需要先新建这些文件夹，以免运行命令是出错。数据统一放在 `~/data` 目录；应用程序统一安装在 `~/app` 目录；下载等操作的临时目录在 `~/tmp` 目录建立操作所需的文件夹
```bash
$ cd                         # 进入当前用户主目录
$ mkdir -p data app tmp      # 在当前用户主目录下新建目录 data, app 和 tmp
```

对于的病原微生物研究人员，掌握一点最基本的 [Linux][] 命令会有很大帮助。注意一下内容 `$` 后面的是输入命令，`#` 后的是解释：

```bash
$ ls                      # 查看当前目录下的文件夹
$ cd /etc                 # 改变当前目录到/etc目录
$ pwd                     # 现实当前所在目录的路径
$ cp ~/a ~/sub/b          # 将用户目录下的文件a复制到sub子目录下命名为b
$ rm ~/a                  # 删除用户目录下的文件a
```

下面是几个常用的简单命令，在后面的使用中可能会接触到。用 `man 命令名` 可以查看 manual，直接输入 `命令名 -h` 可以查看参数帮助
```bash
$ wc                          # word count
$ grep                        # output grep
$ head                        # file head
$ tail                        # file tail
```

有兴趣的微生物学研究者可以阅读相关资料，增强对 Unix 的相关知识：
* [Unix and Perl Primer for Biologists](http://korflab.ucdavis.edu/Unix_and_Perl/current.html)
* [The Linux Command Line](http://billie66.github.io/TLCL/book/zh/)

# 其他方法？

对于实在不愿意学习命令行工具，无法摆脱Windows使用环境的，但想自己分析微生物数据的用户，推荐尝试 [Orione][] 工具。

[Orione][]这是一个基于 [Galaxy][] （主流的基于 web 的免费生物数据分析平台） 的专门针对微生物高通量测序数据分析框架，里面集成了常见的微生物数据分析工具，各种测序应用工作流。 [Orione][] 也是可以免费使用的。想了解如何使用或者希望自己部署或定义自己的基于[Galaxy][]的[Orione][]工具集参阅它的 [文档](http://orione-documentation.readthedocs.org/)和[代码](https://bitbucket.org/crs4/orione-tools)。

对于可以接受购买商业化产品的用户，希望带 GUI 的本地软件用户，可以考虑购买 [CLC workbench](http://www.clcbio.com/products/clc-genomics-workbench/)等图形化界面的工具。

[Linux]: http://www.linux.com/ "Linux"
[Illumina]: http://www.illumina.com/ "Illumina"
[MiSeq]: http://www.illumina.com/search.ilmn?search=MiSeq&Pg=1&ilmn_search_btn.x=1 "MiSeq"
[gitbook]: http://www.gitbook.io/ "Git Book"
[Open Source]: http://opensource.org/ "开源思想"
[Linux]: http://www.linux.com/ "Linux"
[Ubuntu]: http://www.ubuntu.com/ "Ubuntu"
[Amazon EC2]: http://aws.amazon.com/cn/ec2/ "Amazon EC2 Service"
[Orione]: http://orione.crs4.it/ "A framework for microbiology ngs data analysis"
[Galaxy]: http://galaxyproject.org/ "Galaxy is an open, web-based platform for data intensive biomedical research"
