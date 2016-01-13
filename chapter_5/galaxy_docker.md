# Galaxy快速部署

## 小型实验室快速解决方案

使用 Amazon EC2 对于不熟悉 Linux 和服务器配置的用户而言是很方便，节省用户在硬件的经济投入，配置学习以及维护能力的培养。但对于测序数据不适合上传到第三方服务器的实验室或要结合本地 LIMS 系统的实验室，要快速构建 Galaxy 以及相配套的其他软件，目前来说最简便的方法之一是利用 docker 部署到本地服务器上。

### Docker

#### 安装 Docker

Ubuntu 14.04 已经自带了 docker 安装包，不过版本相对较老，想使用 docker 最新版的功能，添加源来安装 docker:

```bash
# 添加 docker 源
~$ sudo apt-get install apt-transport-https
~$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9
~$ sudo bash -c "echo deb https://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list"

# 安装 docker
~$ sudo apt-get update
~$ sudo apt-get install lxc-docker

# 安装完成后启动 docker 服务
~$ sudo service docker start
```

#### Docker入门

首先要对 docker 镜像和容器的概念有所了解。细节可以参阅 [Docker 从入门到实践](https://yeasy.gitbooks.io/docker_practice/content)

```bash
~$ docker run ubuntu:14.04 /bin/echo 'Hello world'
```

#### 安装 galaxy docker 镜像

galaxy 的 docker 镜像可以自己来创建，建议使用`docker galaxy-stable`，源代码可以在 [Github](https://github.com/bgruening/docker-galaxy-stable) 下载。也可以直接到官方 Hun Registry 里下载`galaxy-stable`镜像

```bash
~$ sudo docker pull bgruening/galaxy-stable
```

官方 Registry 非常慢，可以通过国内 daocloud 服务商的加速器来加快`docker pull`过程：首先注册一个 [daocloud](daocloud.io) 用户，然后在命令行中添加 registry mirror。

```bash
# ****** 为daocloud分配给你的ID
~$ echo "DOCKER_OPTS=\"\$DOCKER_OPTS --registry-mirror=http://******.m.daocloud.io\"" | sudo tee -a /etc/default/docker
~$ sudo service docker restart
~$ sudo docker pull bgruening/galaxy-stable
```

该脚本可以将`--registry-mirror`加入到你的Docker配置文件`/etc/default/docker`中。适用于Ubuntu14.04，其他版本根据docker配置文件和环境变量，可能有细微不同。

```bash
~/app$ wget https://github.com/bgruening/docker-galaxy-stable/archive/15.10.tar.gz
~/app$ tar zxf 15.10.tar.gz
~/app$ cd docker-galaxy-stable-15.10
```

#### 启动 galax-stable 容器

`docker run -d`表示以daemon方式运行docker，执行该命令后，docker完成galaxy初始化需要花几分钟时间才能访问。如果是本地运行，用浏览器访问`127.0.0.1:8080`可以看到galaxy首页，如果是服务器上用ssh连接执行命令，要访问服务器IP加上8080端口。

```bash
# 后台运行 galaxy 容器
~$ sudo docker run -d -p 8080:80 -p 8021:21 bgruening/galaxy-stable

# 交互方式运行 galaxy 容器
~$ sudo docker run -i -t -p 8080:80 bgruening/galaxy-stable /bin/bash
root$ sh run.sh
```

有时候需要进入docker容器中进行操作，就可以以`docker run -i`交互模式进行访问。进入容器后运行`sh run.sh`可以DEBUG方式运行galaxy，适合本地测试使用。

#### 复制容器内文件

```bash
# fc3e62e0471d 是想要获取文件的所在容器。foo.txt是想要获得的文件。
~$ sudo docker cp fc3ea62e471d:/home/foo.txt .
```

#### 加载数据卷

需要分析的数据通过添加外部数据卷来实现。

```bash
# 添加服务器上的 /mydata 卷到容器中
~$ sudo docker create -v /mydata --name my_data_vol bgruening/galaxy-stable /bin/bash

# 或者在运行时将本地卷`/mydata`加入到容器中`/container_data`位置
~$ sudo docker run -d -p 8080:80 -v /mydata:/container_data/ bgruening/galaxy-stable
```

镜像是只读的，当`Ctrl+D`方式退出容器后，再次进入容器时你上次以添加的内容是看不到的。如果想要从上一次运行的容易中获得文件可以用`docker cp`的方法，不过你得记住上一次运行的container id号。

#### 删除所有不运行的容器

```bash
~$ sudo docker ps -a | cut -d ' ' -f 1 | sudo xargs docker rm
```

#### 删除镜像

```bash
# IMAGE ID 是该镜像的ID，如果镜像还有容器运行，或有其他镜像的依赖关系，则无法删除要先删除容器或其他镜像。
~$ sudo docker rmi IMAGE_ID
```

## Rreference

1. [Docker 从入门到实践](https://yeasy.gitbooks.io/docker_practice/content)
2. [docker-galaxy-stable](https://github.com/bgruening/docker-galaxy-stable)