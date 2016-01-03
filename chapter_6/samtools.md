## samtools

#### 安装samtools

`samtools`软件库已经移植到了`github`，你可以直接`git clone`软件仓库，也可以直接下载新版的release包。

```
# 安装依赖包
~/tmp$ sudo apt-get install libncurses-dev

# 下载并安装 samtools
~/tmp$ wget https://github.com/samtools/samtools/releases/download/1.3/samtools-1.3.tar.bz2
~/tmp$ tar xjf samtools-1.3.tar.bz2 -C ~/app
~/tmp$ cd ~/app/samtools-1.3
~/app/samtools-1.3$ make
~/app/samtools-1.3$ sudo cp samtools /usr/local/sbin
```