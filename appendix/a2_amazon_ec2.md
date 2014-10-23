## Amazon EC2 教程

本地电脑如果是 Windows 系统，建议下载一个 putty 来访问连接。如是 Linux 或 Mac OSX 操作系统，则直接开一终端程序用 ssh 命令访问即可。

** 1. 注册申请**

首先要去 https://aws.amazon.com/cn/ 注册一个帐号。用新注册帐号登录后，可以看到 **Amazon Web Services** 页面，上面列出了Amazon所有的服务内容。点击`Compute & Networking`里的`EC2`，可以进入你的`EC2 Dashboard`。

要注意的一点是，顶部工具栏在用户工具右侧有一个地区选项下拉菜单，可以选择不同区域的服务器。个人使用感受，亚洲的服务器比如东京和新加坡的连接速度最快，但是美国的服务器速度也很不错，比一般的VPS商售卖的要快的多！至于选择哪一个地区，可以根据自己的网络实际情况来选择，但是如果你要使用一些`AWS Public Datasets`，比如`Human genome project`或者`Genbank`之类的数据，需要添加`volume`的`snapshot`，这些`snapshot`往往是限定区域的。比如`Genbank`数据你只能在`us-east`区域使用，也就是说你要用这个`snapshot`，你必须把你的`Instance`建在`US East(N.Virginia)`区域。

** 2. 建立 Instance**
在`EC2 Dashboard`左侧列表里点击`Instances`，然后点击`Launch Instance`，会弹出如下图所示的一步步配置Instance的界面。

`Step 1: Choose an Amazon Machine Image (AMI)`界面里选择*Ubuntu Server 14.04LTS*。

![Instance](../assets/img/appendix_a2_1.png)
figure1. 第一步选择操作系统

`Step 2: Choose an Instance Type`，Type 选择 `t2.micro`
是因为这个配置是1年免费的。缺点当然是计算资源比较低，而且当你的计算资源超限额后，会自动把 CPU 性能降低到大概 1/10 左右的情况。要注意的是虽然是免费的，但本质上是1个月720hour的免费计算。如果你同时开了多个 t2.micro Instances，没有在不计算的时候停机stop，仍旧会超出计算时间，被扣美金的。所以如果开了多个Instances，千万别忘了不用的时候关闭。

然后就可以点击`review and launch`。如果想再定制其他配置可以点击`Next: Configure Instance Details`。当需要自动加载 `docker`，或者添加数据`Volumes`的时候要到后面的步骤里设置。

![Instance](../assets/img/appendix_a2_2.png)
figure2. 第二步选择配置类型

`Step 7: Review Instance Launch`，确认配置无误就点击 Launch，会弹出一个`Select an existing key pair or create a new key pair`选项窗口。这一步很重要，第一次建立新的 Instance 的话，下拉菜单选择`Create a new key pair`。然后给 key pair 取个好记的名字，比如**bac_ngs**，然后点击`Download Key Pair`，就会下载这个密钥文件到你的电脑上。名字叫**bac_ngs.pem**。这时候底部的`Launch Instance`按钮就变成可以点击状态，把你的鼠标按上去，你的Instance就开始初始化了。

** 3. 访问 Instance**
如果你的本地电脑操作系统是Linux，可以直接开一个终端程序运行ssh来访问Instance，如果是Windows电脑，推荐使用putty来访问。

**3.1 Linux**

**3.2 Windows**
手头上没有Windows机器，可以参考这个[教程](http://ged.msu.edu/angus/tutorials-2013/log-in-with-ssh-win.html)。

#### Running docker in Amazon EC2

docker 是一种虚拟环境，可以很方便的在不同机器上配置环境，近年来快速发展，在云计算服务的领域被高度关注。事实上它的启动速度很快，配置也特别方便，很适合生物信息计算的自动化配置。

你除了可以在Instance里安装docker来实现外，AWS也提供默认的接入方式。

