#dockerfile personal practice
##使用合适的软件源
ubuntu我一般使用[aliyun的源](http://wiki.ubuntu.org.cn/Template:14.04source)
```
deb http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
```

##如何安装一些常用软件
###supervisor
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

###mesos
- 使用mesosphere维护的软件包
```

```

##下载指定版本docker
- [docker forum](https://forums.docker.com/t/how-can-i-install-a-specific-version-of-the-docker-engine/1993/6)

##如何让镜像变得更小
###ubuntu
1. 更新完软件后记得清除缓存
```
sudo apt-get autoclean（已卸载软件的安装包）
sudo apt-get clean（未卸载软件的安装包）类似于 rm -rf /var/cache/apt/archives/*
sudo apt-get autoremove （清理系统不再需要的孤立的软件包）
```
