@Link[官方]：https://docs.docker.com/engine/install/centos/

Docker 要求 CentOS 系统的内核版本高于 3.10 ，首先验证你的服务器是否支持Docker。

通过 **uname -r** 命令查看当前的内核版本

![image-20220403143746212](https://image-zico-markdown.oss-cn-hangzhou.aliyuncs.com/img/image-20220403143746212.png)

## 1.卸载旧版本

首先，这部并不是必须的。如果是新的服务器，那么应该是不存在老版本的docker的。不过为了保证后续的安装，请再执行一次：

```
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

出现下面表示没有依赖项存在。（为了保障docker的顺利安装还是执行下吧！）

```
No Packages marked for removal
```

## 2.下载Docker依赖的工具

```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

