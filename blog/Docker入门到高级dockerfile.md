---
title: Docker入门到高级dockerfile
categories: Docker
tags: [docker,dockerfile]
---

# Docker容器基础入门实战

## Linux Centos7环境下安装Docker

参考文章：https://help.aliyun.com/document_detail/51853.html 

查看docker版本

```bash
docker version
```

输出

```bash
[root@CentOS7 yum.repos.d]# docker version
Client:
 Version:         1.13.1
 API version:     1.26
 Package version: docker-1.13.1-204.git0be3e21.el7.x86_64
 Go version:      go1.10.3
 Git commit:      0be3e21/1.13.1
 Built:           Fri Mar 19 13:57:09 2021
 OS/Arch:         linux/amd64

Server:
 Version:         1.13.1
 API version:     1.26 (minimum version 1.12)
 Package version: docker-1.13.1-204.git0be3e21.el7.x86_64
 Go version:      go1.10.3
 Git commit:      0be3e21/1.13.1
 Built:           Fri Mar 19 13:57:09 2021
 OS/Arch:         linux/amd64
 Experimental:    false
```

查看docker的详细信息

```bash
docker info
```

输出

```bash
[root@CentOS7 yum.repos.d]# docker info
Containers: 5
 Running: 0
 Paused: 0
 Stopped: 5
Images: 5
Server Version: 1.13.1
Storage Driver: overlay2
 Backing Filesystem: extfs
 Supports d_type: true
 Native Overlay Diff: true
Logging Driver: journald
Cgroup Driver: systemd
Plugins:
 Volume: local
 Network: bridge host macvlan null overlay
Swarm: inactive
Runtimes: runc docker-runc
Default Runtime: docker-runc
Init Binary: /usr/libexec/docker/docker-init-current
containerd version:  (expected: aa8187dbd3b7ad67d8e5e3a15115d3eef43a7ed1)
runc version: 66aedde759f33c190954815fb765eedc1d782dd9 (expected: 9df8b306d01f59d3a8029be411de015b7304dd8f)
init version: fec3683b971d9c3ef73f284f176672c44b448662 (expected: 949e6facb77383876aeff8a6944dde66b3089574)
Security Options:
 seccomp
  WARNING: You're not using the default seccomp profile
  Profile: /etc/docker/seccomp.json
Kernel Version: 3.10.0-1160.15.2.el7.x86_64
Operating System: CentOS Linux 7 (Core)
OSType: linux
Architecture: x86_64
Number of Docker Hooks: 3
CPUs: 2
Total Memory: 3.561 GiB
Name: CentOS7.9shili
ID: IEMJ:VMKA:YMU4:WOBC:O5PP:M2DQ:4QWA:6QZF:HAHA:D6QY:4V2O:YGLY
Docker Root Dir: /var/lib/docker
Debug Mode (client): false
Debug Mode (server): false
Registry: https://index.docker.io/v1/
Experimental: false
Insecure Registries:
 127.0.0.0/8
Registry Mirrors:
 https://pb5bklzr.mirror.aliyuncs.com
Live Restore Enabled: false
Registries: docker.io (secure)
```

## Docker镜像的搜索下载以及查看删除实战

```bash
# 什么是镜像？
# 查看本地镜像：
docker images
# 搜索镜像：
docker search centos
# 搜索镜像并过滤是官方的：
docker search --filter "is-official=true" centos
# 搜索镜像并过滤大于多少颗星星的：
docker search --filter stars=10 centos
# 下载centos7镜像：
docker pull centos:7
# 修改本地镜像名字（小写）：
docker tag centos:7 mycentos:1
# 本地镜像的删除：
docker rmi centos:7
```

## Docker核心基础之配置阿里云镜像加速

阿里云镜像加速器配置地址：https://cr.console.aliyun.com/cn-shenzhen/instances/mirrors

```bash
# 配置步骤：
vi /etc/docker/daemon.json
# 重启：
systemctl daemon-reload && systemctl restart docker
# 添加
{
 "registry-mirrors": ["https://pb5bklzr.mirror.aliyuncs.com"]
}
```

