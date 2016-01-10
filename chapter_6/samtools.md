## samtools

**安装 samtools**

`samtools`软件库已经移植到了`github`，你可以直接`git clone`软件仓库，也可以直接下载新版的软件包。

```
# 安装依赖包
~/tmp$ sudo apt-get install libncurses-dev

# 下载软件包
~/tmp$ wget https://github.com/samtools/samtools/releases/download/1.3/samtools-1.3.tar.bz2
~/tmp$ tar xjf samtools-1.3.tar.bz2 -C ~/app
~/tmp$ cd ~/app/samtools-1.3

# 编译并安装
~/app/samtools-1.3$ make
~/app/samtools-1.3$ sudo cp samtools /usr/local/sbin
```

**使用 samtools**
