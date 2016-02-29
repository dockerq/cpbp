# dockerfile personal practice
## 使用合适的软件源
ubuntu我一般使用[aliyun的源](http://wiki.ubuntu.org.cn/Template:14.04source)
```
deb http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
```

## 如何安装一些常用软件
### supervisor
- ubuntu
```shell
apt-get update
apt-get install supervisor
```

- python setuptools
```
apt-get install python-setuptools
easy_install supervisor
```

### mesos
- 使用mesosphere维护的软件包
```
echo “hello world”
```

##下载指定版本docker
- [docker forum](https://forums.docker.com/t/how-can-i-install-a-specific-version-of-the-docker-engine/1993/6)

## openjdk
截至2016.2.18,ubuntu 14.04不支持apt安装openjdk8,如果想在ubuntu上安装openjdk8,可以使用14.10以上的版本镜像
```
apt-get install openjdk-7-jre
```

- [ask ubuntu](http://askubuntu.com/questions/464755/how-to-install-openjdk-8-on-14-04-lts)
- [openjdk official method](http://openjdk.java.net/install/)


## 如何让镜像变得更小
### ubuntu
1. 更新完软件后记得清除缓存
```
sudo apt-get autoclean（已卸载软件的安装包）
sudo apt-get clean（未卸载软件的安装包）类似于 rm -rf /var/cache/apt/archives/*
sudo apt-get autoremove （清理系统不再需要的孤立的软件包）
```

## 使用管道减少镜像大小
有些软件无法通过包管理工具直接下载，比如`Oracle Java`;或者版本不符合，比如需要安装scala2.10,但是截至**2016.2.29**，ubuntu14.04只有2.9.2，这时我们就需要自己从官网上下载安装。

如果分两步：
1. 下载压缩包
2. 解压

这样会占用两个镜像层，而如果使用管道下载并解压，这时只占用一个镜像层。

举例：

- 安装`zookeeper`
```
RUN apt-get install -y curl && \
    curl -fL http://apache.fayea.com/zookeeper/zookeeper-3.3.6/zookeeper-3.3.6.tar.gz | tar xzf - -C /usr/local && \
    mv /usr/local/zookeeper-3.3.6 /usr/local/zookeeper
```
[dockerfile地址](https://github.com/DHOPL/docker-zookeeper/blob/master/Dockerfile)

- 安装kafka
```
wget DOWNLOAD_URL | tar zxvf - -C /usr/local/
```
[dockerfile地址](https://github.com/DHOPL/docker-kafka)