## Docker核心基础之容器的构建等基本操作

```bash
# 运行一个容器
docker run -it 镜像的名字或者id
-i ：表示以交互模式运行容器（让容器的标准输入保持打开）
-d：表示后台运行容器，并返回容器ID
-t：为容器重新分配一个伪输入终端
exit 退出伪输入终端

查看本地所有的容器：docker ps -a
查看本地正在运行的容器：docker ps

# 如果不加上7，默认就是 centos:latest
docker run -itd --name=mycentos centos:7
# 输出一串很长的容器id

# 停止容器：
docker stop CONTAINER_ID / CONTAINER_NAME
# 如果为容器指定了name
docker stop NAMES
docker stop/start/restart mycentos

# 删除容器：
docker rm CONTAINER_ID / CONTAINER_NAME
# 强制删除容器：
docker rm -f CONTAINER_ID / CONTAINER_NAME
# 查看容器详细信息：
docker inspect CONTAINER_ID / CONTAINER_NAME
# 进入容器 /bin/bash 表示以什么容器进去的
docker exec -it mycentos /bin/bash
# 启动多个容器，每个容器的id都是不一样的
# 一次性停止所有容器：
docker stop $(docker ps -a -q)
# 批量启动所有容器
docker start $(docker ps -a -q)
```

## Docker核心基础之容器的文件复制与挂载

```bash
# 从宿主机复制到容器：docker cp 宿主机本地路径 容器名字/ID：容器路径
cd /root
cat >123.txt
docker cp /root/123.txt mycentos:/home/
# 进入容器
docker exec -it mycentos /bin/bash
cd home/
ls 

# 继续往宿主机追加，在容器伪终端不能同步看到添加的内容
cat >>123.txt
jjjjj
# 在容器里新建文件
# 从容器复制到宿主机：docker cp 容器名字/ID：容器路径 宿主机本地路径
docker cp mycentos:/home/456.txt /root

# 宿主机文件夹挂载到容器里：docker run -itd -v 宿主机路径:容器路径 镜像ID
mkdir xdclass
# 只能重新构建一个新的容器，再挂载上去
docker run -itd -v /root/xdclass/:/home centos:7
# 进入容器
docker exec -it  CONTAINER_ID / CONTAINER_NAME /bin/bash
cd /home 
ls
# 到宿主机下创建一个文件
cd xdclass 
touch 123.txt 
# 到容器中可以看到有个文件123.txt
# 很明显数据是同步的
```

# Docker核心必备之自定义镜像实战

## docker目前镜像的制作的两种方法

1. 基于Docker Commit制作镜像
2. 基于dockerfile制作镜像，Dockerfile方式为主流的制作镜像方式

## Commit构建自定义镜像

```bash
# 启动并进入容器：
docker run -it centos:7 /bin/bash
# 在/home 路径下创建xdclass文件夹：
mkdir /home/xdclass
# 安装ifconfig命令：
yum -y install net-tools
# 重启容器，查看容器的xdclass文件夹还在不在，ifconfig命令能不能用
docker restart 67862569d4f
# 删除容器，再重新启动一个容器进入查看有没有xdclass文件夹：很显然这个文件和命令都不在了
docker rm 67862569d4f7 && docker run -it centos:7 /bin/bash
# 在删除第一个容器前先构建镜像：
docker run -it centos:7 /bin/bash
docker commit 4eb9d14ebb18 mycentos:v1
docker commit -a "XD" -m "mkdir /home/xdclass" 4eb9d14ebb18 mycentos:v1
-a：标注作者
-m：说明注释，比如在这个容器里做了什么操作
# 查看镜像，发现多了一个mycentos 
docker images 
# 查看详细信息：
docker inspect mycentos
# 启动容器：
docker run -itd mycentos:v1 /bin/bash
# 进入容器查看：里面的文件存在和ifconfig命令能够使用
docker exec -it 2a4d38eca64f /bin/bash
```

## 核心必备知识之Dockerfile构建镜像实战

- Dockerfile

  注意：dockfile和123.txt的路径要一样

