## Local Blast

拿到测序完成的草图后，因为基因组数据较大，连接NCBI网站往往又非常缓慢，所以一半要做 Blast 比对的话都需要做 Local Blast。这里介绍2个方式来实现：

1. 用命令行进行 Blast
2. 快速构建 Web Blast 服务


One thing everyone wants to do is BLAST sequence data, right?  Here's a
simple way to set up a stylish little BLAST server that lets you search
your newly assembled sequences.



#### 2.1 安装 blastkit

安装依赖包

```
~$ sudo pip install pygr
~$ sudo pip install whoosh
~$ sudo pip install git+https://github.com/ctb/pygr-draw.git
~$ sudo pip install git+https://github.com/ged-lab/screed.git
~$ sudo apt-get -y install lighttpd
```

对lighttpd webserver进行配置

```
~$ cd /etc/lighttpd/conf-enabled
~$ sudo ln -fs ../conf-available/10-cgi.conf ./
~$ sudo echo 'cgi.assign = ( ".cgi" => "" )' >> 10-cgi.conf
~$ sudo echo 'index-file.names += ( "index.cgi" ) ' >> 10-cgi.conf
~$ sudo /etc/init.d/lighttpd restart
```

本地安装 Blast

```
~$ cd tmp
~/tmp$ curl -O ftp://ftp.ncbi.nih.gov/blast/executables/release/2.2.24/blast-2.2.24-x64-linux.tar.gz
~/tmp$ tar xzf blast-2.2.24-x64-linux.tar.gz -C ~/app
~/tmp$ sudo cp ~/app/blast-2.2.24/bin/* /usr/local/sbin
~/tmp$ sudo cp -r ~/app/blast-2.2.24/data /usr/local/blast-data
```

安装 blastkit

```
~/tmp$ cd ~/app
~/app$ git clone https://github.com/ctb/blastkit.git -b ec2
~/app$ cd blastkit/www
~/app/blastkit/www$ sudo ln -fs $PWD /var/www/blastkit
~/app/blastkit/www$ mkdir files
~/app/blastkit/www$ chmod a+rxwt files
~/app/blastkit/www$ chmod +x /root
~/app/blastkit/www$ cd ..
~/app/blastkit$ python ./check.py
```

如果安装顺利，就会提示一切已经准备完毕。接下来要准备数据。

```
~$ cp /mnt/assembly/ecoli.21/contigs.fa ~/app/blastkit/db/db.fa
~$ cd ~/app/blastkit
~$ formatdb -i db/db.fa -o T -p F
~$ python index-db.py db/db.fa
```

## Reference

* [Blastkit](https://github.com/ctb/blastkit.git)
* [Caltech workshop](https://github.com/dib-lab/2013-caltech-workshop/blob/master/blastkit.txt)