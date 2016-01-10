## Fastx_toolkit

`fastx_toolkit` 提供了一套 fasta/q 数据转换与处理的方式，其中最常用的几个工具如 `fastx_clipper`, `fastx_trimmer` 等，灵活运用可以为我们数据前处理提供方便。

** fastx_toolkit 安装**
```
~/tmp$ wget http://cancan.cshl.edu/labmembers/gordon/files/libgtextutils-0.7.tar.bz2
~/tmp$ wget http://cancan.cshl.edu/labmembers/gordon/files/fastx_toolkit-0.0.14.tar.bz2
~/tmp$ tar zxvf libgtextutils-0.7.tar.bz2
~/tmp$ cd libgtextutils-0.7
~/tmp$ ./configure
~/tmp$ make
~/tmp$ sudo make install
~/tmp$ cd ..
~/tmp$ tar xjf fastx_toolkit-0.0.14.tar.bz2 -C ~/app
~/tmp$ cd ~/app/fastx_toolkit-0.0.14
~/app$ ./configure
~/app$ make
~/app$ sudo make install
~/app$ sudo ldconfig
```