```dockerfile
# this is a dockerfile
FROM centos:
MAINTAINER XD 123456@qq.com
RUN echo "正在构建镜像！！！"
WORKDIR /home/xdclass
COPY 123.txt /home/xdclass
RUN yum install -y net-tools
```

- 构建镜像

```bash
# 查看：
docker images
# 构建, . 这个点是指当前路径的意思
docker build -t mycentos:v2 .
# 查看：
docker images
# 进入验证：验证成功
docker run -it mycentos:v2 /bin/bash
# 运行后直接进入工作路径/home/xdclass
```

## Docker核心知识之镜像分层结构剖析

```bash
docker history mycentos:v2
```

从输出中可以看出每一个步骤都会生成一个镜像层，叠加在基础镜像层上面

镜像层是静态的，只可以读不可以修改，而容器层是动态的，可以进行修改

![image-20210503122716371](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210503122718.png)

总结：共享资源；对容器的任何改动都是发生容器层；容器层是可写可读，而镜像层只读

## 不得不掌握的Dockerfile基础指令

- 命令格式

```
shell命令格式：RUN yum install -y net-tools
exec命令格式：RUN [ "yum","install" ,"-y" ,"net-tools"]
```

- 一些常用的Dockerfile指令

```
FROM：基于哪个镜像
MAINTAINER：注明作者
COPY：复制文件进入镜像（只能用相对路径，不能用绝对路径）
ADD：复制文件进入镜像（假如文件是.tar.gz文件会解压）
WORKDIR：指定工作目录，假如路径不存在会创建路径
ENV：设置环境变量
EXPOSE：暴露容器端口
RUN：在构建镜像的时候执行，作用于镜像层面
ENTRYPOINT：在容器启动的时候执行，作用于容器层，dockerfile里有多条时只允许执行最后一条
CMD：在容器启动的时候执行，作用于容器层，dockerfile里有多条时只允许执行最后一条，与ENTRYPOINT的区别就是：容器启动后执行默认的命令或者参数，允许被修改
```

- dockerfile

![image-20210503125049245](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210503125100.png)

```bash
docker build -t mycentos:v3 .
```

![image-20210503125214739](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210503125404.png)

- 修改dockerfile，ENTRYPOINT可以接收CMD的参数（前提是exec命令格式）

![image-20210503131302967](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210503131305.png)

- 构建容器并执行

![image-20210503131438829](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210503131925.png)

- 在运行后加入 aux参数，可以覆盖CMD

![image-20210503131543407](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210503131933.png)

## 实战系列之Dockerfile构建JAVA网站镜像

```bash
# 本地宿主机配置jdk
vi /etc/profile
# copy到 /etc/profile
export JAVA_HOME=/usr/local/jdk
export JRE_HOME=$JAVA_HOME/jre
export CLASSPATH=$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH

# 加载环境变量
source /etc/profile
```

Dockerfile构建java环境，注意dockerfile要放在安装包的同个目录下

```dockerfile
FROM centos:7
ADD jdk-8u211-linux-x64.tar.gz /usr/local
RUN mv /usr/local/jdk-1.8.0_211 /usr/local/jdk
ENV JAVA_HOME=/usr/local/jdk
ENV JRE_HOME=$JAVA_HOME/jre
ENV CLASSPATH=$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
ENV PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH


ADD apache-tomcat-8.5.35.tar.gz /usr/local
RUN mv /usr/local/apache-tomcat-8.5.35  /usr/local/
EXPOSE 8080
# 如果使用我们常用的startup.sh作为容器启动脚本，容器会自动关闭，此时Tomcat在后台运行，没有在前台运行的线程，所以只能在前台启动tomcat
ENTRYPOINT ["/usr/local/tomcat/bin/catalina.sh","run"]
```

构建，启动容器

```bash
docker build -t mycentos:jdk .
docker run -itd -p 80:8080  mycentos:jdk /bin/bash

# 宿主机挂载到容器
docker run -itd -p 8080:8080 -v /root/test/ROOT:/usr/local/tomcat/webapps/ROOT mycentos:jdk /bin/bash

# 创建index.html
cd /root/test/ROOT
vi index.html
```

