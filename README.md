# Container personal best practice

## Dockerfile
### delete extra softs each image layer
- for ubuntu

```
...
RUN apt-get update && apt-get install -y --no-install-recommends libjemalloc1 && rm -rf /var/lib/apt/lists/*

RUN apt-get update \
	&& apt-get install -y cassandra="$CASSANDRA_VERSION" \
	&& rm -rf /var/lib/apt/lists/*
...
```

- for alpine

```
...
RUN apk add --update --no-cache \
    python \
    python-dev \
    py-pip \
    build-base \
  && pip install virtualenv \
  && rm -rf /var/cache/apk/*
...
```

[source of cassandra:2.2](https://github.com/docker-library/cassandra/blob/master/2.2/Dockerfile#L43-L45)

### useful template
```
FROM
MAINTAINER

ENV FRESHED_AT 2017-01-17
# set timezone if possible
RUN ln -f -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

```

### use proper software source if under GFW
for example,use [aliyun source](http://wiki.ubuntu.org.cn/Template:14.04source) for ubuntu
```
deb http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
```

## Install common softwares
### supervisor
- ubuntu
```shell
apt-get update
apt-get install -yqq --no-install-recommends supervisor
```

- python setuptools
```
apt-get install python-setuptools
easy_install supervisor
```

### mesos
```
RUN echo "deb http://repos.mesosphere.io/ubuntu/ trusty main" > /etc/apt/sources.list.d/mesosphere.list && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv E56151BF
```

### docker
- [docker forum](https://forums.docker.com/t/how-can-i-install-a-specific-version-of-the-docker-engine/1993/6)

### openjdk
by the time `2016.2.18`, ubuntu 14.04 does not support to install `openjdk8` by apt-get,use 14.10+ instead.
```
apt-get install openjdk-7-jre
```

reference：
- [ask ubuntu](http://askubuntu.com/questions/464755/how-to-install-openjdk-8-on-14-04-lts)
- [openjdk official method](http://openjdk.java.net/install/)
- [set JAVA_HOME for openjdk](reference http://askubuntu.com/questions/175514/how-to-set-java-home-for-java)

## make image small and light
### use alpine as far as possible

### ubuntu
1. remove cache after update or installing softwares
```
sudo apt-get autoclean（已卸载软件的安装包）
sudo apt-get clean（未卸载软件的安装包）类似于 rm -rf /var/cache/apt/archives/*
sudo apt-get autoremove （清理系统不再需要的孤立的软件包）
```

### --no-install-recommends
```
apt-get install -y --no-install-recommends vim
```

### use pipe
```
RUN apt-get install -y curl && \
    curl -fL http://apache.fayea.com/zookeeper/zookeeper-3.3.6/zookeeper-3.3.6.tar.gz | tar xzf - -C /usr/local && \
    mv /usr/local/zookeeper-3.3.6 /usr/local/zookeeper
```
[dockerfile](https://github.com/dockerq/docker-zookeeper/blob/master/Dockerfile#L10-L13)
