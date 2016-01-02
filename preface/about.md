## 关于示例

本笔记中的软件和示例均为运行在 [Linux][] （[Ubuntu][] 发行版） 下实现，如果你的电脑操作系统使用的是 [Windows]()，建议你尝试切换到 [Linux][] 来学习。比较简单的方法是通过 [VMware](http://www.vmware.com/) 或 [Virtualbox](https://www.virtualbox.org/) 建立虚拟机，在其中运行 [Linux][]。具体方法请自行搜索相关资料，这里就不具体展开了。如果你电脑操系统使用的是 [Mac OS X](https://www.apple.com/osx/)，除了一些软件以及依赖库的安装会有一些细小的差异。大部分命令行实现是一致的。

对于小型实验室的用户，要研究微生物基因组数据但是没有服务器或者缺乏软硬件安装能力的话，目前比较优秀的解决方式是通过在 [Amazon EC2][] 中运行 [Linux][] 实例来实现。[Amazon EC2][] 的最低配置 [Linux][] 实例提供了一年的免费试用，可以作为入门学习。通过自己数据的计算量，购买适合的配置即可开展日常工作。而且按计算时长付费，当不需要使用时关闭服务即可，也节约经费投入。定义完硬件配置和操作系统后，[Amazon EC2][] 系统的初始化完成很快，通过远程 ssh 访问的方式，避免了大量需要经验的 [Linux][] 安装和优化过程。减少非生物信息学的大量学习开销，让研究人员的注意力集中在专业领域。此外安装的系统可以作为AMI镜像复制到其他容器中，能保证切换网络或者环境配置的一致性。

[Amazon EC2][] 方法参见附录2。此外，个人推荐使用 [docker][]（一个轻量级的操作系统虚拟化解决方案）来建立的配置与主系统隔离的应用环境，可以方便的做到一次创建，任意服务器中运行。


## 其他方法？

对于实在不愿意学习命令行工具，不愿意尝试 [Linux][] 使用环境的，但想自己分析微生物数据的用户，可以尝试 [Orione][] 工具。

[Orione][] 这是一个基于 [Galaxy][] （主流的基于 web 的免费生物数据分析平台） 的专门针对微生物高通量测序数据分析框架，里面集成了常见的微生物数据分析工具，各种测序应用工作流。[Orione][] 也是可以免费使用的。想了解如何使用或者希望自己部署或定义自己的基于 [Galaxy][] 的 [Orione][] 工具集的，可以参考第五章内容，或参阅它的 [文档](http://orione-documentation.readthedocs.org/) 和 [代码](https://bitbucket.org/crs4/orione-tools)。

对于可以接受购买商业化产品的用户，希望有更好用户体验的 GUI 图形程序的本地用户，可以考虑购买 [CLC workbench](http://www.clcbio.com/products/clc-genomics-workbench/)等图形化界面的工具。

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
[docker]: http://www.docker.io "Docker"
[Windows]: http://windows.microsoft.com "Microsoft"