网页进行访问验证

## 实战系列之Dockerfile构建nginx镜像

**Dockerfile构建nginx**

- 安装nginx的shell脚本：/usr/local/nginx_install.sh(进入安装包里面)

```shell
#!/bin/bash
yum install -y gcc gcc-c++ make pcre pcre-devel zlib zlib-devel
cd /usr/local/nginx-1.16.0
./configure --prefix=/usr/local/nginx && make && make install
```

- dockfile

```dockerfile
FROM centos:
ADD nginx-1.16.0.tar.gz /usr/local
COPY nginx_install.sh /usr/local
RUN sh /usr/local/nginx_install.sh
EXPOSE 80
```

- 制作Nginx镜像

```bash
docker build -t mycentos:nginx .
```

- 使用前台方式永久运行：

```
docker run -itd -p 80:80 mycentos:nginx /usr/local/nginx/sbin/nginx -g "daemon off;"
```

Nginx镜像启动注意：在容器里nginx是以daemon方式启动，退出容器时，nginx程序也会随着停止

## 实战系列之Dockerfile构建redis镜像

- 不用dockerfile安装redis的过程

  编写redis编译安装shell脚本redis_install.sh

  sh redis_install.sh

  ```shell
  #!/bin/bash
  yum install -y gcc gcc-c++ make openssl openssl-devel
  cd /home/redis-4.0.9
  # PREFIX=/usr/local/redis 指定安装路径 写在中间也没错
  make && make PREFIX=/usr/local/redis install
  mkdir -p /usr/local/redis/conf/
  cp /home/redis-4.0.9/redis.conf /usr/local/redis/conf/
  ```

  默认是以前台的方式启动

  ```bash
  /usr/local/redis/bin/redis-server /usr/local/redis/conf/redis.conf
  ```

- 编写dockerfile

  ```dockerfile
  FROM centos:
  ADD redis-4.0.9.tar.gz /home
  COPY redis_install.sh /home
  RUN sh /home/redis_install.sh
  ENTRYPOINT /usr/local/redis/bin/redis-server /usr/local/redis/conf/redis.conf
  ```

- 测试redis:

  ```bash
  # 启动容器：
  docker run -itd -p 6380:6379 mycentos:redis #6380是宿主机端口， 6379是容器的端口
  # 进入容器：
  docker exec -it 9b402baeaba7 /bin/bash
  # 宿主机连接redis：
  /usr/local/redis/bin/redis-cli -p 6380
  # 验证
  ```

  宿主机连接容器的redis服务，如果没有连接成功，说明容器的redis开启了保护模式，修改配置文件redis.conf

  设置protected-mode no

  同时注释 bind 127.0.0.1

  或者在脚本文件redis_install.sh添加最后两行

  ```shell
  #!/bin/bash
  yum install -y gcc gcc-c++ make openssl openssl-devel
  cd /home/redis-4.0.
  make && make PREFIX=/usr/local/redis install
  mkdir -p /usr/local/redis/conf/
  cp /home/redis-4.0.9/redis.conf /usr/local/redis/conf/
  sed -i '69s/127.0.0.1/0.0.0.0/' /usr/local/redis/conf/redis.conf
  sed -i '88s/protected-mode yes/protected-mode no/' /usr/local/redis/conf/redis.conf
  ```

  docker restart 容器的id

  ```bash
  [root@localhost home]# /usr/local/redis/bin/redis-cli -p 6380
  127.0.0.1:6380> set name xdclass
  OK
  127.0.0.1:6380> get name
  "xdclass"
  127.0.0.1:6380>
  ```

## 实战系列之docker快速部署mysql数据库并初始化

```bash
# 官方网址：
# https://hub.docker.com/
# 启动命令：
docker run --name some-mysql -p 3307:3306 -e MYSQL_ROOT_PASSWORD=abc123456 -d mysql:5.7
# 进入容器命令：
docker exec -it 4336ae28fbfa env LANG=C.UTF-8 /bin/bas
```

初始化sql语句：

