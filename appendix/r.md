## R

#### 安装

Ubuntu 下的安装添加 ppa 源来安装最新的 R 版本。

```
sudo add-apt-repository ppa:marutter/rrutter
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install r-base
```

安装 bioconductor: 在 R 界面里操作。

```
> source("http://bioconductor.org/biocLite.R")
> biocLite()
```
