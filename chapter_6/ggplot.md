## ggplot2

ubuntu14.04 LTS 官方源版本为3.0，安装ggplot2时一些依赖包无法正常安装，因此需要添加更新源。这样安装的R就是3.2版本的了。

```
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 51716619E084DAB9
$ echo 'deb http://mirror.bjtu.edu.cn/cran/bin/linux/ubuntu trusty/
$ sudo apt-get update
$ sudo apt-get upgrade
```

```
$ R
R version 3.2.3 (2015-12-10) -- "Wooden Christmas-Tree"
Copyright (C) 2015 The R Foundation for Statistical Computing
Platform: x86_64-pc-linux-gnu (64-bit)
...

# 安装ggplot2
> install.packages("ggplot2")
```