```sql
# 建库
create database `db_student`;
SET character_set_client = utf8;
use db_student;
# 建表
drop table if exists `user`;
CREATE TABLE user (
id tinyint(5) zerofill auto_increment not null comment '学生学号',
name varchar(20) default null comment '学生姓名',
age tinyint default null comment '学生年龄',
class varchar(20) default null comment '学生班级',
sex char(5) not null comment '学生性别',
unique key (id)
)engine=innodb charset=utf8;
# 插入数据
insert into user values('1','小明','15','初三','男');
insert into user values('2','小红','13','初二','女');
insert into user values('3','小东','14','初一','男');
insert into user values('4','小西','12','初二','男');
```

查看数据库会发现中文根本插入不进去，直接拉取的mysql的dockerfile的基础镜像不是centos

那这个容器就没用

```bash
# 删除容器
dockerfile rm -f 容器的id
# 重新运行另一个容器
docker run --name some-mysql -p 3307:3306 -e MYSQL_ROOT_PASSWORD=abc123456 -d mysql:5.7
docker exec -it 4336ae28fbfa env LANG=C.UTF-8 /bin/bash
```

建立一个脚本init.sql

```sql
# 建库
create database `db_student`;
SET character_set_client = utf8;
use db_student;
# 建表
drop table if exists `user`;
CREATE TABLE user (
id tinyint(5) zerofill auto_increment not null comment '学生学号',
name varchar(20) default null comment '学生姓名',
age tinyint default null comment '学生年龄',
class varchar(20) default null comment '学生班级',
sex char(5) not null comment '学生性别',
unique key (id)
)engine=innodb charset=utf8;
# 插入数据
insert into user values('1','小明','15','初三','男');
insert into user values('2','小红','13','初二','女');
insert into user values('3','小东','14','初一','男');
insert into user values('4','小西','12','初二','男');
```

cat >init.sql

- 新建dockerfile

```dockerfile
FROM mysql:5.7
WORKDIR /docker-entrypoint-initdb.d # 这里无所谓
ENV LANG=C.UTF-8
ADD init.sql . # 用copy只会复制，不会执行这个文件，ADD有解压和执行的作用
```

- 构建，执行，进入容器，查表

```bash
docker build -t my-mysql:5.7 .
docker run --name some-mysql -p 3307:3306 -e MYSQL_ROOT_PASSWORD=abc123456 -d my-mysql:5.7
docker exec -it 容器id  /bin/bash
select * from db_student.user;
```

# Docker核心知识之网络模式与特权指令

##  Docker 容器的网络模式

默认的三种网络模式： 

1. bridge：桥接模式 
2. host：主机模式 
3. none：无网络模式 

查看网络模式： 

```bash
docker network ls

# --net 指定启动的网络模式
```

## Docker 容器的bridge模式实战

Docker桥接网络模式 

桥接模式是docker 的默认网络设置，当Docker服务启动时，会在主机上创建一个名为docker0的虚拟网桥，并选择一个和宿主机不同的IP地址和子网分配给docker0网桥 

桥接拓扑图

![image-20210503171539187](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210503171542.png)

安装工具：

```bash
yum -y install net-tools # route -n 查看路由表
yum install -y bridge-utils # brctl show 当前网桥的连接情况
```

查看桥接情况：

```bash
[root@localhost ~]# brctl show
bridge name 	bridge id 			STP enabled 	interfaces
docker0 		8000.0242055f4af6 	no 				veth60d950c
												 veth827b88f
											      vethaabef5c
												  vethd22be73
											      vethe1396d2
```

##  Docker 容器的host模式实战

host 模式：该模式下容器是不会拥有自己的ip地址，而是使用宿主机的ip地址和端口。

![image-20210503172036787](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210503172039.png)

```bash
# 启动nginx容器命令并防火墙放开80端口：
docker run -d --net=host mycentos:nginx /usr/local/nginx/sbin/nginx -g "daemon off;"
firewall-cmd --zone=public --add-port=80/tcp --permanent
firewall-cmd --reload
```

##  Docker 容器的none模式介绍

none模式：关闭模式，无法连外网

