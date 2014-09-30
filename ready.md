# 关于教程示例

本书教程中的软件和示例均为运行在 [Linux][] （[Ubuntu][] 发行版） 下实现，如果你的电脑操作系统使用的是 [Windows](http://windows.microsoft.com)，建议你尝试切换到 Linux 来学习。比较简单的方法是通过 [VMware](http://www.vmware.com/) 或 [Virtualbox](https://www.virtualbox.org/) 建立虚拟机，在其中运行 [Linux][]。具体方法请自行搜索相关资料，这里就不具体展开了。

如果你电脑操系统使用的是 [Mac OS X](https://www.apple.com/osx/)，由于一般来说大部分命令行实现是一致的，你只需要开一个终端来运行命令即可。但是一些软件以及依赖库的安装会有一些细微的差异。

最佳的解决方式：在 [Amazon EC2][] 中运行 [Linux][]（亚马逊 EC2 提供一年免费的试用,足够学习使用；也可以购买更好的配置作为日常工作中的生产部署），通过远程 ssh 访问的方式来学习。这种方法避免了安装 [Linux][] 系统过程中的大量学习开销，也能保证系统环境配置的一致性，读者对生物信息学在云计算中的应用也能有个初步的认识。方法参见附录

本书中所涉及到的默认配置，需要先新建这些文件夹，以免运行命令是出错。

1. 数据统一放在 `~/data` 目录
2. 应用程序统一安装在 `~/apps` 目录

```
$ cd                      # 进入当前用户主目录
$ mkdir -p data apps      # 在当前用户主目录下新建目录 data 和 apps
```

# 热身

对于的病原微生物研究人员，掌握一点最基本的 [Linux][] 命令会有很大帮助。注意一下内容 `$` 后面的是输入命令，`#` 后的是解释：

```
$ ls                      # 查看当前目录下的文件夹
$ cd /etc                 # 改变当前目录到/etc目录
$ pwd                     # 现实当前所在目录的路径
$ cp ~/a ~/sub/b          # 将用户目录下的文件a复制到sub子目录下命名为b
$ rm ~/a                  # 删除用户目录下的文件a
```

**Unix 下系统程序可以用 `man 命令名` 来查看该命令的参数说明**

[Ubuntu][] 作为最主流的 [Linux][] 发行版之一，有一套自身完善的软件管理命令，因经常会用到，所以来熟悉一下：

```
$ sudo apt-get update && sudo apt-get upgrade   # 系统软件更新
$ sudo apt-get install python-dev               # 安装python-dev
$ sudo apt-get remove python-dev                # 删除python-dev
```

生物信息数据的一些简单操作可以直接使用 [Linux][] 命令或者 sed/awk 等程序来实现。

```
1. wc
2. grep
```

有兴趣的生物学家可以先阅读以下资料，增强对 Unix 的相关知识：
1. [Unix and Perl Primer for Biologists](http://korflab.ucdavis.edu/Unix_and_Perl/current.html)

## 我实在学不会 Linux，有没有别的方案？

对于实在不喜学习命令行工具但仍想自己分析数据的，推荐尝试 [Orione][] 工具。这是一个基于 Galaxy （最主流的基于 web 的免费生物数据分析平台） 的微生物高通量测序数据分析框架，里面集成了常见的微生物数据分析工具，各种测序应用的分析工作流。 [Orione][] 的文档参考[这里](http://orione-documentation.readthedocs.org/)，想研究代码参考[这里](https://bitbucket.org/crs4/orione-tools)。

对于可以接受购买商业化产品的用户，希望带 GUI 的本地软件用户，可以考虑购买 [CLC workbench](http://www.clcbio.com/products/clc-genomics-workbench/)
S
[Linux]: http://www.linux.com/ "Linux"
[Ubuntu]: http://www.ubuntu.com/ "Ubuntu"
[Amazon EC2]: http://aws.amazon.com/cn/ec2/ "Amazon EC2 Service"
[Orione]: http://orione.crs4.it/ "A framework for microbiology ngs data analysis"
