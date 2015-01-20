## 1. Galaxy 本地安装与配置

#### 下载与安装

galaxy 的代码库托管在 bitbucket.org ，需要先安装 mercurial ，然后用 hg 工具将 galaxy 代码库克隆到本地。

```
~app$ sudo apt-get install mercurial
~app$ hg clone https://bitbucket.org/galaxy/galaxy-dist/
~app$ cd galaxy-dist
```

可以用 `hg branch` 命令查看代码分支是否为 stable，如果是其他分支（galaxy代码库有另一分支'default'），建议切换到 stable 分支：`hg update stable`。建议用 stable 分支的代码在生产环境下使用。

#### 配置与运行


```
~app$ ./run.sh
```

#### 生产环境配置

作为个人使用，前面的步骤在PC机上已经可以正常运行，但如果要作为生产环境下多用户使用，建议使用专门的代理服务器和数据库来增强效率和速度。

这里采用nginx+postgresql构建生产环境下的 Galaxy 服务。首先在ubuntu下新建一个用户galaxy。

```
~$ sudo adduser galaxy
```

按照提示，在弹出提示符输入相应内容，主要填好密码即可，其他可以留空。然后切换到galaxy用户：

```
~$ su galaxy
~$ cd
```

重复`Galaxy 本地安装与配置`的第一步`下载与安装`步骤,建立配置文件：
```
~$ cp config/galaxy.ini.sample config/galaxy.ini
~$ vim config/galaxy.ini
```

将配置文件galaxy.ini设置如下

```

```

## 2. Galaxy 运行在 Amazon EC2

## 3. 添加第三方软件




#### Reference

1. http://www.chenlianfu.com/?p=1469
2. http://methdb.univ-perp.fr/cgrunau/methods/Galaxy_production_mode_installation_walkthrough.html
