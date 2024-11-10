---
title: Ubuntu基础配置(18.04)+Docker配置和使用+容器化centos7大数据环境准备
date: 2024-11-10 22:48:05
tags: 
  - Bigdata
  - Docker
categories: 
  - 大数据运维系列
---
## 1.ubuntu 基础配置

环境：`ubuntu 18.04`

安装完Ubuntu之后，除了需要新建用户、设置密码之外，我们还要设置root密码，虽然Ubuntu默认有root超级管理员账户，但是具体的密码我们可以自行设置

### 1.1设置root密码：

1.启动Ubuntu
启动Ubuntu，有图形界面的，启动终端即可

2.终端输入sudo passwd root

```shell
sudo passwd root
```

![image-20230320172400074](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202309241324207.png)

验证测试：

验证：输入**su -** 后输入超级管理员账户的密码

### 1.2 更换源：

更换下载源，不更换的话安装docker时会很慢，可以选择阿里或者清华源

``` shell
sudo apt install vim 
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak		备份
sudo vi /etc/apt/sources.list    #添加清华源
```

清华源：

[ubuntu | 镜像站使用帮助 | 清华大学开源软件镜像站 | Tsinghua Open Source Mirror](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)

```shell
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse

# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
# # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse

deb http://security.ubuntu.com/ubuntu/ bionic-security main restricted universe multiverse
# deb-src http://security.ubuntu.com/ubuntu/ bionic-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
# # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
```

```shell
#更新源
sudo apt update
```

### 1.3 安装ssh服务：

（方便连接xshell工具）

```shell
sudo apt install openssh-server -y
```

解决windows Ubuntu 之间复制粘贴问题：

```shell
sudo apt-get autoremove open-vm-tools -y	//卸载已有的工具
sudo apt-get install open-vm-tools -y		//安装工具open-vm-tools
sudo apt-get install open-vm-tools-desktop -y //安装open-vm-tools-desktop
```

ubuntu下安装asbru-cm工具：

Asbru-CM是一种开源的配置管理工具，用于管理和部署服务器配置。它提供了一个Web界面，使用户可以轻松地管理和监控多台服务器的配置。

```shell
sudo apt install curl -y

curl -s https://packagecloud.io/install/repositories/asbru-cm/asbru-cm/script.deb.sh | sudo bash

sudo apt install asbru-cm -y
```



## 2. Docker配置

因为某些原因,国内已经访问不了docker官网,所以此处改成使用阿里源安装docker

