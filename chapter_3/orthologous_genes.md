## Orthologues Identification

对于orthologs来说，因为其源自共同祖先，一般而言功能是相同的，因此对于新测序物种的基因组，其基因的功能可以通过 ortholog identification 来了解。

### 1. Orthomcl

MCL算法是 Markov Cluster Algorithm 算法的缩写。

#### 1.1 安装和配置 MySQL

Ubuntu 14.04 安装 MySQL

```
~$ sudo apt-get install mysql-server-5.6
```

安装过程中会进行配置提示：

```
# 输入 MySQL 管理员用户密码
New password for the MySQL "root" user:
Repear password for the MySQL "root" user:
```

安装完后，可以测试 MySQL 是否正常运行：

```
~$ sudo netstat -tap | grep mysql
```

如果能看到`tcp 0 0 localhost:mysql *:* LISTEN port/mysqld `这样的字符输出，表示安装成功。接下来用`mysql`命令建立orthomcl用户和数据库:

```
~$ mysql -u root -p
```

然后在 `Enter password` 命令行提示符号下输入前面设置的密码，就可以看到 `mysql>` 连接界面了。输入`select version();`会打印运行版本，输入`quit;`或者`exit`退出连接界面。

```
mysql> select version();
+-------------------------+
| version()               |
+-------------------------+
| 5.6.19-0ubuntu0.14.04.1 |
+-------------------------+
1 row in set (0.00 sec)
```

在 mysql 终端建立 orthomcl用户与数据库：

```
# 建立数据库orthomcl的管理用户， 其中'123456' 是用户密码，可以设置成实际需要的
mysql> create user 'orthomcl'@localhost' identified by '123456';
Query OK, 0 rows affected (0.00 sec)

# 创建数据库orthomcl用来储序列数据
mysql > create database orthomcl;
Query OK, 1 rows affected (0.00 sec)

# 分配权限
mysql > grant all on orthomcl.* to 'orthomcl'@'localhost'
Query OK, 0 rows affected (0.00 sec)

# 查看所有的数据库
mysql > show databases;
+---------------------+
| Databases           |
+---------------------+
| orthomcl            |
| ...                 |
+---------------------+
```

#### 1.2 安装 mcl

从源代码安装 mcl

```
~/tmp$ wget http://www.micans.org/mcl/src/mcl-latest.tar.gz
~/tmp$ tar zxvf mcl-latest.tar.gz -C ~/apps
~/tmp$ mv ~/apps/mcl-14-137 ~/apps/mcl && cd ~/apps/mcl
~/apps$ ./configure
~/apps$ make
~/apps$ sudo make install
```

或者直接安装 ubuntu 编译包，不过 ubuntu 14.04里的编译包不是最新版本的。只有在14.10里才是最新的。

```
~$ sudo apt-get install mcl
```

#### 1.3 安装 OrthoMCL

```
~/tmp$ wget http://orthomcl.org/common/downloads/software/v2.0/orthomclSoftware-v2.0.9.tar.gz
~/tmp$ tar zxvf orthomclSoftware-v2.0.9.tar.gz -C ~/apps
~/tmp$ mv ~/apps orthomclSoftware-v2.0.9 orthomc
```

配置 MySQL 数据库连接

```
~/tmp$ cd ~/apps orthomcl
~/apps$ cp doc/OrthoMCLEngine/Main/orthomcl.config.template ./orthomcl.config
~/apps$ sudo `pwd`/bin/* /usr/local/sbin/
~/apps$ nano orthomcl.config
```

安装perl依赖库，由于bioperl用cpanm貌似装不上，就直接用预编译包安装。

```
~$ sudo apt-get install bioperl
~$ sudo apt-get install cpanminus
~$ sudo cpanm DBD::mysql DBI Parallel::ForkManager YAML::Tiny Set::Scalar Text::Table Exception::Class Test::Most Test::Warn Test::Exception Test::Deep Moose SVG Algorithm::Combinatorics
~$ cd ~/repos
~/repos$ git clone https://github.com/apetkau/orthomcl-pipeline.git
~/repos$ orthomcl-setup-database.pl --user orthomcl --password password --host localhost --database orthomcl > orthomcl.conf
```

参考下面内容配置 orthomcl.config 文件：

```
# this config assumes a mysql database named 'orthomcl'.  adjust according
# to your situation.
dbVendor=mysql
dbConnectString=dbi:mysql:orthomcl:localhost:mysql_local_infile=1
# for the error of 'the used command is not allowed with this MYSQL version' when importing blast result to mysql, change dbConnectString like this
# dbConnectString=dbi:mysql:orthomcl:mysql_local_infile=1:localhost:3307
# and add local-infile=1 and loose-local-infile=1 1/2 in /etc/mysql/my.cnf
dbLogin=orthomcl
dbPassword=123456
similarSequencesTable=SimilarSequences
orthologTable=Ortholog
inParalogTable=InParalog
coOrthologTablInCoOrtholog
interTaxonMatchView=InterTaxonMatch
percentMatchCutoff=50
evalueExponentCutoff=-5
oracleIndexTblSpc=NONE
```

添加 orthomcl 的 perl 模块到系统 perl 路径

```
~/apps$ echo "export PERL5LIB=$PERL5LIB:~/apps/orthomcl/lib/perl" > ~/.bashrc
~/apps$ source ~/.bashrc
```

用 orthomclInstallSchema 脚本

```
~/apps$
~/apps$ orthomclInstallSchema orthomcl.config install.log
```

#### 1.4 安装 NCBI Blast+

```
~$ sudo apt-get install ncbi-blast+
```

#### 1.5 用 orthomcl-pipeline 来简化操作

---

### 2. Proteinortho

---

### 3. get_homologues

#### 3.1 安装 get_homologues

```
~$ 
```


#### Reference


