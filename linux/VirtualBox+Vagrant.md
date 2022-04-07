# VirtualBox+Vagrant快速搭建Linux系统

说到 Windows 系统上的虚拟机，**VMware**顶顶大名，相信很多人都玩过。今天就介绍另外的一种玩法：**VirtualBox + Vagrant**，通过几个简单的命令就能实现Linux的快速搭建。

## 1.VirtualBox与Vagrant

**注意：通过VirtualBox开启虚拟机的化，电脑本身的虚拟化技术需要打开。这里不做赘述**

### VirtualBox

**VirtualBox** 其实和**VMware**差不多，作为一个虚拟机的载体存在的这么一款软件。除了可以通过本文的：**Vagrant**搭配通过命令的形式创建虚拟机之外，还可以和**VMware**一样，通过本地的ISO镜像文件进行创建。

VirtualBox下载地址：

> https://www.virtualbox.org/wiki/Downloads  【选择windows host】

安装：除了安装文件夹之外其他的一路 下一步就行。

### Vagrant

**Vagrant**是一款用于构建及配置虚拟开发环境的软件，基于Ruby,主要以命令行的方式运行。

主要使用Oracle的开源VirtualBox虚拟化系统，与Chef，Salt，Puppet等环境配置管理软件搭配使用， 可以实行快速虚拟开发环境的构建。

早期以VirtualBox为对象，1.1以后的版本中开始对应VMware等虚拟化软件，包括Amazon EC2之类服务器环境的对应。

Vagrant下载地址：

> https://www.vagrantup.com/downloads  【选择 64的】

安装：同上。

验证一下，打开PowerShell（或者cmd）直接输入：Vagrant

<img src="https://image-zico-markdown.oss-cn-hangzhou.aliyuncs.com/img/image-20220403151703218.png" alt="image-20220403151703218" style="zoom:50%;" />

出现这样，那就是安装成功了。

## 2.快速搭建Linux服务器

### （1）选择镜像

首先去**Vagrant**的镜像仓库，选择一个Linux镜像【地址：@Link：https://app.vagrantup.com/boxes/search】

<img src="https://image-zico-markdown.oss-cn-hangzhou.aliyuncs.com/img/image-20220403152032364.png" alt="image-20220403152032364" style="zoom:50%;" />

本次安装centos，选择centos镜像，然后打开PowerShell（或者cmd）窗口，输入初始化命令：

```
vagrant init centos/7 【注意名字】
```

完成之后会在你的C盘/用户目录下面创建一个文件：

<img src="https://image-zico-markdown.oss-cn-hangzhou.aliyuncs.com/img/image-20220403152544177.png" alt="image-20220403152544177" style="zoom:50%;" />

### （2）启动环境

打开**VirtualBox**，同时打开PowerShell（或者cmd）窗口，输入：

```
vagrant up //这个就是启动一个centos虚拟镜像 不过他会先从远端拉下来，再启动。一般都会很慢
```

当**VirtualBox**出现 一个虚拟机就证明启动成功了：
<img src="https://image-zico-markdown.oss-cn-hangzhou.aliyuncs.com/img/image-20220403153029530.png" alt="image-20220403153029530" style="zoom:50%;" />

### （3）连接虚拟机

同样开启一个PowerShell（或者cmd）窗口，输入：

```
vagrant ssh //连接虚拟机
exit  //退出虚拟机
```

连接成功后，就可以在window下，通过PowerShell（或者cmd）窗口，直接操作虚拟机的命令了。

<img src="https://image-zico-markdown.oss-cn-hangzhou.aliyuncs.com/img/image-20220403153333336.png" alt="image-20220403153333336" style="zoom:50%;" />

注意：默认使用的是：**vagrant**账号，并不是root

### （4）其他命令

```
vagrant up //启动虚拟机
vagrant reload //重启
```

## 3.网络设置

一般来说，当我们创建好一个虚拟机之后，并且在虚拟机中安装了软件：MySQL，redis等等。需要通过**VMware**/**VirtualBox**，进行设置一个端口转发。

<img src="https://image-zico-markdown.oss-cn-hangzhou.aliyuncs.com/img/image-20220403154233369.png" alt="image-20220403154233369" style="zoom:50%;" />

<img src="https://image-zico-markdown.oss-cn-hangzhou.aliyuncs.com/img/image-20220403154334684.png" alt="image-20220403154334684" style="zoom:50%;" />

相当于每安装一个软件，就要进行一个端口转发，非常的麻烦。这里提供两个方法：

（1）在虚拟机中改网卡，然后保证win和虚拟机之前网络进行通讯。

（2）前面提到过，**vagrant**是通过**Vagrantfile**文件进行创建 虚拟机的，所以我们只需要在这个文件中进行一下配置，就可以保证互通了。

这里我们采用第二种

### Vagrantfile

打开该文件，找到 **"config.vm.network "private_network", ip: "192.168.33.10""**所在，放开注释：

<img src="https://image-zico-markdown.oss-cn-hangzhou.aliyuncs.com/img/image-20220403155037891.png" alt="image-20220403155037891" style="zoom:50%;" />

其次打开cmd窗口，输入：ipconfig

<img src="https://image-zico-markdown.oss-cn-hangzhou.aliyuncs.com/img/image-20220403155157472.png" alt="image-20220403155157472" style="zoom:50%;" />

重点查看**VirtualBox**的ip地址，比如：**192.168.56.1**，然后将该ip地址复制到文件中去：

![image-20220403155447907](https://image-zico-markdown.oss-cn-hangzhou.aliyuncs.com/img/image-20220403155447907.png)

然后保存，重启虚拟机。启动完成之后，查看虚拟机的ip地址：**ip addr**

![image-20220403155627905](https://image-zico-markdown.oss-cn-hangzhou.aliyuncs.com/img/image-20220403155627905.png)

可以看到这个地址就是刚才配置文件的地址，然后进行相互ping一下，验证是否相同。

![image-20220403155844123](https://image-zico-markdown.oss-cn-hangzhou.aliyuncs.com/img/image-20220403155844123.png)

🆗，完全没问题。

## 4.总结

到此呢，这个安装就完成了。其实也很简单，快速总结一下：

- 下载**VirtualBox与Vagrant**，并安装。
- 去**Vagrant**镜像仓库，挑选自己需要的镜像。
- 在cmd窗口进行初始化。**vagrant init 镜像名字【比如：vagrant init centos/7】**
- 启动并连接镜像。**（1）vagrant up【启动】（2）vagrant ssh【连接虚拟机】**
- 设置网络。在配置文件中，找到**config.vm.network "private_network**并放开注释，然后在cmd查看**VirtualBox**的ip地址，并将配置文件的ip地址进行替换。























































