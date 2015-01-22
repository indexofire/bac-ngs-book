## 构建单机产环境

作为个人尝试，前面的步骤在PC机上已经可以正常运行使用。对于要作为生产环境下多用户使用，建议使用专门的代理服务器和数据库来增强效率和速度。这里采用nginx+postgresql构建生产环境下的 Galaxy 服务。这里只是简单的介绍一下最基本的配置方式，对于高负载的web server设置又是另一个很复杂的话题了，这里就不具体展开。也可以参考别人做的[galaxy dockerfile](https://registry.hub.docker.com/u/bgruening/galaxy-stable/dockerfile/)。

首先在ubuntu下新建一个用户galaxy。

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

将配置文件galaxy.ini设置如下，你也可以将内容直接保存成galaxy.ini

```
[server:main]
use = egg:Paste#http
host = 0.0.0.0
use_threadpool = True
threadpool_kill_thread_limit = 10800

[filter:gzip]
use = egg:Paste#gzip

[filter:proxy-prefix]
use = egg:PasteDeploy#prefix
prefix = /galaxy

[app:main]
paste.app_factory = galaxy.web.buildapp:app_factory
database_connection = postgresql://galaxy:galaxy@localhost:5432/galaxyserver
tool_dependency_dir = tool_dep
use_nglims = False
nglims_config_file = tool-data/nglims.yaml
debug = False
use_interactive = False
admin_users = admin@localhost.com
```

#### 安装 postgresql

安装postgresql并建立galaxy数据库，这里用户名和密码都设置为`galaxy`，要与 galaxy.ini 中`database_connection`参数对应的值一致。

```
~$ sudo apt-get install postgresql-9.3
~$ su - postgres
~$ psql template1

> CREATE USER galaxy WITH PASSWORD 'galaxy';
> CREATE DATABASE galaxyserver;
> GRANT ALL PRIVILEGES ON DATABASE galaxyserver to galaxy;
> \q
```

#### 安装 nginx

用nginx做反向代理，处理请求。

```
~$ sudo apt-get install nginx
```

设置nginx.conf

```
http {
    upstream galaxy_app {
        server localhost:8080;
    }

    server {
        client_max_body_size 10G;
        location / {
            proxy_pass   http://galaxy_app;
            proxy_set_header   X-Forwarded-Host $host;
            proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
        }
    }
}
```