## Docker 容器间基于Link实现单向通信

![](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210503175947.png)

![](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210503180718.png)

![](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210503181249.png)

```bash
# 启动mysql数据库容器：
docker run --name mydb -e MYSQL_ROOT_PASSWORD=abc123456 -d mysql:5.7
# 启动tomcat应用容器并link到mysql数据库：
docker run -itd --name tomcat1 --link mydb tomcat:tag

# 注意：mydb 这个容器一定要存在！
# 官方版的mysql 5.7 需要安装工具才有ping命令:
apt-get update && apt-get install iputils-ping
# 注意：mysql是ping不通其他所有tomcat的
```

 ## Docker容器间利用brige网桥实现双向通信

```bash
# 创建一个新的网桥：
docker network create -d bridge my_bridge
# 启动第一个容器：
docker run -itd --name tomcat centos:7
# 启动第二个容器：
docker run -itd --name redis centos:7
# 把第一个容器加入网桥：
docker network connect my_bridge tomcat
# 把第二个容器加入网桥：
docker network connect my_bridge redis
# 最后分别进入俩个容器中进行验证 互ping
```

## Docker容器的特权模式介绍

```bash
# 启动一个普通的容器
docker run -itd --name mycentos centos:7 /bin/bash
# 安装网络工具：
yum -y install net-tools
# 执行route -n
# 删除网关：
route del default gw 172.17.0.1
# 启动拥有特权模式的容器：
docker run -itd --privileged=true --name mycentos1 centos:7 /bin/bash
# 进入容器：
docker exec -it ef /bin/bash
# 删除网关
route del default gw 172.17.0.1
# 成功 备注：特权模式用的比较少
```

## Docker核心知识之Volume数据共享

Volume的介绍使用

- dockerfile

```dockerfile
FROM centos:7
VOLUME ["/usr/local"]
```

docker inspect 容器的id【source是指宿主机的共享路径，destination是指容器的共享路径】

注意：在dockerfile里设置volume是无法修改宿主机的挂载路径的

![](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210503185844.png)

```bash
# 使用volume容器共享创建nginx集群
# 使用--volumes-from 实现容器与容器之间volume共享
# 创建nginx1
docker run -itd -p 8080:80 -v /usr/local/nginx/html:/usr/local/nginx/html --name nginx1 mycentos:nginx /usr/local/nginx/sbin/nginx -g "daemon off;"
# 创建nginx2
docker run -itd -p 8081:80 --volumes-from nginx1 --name nginx2 mycentos:nginx /usr/local/nginx/sbin/nginx -g "daemon off;"
# 创建nginx3
docker run -itd -p 8082:80 --volumes-from nginx1 --name nginx3 mycentos:nginx /usr/local/nginx/sbin/nginx -g "daemon off;"
# 对/usr/local/nginx/html/index.html进行修改
# 打开浏览器进行访问测试
# 使用docker inspect 容器ID 可以查看详细的挂载信息
```

# 实战系列之利用Compose操作容器

## 实用工具Docker-Compose的介绍与安装

Compose的介绍
docker-compose：是一个用于定义和运行多容器 Docker 的应用程序工具，可以帮助我们可以轻松、高效的管理容器

```bash
# compose的安装
# 安装pip工具
yum install -y epel-release
yum install -y python-pip
# 安装docker-compose
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple docker-compose==1.24.1
# 查看docker-compose版本
docker-compose version
```

安装pip报错解决：

```bash
[root@localhost yum.repos.d]# yum install python-pip -y
已加载插件：fastestmirror
One of the configured repositories failed (未知),
and yum doesn't have enough cached data to continue. At this point the only
safe thing yum can do is fail. There are a few ways to work "fix" this:
1. Contact the upstream for the repository and get them to fix the problem.
2. Reconfigure the baseurl/etc. for the repository, to point to a working
upstream. This is most often useful if you are using a newer
distribution release than is supported by the repository (and the
packages for the previous distribution release still work).
3. Disable the repository, so yum won't use it by default. Yum will then
just ignore the repository until you permanently enable it again or use
--enablerepo for temporary usage:
yum-config-manager --disable <repoid>
4. Configure the failing repository to be skipped, if it is unavailable.
Note that yum will try to contact the repo. when it runs most commands,
so will have to try and fail each time (and thus. yum will be be much
slower). If it is a very temporary problem though, this is often a nice
compromise:
yum-config-manager --save --setopt=<repoid>.skip_if_unavailable=true
Cannot retrieve metalink for repository: epel/x86_64. Please verify its path and
try again
```