[Ubuntu18.04使用阿里源镜像安装Docker并配置镜像加速【图文详细】_阿里的ubuntu镜像源安装-CSDN博客](https://blog.csdn.net/single_0910/article/details/120562065)

### 2.1 如果之前安装过docker，卸载旧版本docker

```shell
sudo apt-get remove docker docker-engine docker.io containerd runc
```

### 2.2 更新及安装工具软件

#### 2.2.1 更新系统里的所有的能更新的软件

```shell
sudo apt-get update
```

#### 2.2.2 安装几个工具软件

```shell
sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release -y
```

#### 2.2.3 增加一个docker的官方GPG key：

gpgkey：是用来验证软件的真伪 ——防伪的

```shell
sudo curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

2.2.4 下载仓库文件

```shell
sudo echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 2.3 安装docker

#### 2.3.1 再次更新系统

```shell
sudo apt-get update
```

#### 2.3.2 安装docker-ce软件

```shell
sudo apt-get install docker-ce docker-ce-cli containerd.io -y
```

### 2.4 查看是否启动docker

由于docker安装的时候自带设置启动，所以直接查看进程是否启动就可以了

```shell
ps aux|grep docker
```

![image-20230812180124831](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202309241324877.png)

安装成功

## 3. Docker 服务操作

### 3.1 常用命令

```shell
sudo service docker start  # 启动Docker
sudo service docker stop  # 停止Docker
sudo service docker restart  # 重启Docker
sudo service docker status  # 查看Docker状态
```

```shell
#查看docker容器
docker ps    #查看当前运行的容器
docker ps -a    #查看所有容器
docker rm -f $(docker ps -aq)  #删除所有容器
docker images  #查看docker镜像
```

```shell
#导入镜像包,两者都会恢复为镜像
docker load < /home/hj/大数据比赛环境包/bigdata.tar
docker load -i < /home/hj/大数据比赛环境包/bigdata.tar
#导入容器包,两者都会恢复为镜像
docker import 路径

#删除镜像（镜像有创建过容器则需要删除容器才可以删除镜像）
docker rmi 镜像名或镜像ID

#删除所有容器
docker rm -f $(docker ps -aq)
docker rm 容器

#启动容器
docker start 容器id

#运行容器
docker exec -it 容器id /bin/bash

#将容器导成镜像
sudo docker commit -a "镜像作者" -m "提交成镜像的说明信息" 容器的名称 新镜像名称:标签

docker 上传文件到容器
docker cp /home/hj/clickhouse-21.9.4.35（tgz） master:/opt/

从容器中上传文件到本地
docker cp master:/opt/ /home/hj
```

### 3.2 Docker 的镜像操作

#### 3.2.1 列出镜像

```shell
docker images
```

结果：![image-20230812180956921](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202309241324626.png)

| 参数       | 含义                                 |
| ---------- | ------------------------------------ |
| REPOSITORY | 镜像所在的仓库名称                   |
| TAG        | 镜像标签(版本)                       |
| IMAGEID    | 镜像ID                               |
| CREATED    | 镜像的创建日期(不是获取该镜像的日期) |
| SIZE       | 镜像大小                             |

#### 3.2.2 搜索镜像

```shell
docker search 镜像名
```

#### 3.2.3 拉取镜像

要想获取某个镜像，我们可以使用pull命令，从仓库中拉取镜像到本地。

如果下载镜像时不指定标签，则默认会下载仓库中最新版本的镜像，即选择标签为 latest 标签。

```shell
docker pull 仓库名称/标签
```

#### 3.2.4 删除镜像

镜像有创建过容器则需要删除容器才可以删除镜像（或者直接强制删除）

```shell
#普通删除
docker rmi 镜像名或镜像ID
#强制删除
docker rmi -f 镜像名或镜像ID
```

#### 3.2.5 导入镜像

```shell
#方法1
docker load < 镜像包
#方法2
sudo docker load -i 镜像包
```

#### 3.2.6 导出镜像

```shell
docker save –o /opt/导出名称.tar 镜像名称:标签
```

### 3.3 Docker容器操作

#### 3.3.1 创建容器

```shell
docker run [option] 镜像名:tag [向启动容器中传入的命令]
```

 常用可选参数说明：

| 参数     | 意义                                                         |
| -------- | ------------------------------------------------------------ |
| -i       | 表示以“交互模式”运行容器                                     |
| -t       | 表示容器启动后会进入其命令行。加入这两个参数后，容器创建就能登录进去。即分配一个伪终端。 |
| –name    | 为创建的容器命名                                             |
| -v       | 表示目录映射关系(前者是宿主机目录，后者是映射到宿主机上的目录，即 宿主机目录:容器中目录)，可以使用多个-v 做多个目录或文件映射。注意:最好做目录映射，在宿主机上做修改，然后 共享到容器上。 |
| -d       | 在run后面加上-d参数,则会创建一个守护式容器在后台运行(这样创建容器后不 会自动登录容器，如果只加-i -t 两个参数，创建后就会自动进去容器)。 |
| -p       | 表示端口映射，前者是宿主机端口，后者是容器内的映射端口。可以使用多个-p 做多个端口映射 |
| -e       | 为容器设置环境变量                                           |
| –network | 指定网桥                                                     |

#### 3.3.2 进入已运行容器

```shell
docker exec -it 容器名或容器ID /bin/bash
```

#### 3.3.3 查看容器

```shell
docker ps     #查看当前运行的容器
docker ps –a    #查看当前所有的容器
```

#### 3.3.4 启动容器

```shell
docker start 容器名或容器ID
```

#### 3.3.5 停止容器

```shell
docker stop 容器名或容器ID
```

#### 3.3.6 删除容器   

（运行中的容器不能删除）

```shell
docker rm 容器名或容器ID 
```

#### 3.3.7 将容器保存为镜像

```shell
docker commit [OPTIONS] CONTAINER [REPOSITORY]:[TAG]
```

OPTIONS说明：

-a :提交的镜像作者；

-c :使用Dockerfile指令来创建镜像；

-m :提交时的说明文字；

-p :在commit时，将容器暂停。

$ docker commit 容器名 镜像名:tag

### 3.4 docker网络设置

#### 3.4.1 Docker基本网络

`Docker安装后自动创建3种网络：bridge、host、none`

Docker在启动时会开启一个虚拟网桥设备docker0，默认的地址为172.17.0.1/16，容器启动后都会被桥接到docker0上，并自动分配到一个ip地址。

| bridge 桥接网络模式：                                        |
| :----------------------------------------------------------- |
| 1、为每一个容器分配、设置IP等，并将容器连接到一个docker0虚拟网桥，默认为该模式 |
| 2、为容器分配独立IP，具有很好的网络隔离性，服务不会跟宿主机上的服务发送端口冲突问题 |
| 3、主机和容器间通过桥接的方式进行通信                        |
| 4、只能单机使用，不适合跨主机docker服务间通信                |

| host 主机本地网络模式：                                     |
| :---------------------------------------------------------- |
| 1、docker容器共享主机的ip、端口号等等网络资源，如果单机部署 |
| 2、只能单机使用，不适合跨主机docker服务间通信               |
| 3、这种网络模式效率最高                                     |

| overlay 集群网络模式：                               |
| :--------------------------------------------------- |
| 多节点集群下统一分配服务独立ip                       |
| 跨机器节点上的docker服务间能互相通信                 |
| 支持主机节点和集群网络内的节点间互相通信             |
| 支持节点间加密通信 注：windows机器节点不支持加密通信 |

`添加网桥，创建容器时可以指定网桥，不使用默认网桥`

```shell
docker network ls            #查看所有网络
docker network inspect 网络   #查看网络的相关信息

#自定义创建的默认default bridge   

docker network create --driver bridge --subnet 192.168.1.1/24 --gateway 192.168.1.1 mynet #自定义创建一个网络mynet

docker network rm 网络id  #删除网络
```

## 4.Docker封装Centos7大数据环境

### 4.1 编写Dockerfile

Pull一个centos7镜像，在此镜像基础上安装ssh服务，开放端口，上传jdk、hadoop等组件等操作，封装成hadoop大数据环境

> 已经封装好的镜像:
>
> ```
> registry.cn-hangzhou.aliyuncs.com/shangguan-hj/bigdata:gxjzy
> ```
>
> 拉取
>
> ```
> docker pull registry.cn-hangzhou.aliyuncs.com/510_repo/bigdata:gxjzy
> ```

#### 4.1.2 编写第一个dockerfile，封装一个新的镜像

```shell
# 选择一个已有的os镜像作为基础  
FROM centos:7
# 安装openssh-server和sudo软件包  
RUN yum install -y --nogpgcheck openssh-server sudo
#安装openssh-clients
RUN yum install -y --nogpgcheck openssh-clients
# 修改ssh配置文件，方便后面通过root用户进行ssh远程登录
RUN sed -i 's/ GSSAPIAuthentication yes /#GSSAPIAuthentication yes/g' /etc/ssh/sshd_config
RUN sed -i 's/ GSSAPICleanupCredentials no /#GSSAPICleanupCredentials no/g' /etc/ssh/sshd_config

#安装initscripts，方便 ip addr 查询网络状态
RUN yum install -y --nogpgcheck initscripts

#安装防火墙 
RUN yum install firewalld -y

#安装which  hadoop版本号查看需要用到  
RUN yum install which -y

#mysql 初始化报错 ，缺少libnuma.so.1情况，缺啥补啥 
RUN yum install numactl -y
RUN yum install libaio -y
RUN yum install libnuma.so.1 -y


# 添加用户root，密码123，并且将此用户添加到sudoers里  
RUN echo "root:123" | chpasswd
RUN echo "root   ALL=(ALL)       ALL" >> /etc/sudoers

# 启动sshd服务并且暴露22端口  
RUN mkdir /var/run/sshd
EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
RUN mkdir /opt/{software,module}
#封装：docker build -t"centos-ssh-root" . 
```

编辑好脚本后，使用build命令开始运行

格式：docker build -t”镜像名称” . 

```shell
docker build -t"centos-ssh-root" .
```

运行效果：![image-20230812190933232](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202309241324924.png)

查看创建的镜像：

 ![image-20230812191126430](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202309241324183.png)

#### 4.1.3 编写第二个Dockerfile，上传所需环境包，再次封装成一个新的镜像

封装的系统只是完成了ssh服务的安装，开放22端口，配置root用户。并没有达到大数据环境的使用要求，接下来在这个封装好的系统上，再进行封装，上传大数据环境需要的组件

```shell
#软件包根据自己所需的填写
FROM centos-ssh-root
COPY apache-flume-1.9.0-bin.tar.gz /opt/software/
COPY apache-hive-3.1.2-bin.tar.gz /opt/software/
COPY flink-1.14.0-bin-scala_2.12.tgz /opt/software/
COPY hadoop-3.1.3.tar.gz /opt/software/
COPY hbase-2.2.3-bin.tar.gz /opt/software/
COPY jdk-8u162-linux-x64.tar.gz /opt/software/
COPY kafka_2.12-2.4.1.tgz /opt/software/
COPY maxwell-1.29.0.tar.gz /opt/software/
COPY redis-6.2.6.tar.gz /opt/software/
COPY scala-2.12.0.tgz /opt/software/
COPY spark-3.1.1-bin-hadoop3.2.tgz /opt/software/
COPY clickhouse-21.9.4.35 /opt/software/
COPY mysql /opt/software/
COPY zookeeper-3.4.6.tar.gz /opt/software/
COPY sqoop-1.4.2.bin__hadoop-2.0.0-alpha.tar.gz /opt/software/

#封装：docker build -t"bigdata" . 
```

编辑好脚本后，使用build命令开始运行

格式：docker build -t”镜像名称” . 

```shell
docker build -t"bigdata" .
```

运行效果：![image-20230812191358805](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202309241324473.png)

查看新封装的镜像：

![image-20230812192151801](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202309241324220.png)



#### 4.1.4 :star:镜像优化

> 综合上面两个dockerfile进行镜像优化

```sh
FROM centos:7
RUN yum install -y --nogpgcheck openssh-server sudo && \
    yum install -y --nogpgcheck openssh-clients && \
    sed -i 's/ GSSAPIAuthentication yes /#GSSAPIAuthentication yes/g' /etc/ssh/sshd_config && \
    sed -i 's/ GSSAPICleanupCredentials no /#GSSAPICleanupCredentials no/g' /etc/ssh/sshd_config && \
    yum install -y --nogpgcheck initscripts && \
    yum install which -y && \
    yum install numactl libaio libnuma.so.1 -y && \
    echo "root:1" | chpasswd && \
    echo "root   ALL=(ALL)       ALL" >> /etc/sudoers && \
    mkdir /var/run/sshd && \
    mkdir /opt/{software,module} 
COPY . /opt/software 
EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
```

> 开始构建

```sh
[root@docker-server ~/bigdata]# docker build -t bigdata .
```

> 查看构建的镜像

```sh
[root@docker-server ~/bigdata]# docker images | grep bigdata
bigdata                                                  latest    78d3e81ed5ac   4 hours ago   5.57GB
```



### 4.2 Hadoop配置容器启动：

**集群规划，一主两从**



> master:

测试:

```shell
docker run -id --name master --hostname master --net mynet --privileged=true -v /sys/fs/cgroup:/sys/fs/cgroup -P -p 50070:50070 -p 8088:8088 bigdata /usr/sbin/init
```

:star: 实战:

自定义创建docker网络:

(这会创建一个自定义网络，允许在该网络内分配IP地址，并且子网为`192.168.1.0/24`)

```sh
sudo docker network create --subnet=192.168.1.0/24 mynet
```

创建一个名为master的容器,ip为192.168.1.10:

```sh
sudo docker run -d --privileged --name master --hostname master --network mynet --ip 192.168.1.10 \
  -p 16010:16010 -p 2181:2181 -p 3306:3306 -p 6379:6379 -p 8031:8031 -p 8032:8032 \
  -p 8033:8033 -p 8080:8080 -p 8081:8081 -p 8020:8020 -p 8088:8088 -p 8123:8123 \
  -p 9000:9000 -p 9083:9083 -p 9092:9092 -p 9866:9866 -p 9870:9870 -p 10000:10000 \
  -p 60010:60010 -p 12321:12321 -p 4040:4040 \
  -v /sys/fs/cgroup:/sys/fs/cgroup \
  bigdata:gxjzy /usr/sbin/init
```



> slave1:



测试:

```shell
docker run -id --name slave1 --hostname slave1 --net mynet --privileged=true -P bigdata /usr/sbin/init
```

:star: 实战:

创建一个名为master的容器,ip为192.168.1.10:

```sh
sudo docker run -d --privileged --name slave1 --hostname slave1 --network mynet --ip 192.168.1.20 \
  -v /sys/fs/cgroup:/sys/fs/cgroup \
  bigdata:gxjzy /usr/sbin/init
```





> slave2:



测试:

```shell
docker run -id --name slave2 --hostname slave2 --net mynet --privileged=true -P bigdata /usr/sbin/init
```

:star: 实战:

```sh
sudo docker run -d --privileged --name slave2 --hostname slave2 --network mynet --ip 192.168.1.30 \
  -v /sys/fs/cgroup:/sys/fs/cgroup \
  bigdata:gxjzy /usr/sbin/init
```

查看创建情况：

```sh
docker ps
```

![image-20240108011713901](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202401080117475.png)



iptables端口映射（略）(后续添加端口映射需要使用)

```shell
iptables -t nat -vnL  #查看端口映射

iptables -t nat -A DOCKER -p tcp --dport 8089 -j DNAT --to-destination 192.168.12.10:8088  #添加端口映射

iptables -t nat -vnL DOCKER --line-number #显示端口行号

iptables -t nat -D DOCKER {行号}  #删除规则
```

进入容器：

```shell
docker exec -it master bash
```

## 5. 使用docker-compose编排工具快速建立基础环境

> 首先安装好docker-compose编排工具(略)



### 5.1 编写docker-compose.yml

自定义容器ip:

| 容器   | ip           |
| ------ | ------------ |
| master | 192.168.1.10 |
| slave1 | 192.168.1.20 |
| slave2 | 192.168.1.30 |



```sh
[root@docker-server ~/bigdata-work]# vim docker-compose.yml 
```

```yml
version: "2.2"
services:
  master:
    restart: always
    image: registry.cn-hangzhou.aliyuncs.com/shangguan-hj/bigdata:gxjzy
    container_name: master
    hostname: master
    networks:
      mynet:
        ipv4_address: "192.168.1.10"
    privileged: true
    ports:
      - 1022:22
      - 50070:50070
      - 8088:8088
    volumes:
      - /sys/fs/cgroup:/sys/fs/cgroup
    command: /usr/sbin/init

  slave1:
    restart: always
    image: registry.cn-hangzhou.aliyuncs.com/shangguan-hj/bigdata:gxjzy
    container_name: slave1
    hostname: slave1
    networks:
      mynet:
        ipv4_address: "192.168.1.20"
    privileged: true
    ports:
      - 1023:22
    volumes:
      - /sys/fs/cgroup:/sys/fs/cgroup
    command: /usr/sbin/init
    
  slave2:
    restart: always
    image: registry.cn-hangzhou.aliyuncs.com/shangguan-hj/bigdata:gxjzy
    container_name: slave2
    hostname: slave2
    networks:
      mynet:
        ipv4_address: "192.168.1.30"
    privileged: true
    ports:
      - 1024:22
    volumes:
      - /sys/fs/cgroup:/sys/fs/cgroup
    command: /usr/sbin/init

# 连接外部网络
networks:
  mynet:
    ipam:
      config:
      - subnet: 192.168.1.0/24
```

### 5.2 启动docker容器

```sh
[root@docker-server ~/bigdata-work]# docker-compose up -d
```

![image-20231211150006708](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202312111500849.png)

### 5.3 查看创建情况

#### 5.3.1 方式一(了解):

```sh
[root@docker-server ~/bigdata-work]# docker-compose ps
```

![image-20231211150148532](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202312111501626.png)

#### 5.3.2 方式二:

```sh
[root@docker-server ~/bigdata-work]# docker ps
```

![image-20231211150204022](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202312111502108.png)



### 5.4 进入容器

**master:**

```sh
[root@docker-server ~/bigdata-work]# docker exec -it master bash
[root@master /]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
96: eth0@if97: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:a8:01:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.168.1.2/24 brd 192.168.1.255 scope global eth0
       valid_lft forever preferred_lft forever
[root@master /]# 
```

**slave1:**

```sh
[root@docker-server ~/bigdata-work]# docker exec -it slave1 bash
[root@slave1 /]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
34: eth0@if35: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:a8:01:04 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.168.1.4/24 brd 192.168.1.255 scope global eth0
       valid_lft forever preferred_lft forever
[root@slave1 /]# 
```

**slave2:**

```sh
[root@docker-server ~/bigdata-work]# docker exec -it slave2 bash
[root@slave2 /]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
32: eth0@if33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:a8:01:03 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.168.1.3/24 brd 192.168.1.255 scope global eth0
       valid_lft forever preferred_lft forever
[root@slave2 /]# 
```



### 5.5 连接客户端工具xshell

==注意定义的ssh端口==



> master

![](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202312111517302.png)

![image-20231211151542886](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202312111515957.png)



> slave1

![image-20231211151638743](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202312111516827.png)

![image-20231211151651296](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202312111516374.png)



> slave2

![image-20231211151809445](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202312111518533.png)

![image-20231211151826289](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202312111518365.png)

完成

## 6. asbru-cm工具的使用

1、

![image-20230320175907854](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202401080134495.png)

2、

![image-20230320180309740](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202401080134744.png)
