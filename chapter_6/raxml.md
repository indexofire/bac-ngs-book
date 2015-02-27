## RaxML

#### 下载并安装

```
~/tmp$ wget https://github.com/stamatak/standard-RAxML/archive/v8.1.17.tar.gz
~/tmp$ tar zxvf v8.1.17.tar.gz -C ~/app/
~/tmp$ cd ~/app/standard-RAxML-8.1.17
~/app$ make -f Makefile.gcc
~/app$ echo 'export PATH="$PATH:$HOME/app/standard-RAxML-8.1.17"' > ~/.bashrc
~/app$ source ~/.bashrc
```

#### 软件使用

```
~$ raxmlHPC -f a -x 12345 -p 12345 -# 100 -m GTRGAMMA -s align.phy -n .ex -T 4
```