- 解决： vi /etc/yum.repos.d/epel.repo 修改配置文件，注释掉metalink ，取消注释 baseurl

![image-20210503193307294](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210503193309.png)

## 实用工具Docker-Compose的快速上手

- 编写一个最最简单的yml（3是指yml格式的版本）

```yml
version: '3'
services:
  redis:
    image: mycentos:redis
```

- compose操作容器（一定要进入配置文件目录）

```bash
# 后台启动容器：
docker-compose up -d
# 查看容器运行情况：
docker-compose ps
# 停止并删除容器：
docker-compose down
# 停止并删除容器并删除volume：
docker-compose down --volumes
# 停止启动容器：
docker-compose stop
docker-compose start
# docker-compose exec的使用：进入容器
docker-compose exec redis bash
# 总结：
# 操作docker-compose一定要在配置文件docker-compose.yml文件路径下操作
# 格式一定要注意，该空格要空格，两格
```

## 实用工具Docker-Compose核实用技能

Compose核心实用技能掌握：

1. docker-compose.yml的三大部分：version，services，networks，最关键是services和networks两个部分 
2. compose设置网络模式  
3. compose使用端口映射 
4. compose设置文件共享 
5. compose管理多个容器 
6. docker-compose.yml

```yml
version: '3'
services:
  nginx:
    image: mycentos:nginx
      # network_mode: "host"
      ports:
      - "8080:80"
      network_mode: "host"
      volumes:
      - /home:/usr/local/nginx/html
      - /var/logs/nginx/logs:/usr/local/nginx/logs
      command: /usr/local/nginx/sbin/nginx -g "daemon off;"
  redis:
    image: mycentos:redis
    ports:
    - "6380:6379"
```

## 实战项目篇之利用Docker-Compose快速搭建个人博客

Compose快速搭建个人博客wordpress

官网：https://docs.docker.com/compose/wordpress/ 

docker-compose.yml

```yml
version: '3.3'
services:
  db:						# 容器
    image: mysql:5.7 		  #docker run -itd mysql:5.7
    volumes:
      - db_data:/var/lib/mysql #采用的是卷标的形式挂载（注意：- db_data是参数，可以变，自定义，必须与下面对应）
    restart: always 		  #自动重启，保证服务在线
    environment:
      MYSQL_ROOT_PASSWORD: somewordpress #指定环境变量 docker -itd -e MYSQL_ROOT_PASSWORD=somewordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    
  wordpress:				# 容器
    depends_on:
      - db # - db 是参数，合起来的意思是只有当上面的mysql数据库安装成功后，这个wordpress才可以被安装，还有一个功能，就是docker --link 将上面的mysql数据库，与这个wordpress应用连起来
    image: wordpress:latest
    ports:
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
volumes:
  db_data: {}
```

- 启动wordpress

```bash
docker-compose up -d
# 打开浏览器访问：IP:8000
# 进行安装配置
# 将删除容器和默认网络，但会保留WordPress数据库：
docker-compose down
# 将删除容器，默认网络和WordPress数据库： 
docker-compose down --volumes
```

## 实战项目篇之Docker-Compose 详细分析

分析上节课docker-compose.yml 

docker-compose.yml

```bash
# docker-compose中有两种方式可以设置volumes
# 使用具体路径直接挂载到本地，特点就是直观
# 使用卷标的形式，特点就是简洁，但是不知道数据到底在本地的什么位置。需要通过卷标查看
docker volume ls
docker volume inspect wordpress_db_data
```

# Docker企业核心知识之镜像仓库实战

## 公司中Docker镜像仓库使用

