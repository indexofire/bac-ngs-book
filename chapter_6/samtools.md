## samtools

#### 安装samtools

```
~/tmp$ sudo apt-get install libncurses-dev
~/tmp$ wget http://jaist.dl.sourceforge.net/project/samtools/samtools/1.1/samtools-1.1.tar.bz2
~/tmp$ tar jxvf samtools-1.1.tar.bz2 -C ~/app
~/tmp$ cd ~/apps/samtools-1.1
~/app$ make
~/app$ sudo ln -s `pwd`/samtools /usr/local/sbin
```


```
# install required
~/tmp$ sudo apt-get install libncurses-dev
~/tmp$ wget https://github.com/samtools/samtools/releases/download/1.3/samtools-1.3.tar.bz2
~/tmp$ tar xjf samtools-1.3.tar.bz2 -C ~/app
~/tmp$ cd ~/app/samtools-1.3
~/app/samtools-1.3$ make
~/app/samtools-1.3$ sudo cp samtools /usr/local/sbin
```