什么是镜像仓库? 存放着很多镜像的仓库
为什么要使用镜像仓库？起到备份作用，方便其他机器下载使用

镜像仓库的种类？我们可以大致分为俩大类：

1. 公共镜像仓库
   官方：https://hub.docker.com/，基于各个软件开发或者有软件提供商开发的
   非官方：其他组织或者公司开发的镜像，供大家免费使用
2. 私有镜像仓库
   公司自己搭建的，用于存放公司内部的镜像，自己管理，不提供给外部使用，避免了商业项目暴露出
   去的风险

![image-20210503205038487](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210503205041.png)

## 阿里云镜像仓库的搭建与使用

阿里云镜像仓库申请地址： https://cr.console.aliyun.com/cn-shanghai/instances/repositories 

步骤： 登录阿里云Docker Registry

```bash
$ sudo docker login --username=穿靴子的狸花猫 registry.cn-shenzhen.aliyuncs.com
```

将镜像推送到Registry

```bash
$ sudo docker login --username=穿靴子的狸花猫 registry.cn-shenzhen.aliyuncs.com
$ sudo docker tag [ImageId] registry.cn-shenzhen.aliyuncs.com/xdclassimages/mysql:[镜像版本号]
$ sudo docker push registry.cn-shenzhen.aliyuncs.com/xdclassimages/mysql:[镜像版本号]
```

从Registry中拉取镜像

```bash
$ sudo docker pull registry.cn-shenzhen.aliyuncs.com/xdclassimages/mysql:[镜像版本号]
```

## 企业核心篇幅之harbor仓库搭建

构建自己的镜像仓库 

- 安装之前确保前置条件是否满足，需要安装docker、docker-compose、openssl以及python2.7以上

- 安装 yum -y install openssl 

- Harbor离线版安装下载地址 [Releases · goharbor/harbor · GitHub](https://github.com/goharbor/harbor/releases)

- 上传对应安装包

- 修改配置：harbor.yml

  - 修改主机名（注意空格）：hostname: 192.168.0.151
  - 修改密码（注意空格）：harbor_admin_password: Harbor12345

- 执行脚本，会生成一个文件docker-compose.yml：sh prepare

- 执行安装命令：sh install.sh

- 访问Harbor，默认用户名admin  密码：Harbor12345

  - 查看nginx的端口：docker-compose ps 

  - 访问 192.168.0.151:80

    ![image-20210503211441434](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210503211443.png)

- 关闭：docker-compose down

- 启动：docker-compose up -d

![image-20210503211712530](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210503223357.png)

## 企业核心篇幅之harbor仓库配置与使用

配置与使用harbor仓库 

- Docker配置使用自建仓库 

  - 默认docker只允许访问 https仓库 

  - 如果要访问http仓库需要自己配置 

  - 配置允许访问http仓库：/etc/docker/daemon.json

    ```json
    {
    "insecure-registries":["http://192.168.0.151"]
    }
    ```

  - 重启docker服务：systemctl restart docker.service 

  - 网页上创建项目名 登录：docker login --username=admin 192.168.0.151 

  - 改名：docker tag mysql:5.7 192.168.0.151/xdclass/mysql:5.7 

  - 推送：docker push 192.168.0.151/xdclass/mysql:5.7 

  - 下载：docker pull 192.168.0.151/xdclass/mysql:5.7 

  - docker login 后有一个登录凭证（可删除，下次需要密码）： 

    - 密码存在 /root/.docker/config.json （建议从安全角度出发，每次登录后进行删除）

## 实战系列之本地镜像容器的载入与载出

镜像的本地载入载出

- 两种办法： 
  - 保存镜像 
  - 保存容器
- 保存镜像：
  - docker save cd3ed0dfff7e -o /home/mysql.tar
  - docker save mysql:5.7 > /home/mysql.tar
- 载入镜像：
  - docker load -i mysql.tar
  - docker tag cd3ed0dfff7e mysql:5.7
- 保存容器：
  - docker export 974b919e1fdd -o /home/mysql-export.tar
- 载入容器：
  - docker import mysql-export.tar

