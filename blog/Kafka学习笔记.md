---
title: Kafka学习笔记
categories: kafka
tags: [kafka]
---

# 分布式流处理平台Kafka大纲速览

## 技术技术栈和环境说明

- Kafka版本：2.8.0
- Scala版本：2.13
- Zookeeper版本：3.7
- SpringBoot.2.5 + Maven + IDEA旗舰版 + JDK8 或 JDK11

## 大纲速览

- 高性能分布式流处理平台 Kafka核心知识+项目实战

- Kakfa多种工作模式,点对点/发布订阅模型和应用场景

- 核心概念 生产者、消费者 Broker/Topic/Partition/leader/follower等

- Linux服务器急速部署Zookeeper、Kafka，多种控制台操作指令，分区控制等

- SpringBoot整合Kafka原生多个模块Admin/Producer/Consumer核心API+SpringKafka实战

- 高级篇-Kafka存储流程和原理讲解LEO+HW+Offset

- 高级篇-生产者发送消息模型、分区策略和核心配置实战，自定义分区Key策略等

- 高级篇-消费者消费消息模型、分区策略和重Rebalance实战等

  高级篇-Broker数据文件存储模型-ACK和副本可靠性原理分析+ISR模型

- 高级篇-高可用搭建Zookeeper集群+Kafka集群+SpringBoot项目整合和故障演练

- 高级篇-Kafka高性能原理分析ZeroCopy+多案例事务消息实战+大数据技术栈路线

- Kafka架构+设计思想+底层原理+互联网大厂面试题等

# MQ消息中间件+JMS+AMQP核心知识

## 什么是MQ消息中间件和应用场景

- 什么是MQ消息中间件
  - 全称MessageQueue，主要是用于程序和程序直接通信，异步+解耦
- 使用场景：
  - 核心应用
    - 解耦：订单系统-》物流系统
    - 异步：用户注册-》发送邮件，初始化信息
    - 削峰：秒杀、日志处理
  - 跨平台 、多语言
  - 分布式事务、最终一致性
  - RPC调用上下游对接，数据源变动->通知下属

![image-20210610110754781](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210610110809.png)

## JMS消息服务和和常见核心概念

- 什么是JMS: Java消息服务（Java Message Service),Java平台中关于面向消息中间件的接口

  - JMS是一种与厂商无关的 API，用来访问消息收发系统消息，它类似于JDBC(Java Database Connectivity)。这里，JDBC 是可以用来访问许多不同关系数据库的 API

  - 是由Sun公司早期提出的消息标准，旨在为java应用提供统一的消息操作，包括create、send、receive

  - JMS是针对java的，那微软开发了NMS（.NET消息传递服务）

  - 特性

    - 面向Java平台的标准消息传递API
    - 在Java或JVM语言比如Scala、Groovy中具有互用性
    - 无需担心底层协议
    - 有queues和topics两种消息传递模型
    - 支持事务、能够定义消息格式（消息头、属性和内容）

  - 常见概念

    - JMS提供者：连接面向消息中间件的，JMS接口的一个实现，RocketMQ,ActiveMQ,Kafka等等
    - JMS生产者(Message Producer)：生产消息的服务
    - JMS消费者(Message Consumer)：消费消息的服务
    - JMS消息：数据对象
    - JMS队列：存储待消费消息的区域
    - JMS主题：一种支持发送消息给多个订阅者的机制
    - JMS消息通常有两种类型：点对点（Point-to-Point)、发布/订阅（Publish/Subscribe）

  - 基础编程模型

    - MQ中需要用的一些类
    - ConnectionFactory ：连接工厂，JMS 用它创建连接
    - Connection ：JMS 客户端到JMS Provider 的连接
    - Session： 一个发送或接收消息的线程
    - Destination ：消息的目的地;消息发送给谁.
    - MessageConsumer / MessageProducer： 消息消费者，消息生产者

     

![image-20210610110913772](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210610110915.png)

## AMQP高级消息队列协议和MQTT科普

- 背景
  - JMS或者NMS都没有标准的底层协议，API是与编程语言绑定的，每个消息队列厂商就存在多种不同格式规范的产品，对使用者就产生了很多问题, AMQP解决了这个问题，它使用了一套标准的底层协议
- 什么是AMQP
  - AMQP（advanced message queuing protocol）在2003年时被提出，最早用于解决金融领不同平台之间的消息传递交互问题,就是是一种协议，兼容JMS
  - 更准确说的链接协议 binary- wire-level-protocol 直接定义网络交换的数据格式，类似http
  - 具体的产品实现比较多，RabbitMQ就是其中一种

![image-20210610111444397](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210610111447.png)

- 特性
  - 独立于平台的底层消息传递协议
  - 消费者驱动消息传递
  - 跨语言和平台的互用性、属于底层协议
  - 有5种交换类型direct，fanout，topic，headers，system
  - 面向缓存的、可实现高性能、支持经典的消息队列，循环，存储和转发
  - 支持长周期消息传递、支持事务（跨消息队列）
- AMQP和JMS的主要区别
  - AMQP不从API层进行限定，直接定义网络交换的数据格式,这使得实现了AMQP的provider天然性就是跨平台
  - 比如Java语言产生的消息，可以用其他语言比如python的进行消费
  - AQMP可以用http来进行类比，不关心实现接口的语言，只要都按照相应的数据格式去发送报文请求，不同语言的client可以和不同语言的server进行通讯
  - JMS消息类型：TextMessage/ObjectMessage/StreamMessage等
  - AMQP消息类型：Byte[]
- MQTT
  - MQTT: 消息队列遥测传输（Message Queueing Telemetry Transport ）
  - 背景：
    - 我们有面向基于Java的企业应用的JMS和面向所有其他应用需求的AMQP，那这个MQTT是做啥的？
  - 原因
    - 计算性能不高的设备不能适应AMQP上的复杂操作,MQTT它是专门为小设备设计的
    - MQTT主要是是物联网（IOT）中大量的使用
  - 特性
    - 内存占用低，为小型无声设备之间通过低带宽发送短消息而设计
    - 不支持长周期存储和转发，不允许分段消息（很难发送长消息）
    - 支持主题发布-订阅、不支持事务（仅基本确认）
    - 消息实际上是短暂的（短周期）
    - 简单用户名和密码、不支持安全连接、消息不透明

## 业界主流消息队列和技术选型

- 业界主流的消息队列：Apache ActiveMQ、Kafka、RabbitMQ、RocketMQ

  - ActiveMQ：http://activemq.apache.org/

    - Apache出品，历史悠久，支持多种语言的客户端和协议，支持多种语言Java, .NET, C++ 等
    - 基于JMS Provider的实现

    - 缺点：吞吐量不高，多队列的时候性能下降，存在消息丢失的情况，比较少大规模使用

  - Kafka：http://kafka.apache.org/

    - 是由Apache软件基金会开发的一个开源流处理平台，由Scala和Java编写。Kafka是一种高吞吐量的分布式发布订阅消息系统(严格意义上是不属于队列产品，是一个流处理平台)，它可以处理大规模的网站中的所有动作流数据(网页浏览，搜索和其他用户的行动)，副本集机制，实现数据冗余，保障数据尽量不丢失；支持多个生产者和消费者
    - 类似MQ，功能较为简单，主要支持常规的MQ功能

    - 它提供了类似于JMS的特性，但是在设计实现上完全不同，它并不是JMS规范的实现

    - 缺点：运维难度大，文档比较少, 需要掌握Scala

  - RocketMQ：http://rocketmq.apache.org/
    - 阿里开源的一款的消息中间件, 纯Java开发，具有高吞吐量、高可用性、适合大规模分布式系统应用的特点, 性能强劲(零拷贝技术)，支持海量堆积, 支持指定次数和时间间隔的失败消息重发,支持consumer端tag过滤、延迟消息等，在阿里内部进行大规模使用，适合在电商，互联网金融等领域
    - 基于JMS Provider的实现
    - 缺点：社区相对不活跃，更新比较快，纯java支持

  - RabbitMQ：http://www.rabbitmq.com/
    - 是一个开源的AMQP实现，服务器端用Erlang语言编写，支持多种客户端，如：Python、Ruby、.NET、Java、C、用于在分布式系统中存储转发消息，在易用性、扩展性、高可用性等方面表现不错
    - 缺点：使用Erlang开发，阅读和修改源码难度大

# Kafka核心概念+安装部署实战

## 分布式流处理平台kafka快速认知

Kafka

- Kafka是最初由Linkedin公司开发，Linkedin于2010年贡献给了Apache基金会并成为顶级开源项目，也是一个开源【分布式流处理平台】，由Scala和Java编写，（也当做MQ系统，但不是纯粹的消息系统）

  - open-source distributed event streaming platform

- 核心：一种高吞吐量的分布式流处理平台，它可以处理消费者在网站中的所有动作流数据。

  - 比如 网页浏览，搜索和其他用户的行为等，应用于大数据实时处理领域

- 官网：http://kafka.apache.org/

- 快速开始：http://kafka.apache.org/quickstart

- 快速认识概念

  - Broker
    - Kafka的服务端程序，可以认为一个mq节点就是一个broker
    - broker存储topic的数据
  - Producer生产者
    - 创建消息Message，然后发布到MQ中
    - 该角色将消息发布到Kafka的topic中
  - Consumer消费者:
    - 消费队列里面的消息

  ![Untitled Diagram](https://file.xdclass.net/note/2021/kafka/img/Untitled%20Diagram.png)

## 分布式流处理平台Kafka核心概念

核心概念

- Broker
  - Kafka的服务端程序，可以认为一个mq节点就是一个broker
  - broker存储topic的数据
- Producer生产者
  - 创建消息Message，然后发布到MQ中
  - 该角色将消息发布到Kafka的topic中
- Consumer消费者:
  - 消费队列里面的消息
- ConsumerGroup消费者组
  - 同个topic, 广播发送给不同的group，一个group中只有一个consumer可以消费此消息
- Topic
  - 每条发布到Kafka集群的消息都有一个类别，这个类别被称为Topic，主题的意思
- Partition分区
  - kafka数据存储的基本单元，topic中的数据分割为一个或多个partition，每个topic至少有一个partition，是有序的
  - 一个Topic的多个partitions, 被分布在kafka集群中的多个server上
  - 消费者数量 <=小于或者等于Partition数量
- Replication 副本（备胎）
  - 同个Partition会有多个副本replication ，多个副本的数据是一样的，当其他broker挂掉后，系统可以主动用副本提供服务
  - 默认每个topic的副本都是1（默认是没有副本，节省资源），也可以在创建topic的时候指定
  - 如果当前kafka集群只有3个broker节点，则replication-factor最大就是3了，如果创建副本为4，则会报错
- ReplicationLeader、ReplicationFollower
  - Partition有多个副本，但只有一个replicationLeader负责该Partition和生产者消费者交互
  - ReplicationFollower只是做一个备份，从replicationLeader进行同步
- ReplicationManager
  - 负责Broker所有分区副本信息，Replication 副本状态切换
- offset
  - 每个consumer实例需要为他消费的partition维护一个记录自己消费到哪里的偏移offset
  - kafka把offset保存在消费端的消费者组里
- 特点总结
  - 多订阅者
    - 一个topic可以有一个或者多个订阅者
    - 每个订阅者都要有一个partition，所以订阅者数量要少于等于partition数量
  - 高吞吐量、低延迟: 每秒可以处理几十万条消息
  - 高并发：几千个客户端同时读写
  - 容错性：多副本、多分区，允许集群中节点失败，如果副本数据量为n,则可以n-1个节点失败
  - 扩展性强：支持热扩展
- 基于消费者组可以实现：
  - 基于队列的模型：所有消费者都在同一消费者组里，每条消息只会被一个消费者处理
  - 基于发布订阅模型：消费者属于不同的消费者组，假如每个消费者都有自己的消费者组，这样kafka消息就能广播到所有消费者实例上

## Kafka相关环境准备和安装JDK8

- 需要的软件和环境版本说明

  - kafka-xx-yy
    - xx 是scala版本，yy是kafka版本（scala是基于jdk开发，需要安装jdk环境）
    - 下载地址：http://kafka.apache.org/downloads
  - zookeeper
    - Apache 软件基金会的一个软件项目，它为大型分布式计算提供开源的分布式配置服务、同步服务和命名注册
    - 下载地址：https://zookeeper.apache.org/releases.html
  - jdk1.8

- 步骤

  - 上传安装包（zk、jdk、kafka）

  - 安装jdk

    - 配置全局环境变量

      - 解压：tar -zxvf jdk-8u181-linux-x64.tar.gz

      - 重命名

      - vim /etc/profile

      - 配置

        ```shell
        JAVA_HOME=/usr/local/software/jdk1.8
        CLASSPATH=$JAVA_HOME/lib/
        PATH=$PATH:$JAVA_HOME/bin
        export PATH JAVA_HOME CLASSPATH
        ```

      - 环境变量立刻生效

        - source /etc/profile

    - 查看安装情况 java -version

## Linux环境下Zookeeper和Kafka安装启动

- 安装Zookeeper (默认2181端口)

  - 默认配置文件 zoo.cfg
  - 启动zk
    - bin/zkServer.sh start

- 安装Kafka (默认 9092端口)

  - config目录下 server.properties

    <img src="https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210610113205.png" alt="image-20210610113152510" style="zoom: 67%;" />

    ```properties
    #标识broker编号，集群中有多个broker，则每个broker的编号需要设置不同
    broker.id=0
    #修改下面两个配置 ( listeners 配置的ip和advertised.listeners相同时启动kafka会报错)
    listeners(内网Ip)
    advertised.listeners(公网ip)
    #修改zk地址,默认地址
    zookeeper.connection=localhost:2181
    ```

  - bin目录启动和停止

    ```bash
    #启动
    ./kafka-server-start.sh  ../config/server.properties &
    #停止
    kafka-server-stop.sh
    ```

  - 创建topic

    ```bash
    ./kafka-topics.sh --create --zookeeper 101.132.252.118:2181 --replication-factor 1 --partitions 1 --topic xdclass-topic
    ```

  - 查看topic

    ```bash
    ./kafka-topics.sh --list --zookeeper 101.132.252.118:2181
    ```

  -  Linux环境下daemon守护进程运行Kafka

    ```bash
    ./kafka-server-start.sh -daemon ../config/server.properties &
    ```

# Kafka点对点-发布订阅模型讲解和写入存储流程实战

##  Kafka命令行生产者发送消息和消费者消费消息实战

- 创建topic

  ```bash
  cd /usr/local/software/kafka/bin
  ./kafka-topics.sh --create --zookeeper 101.132.252.118:2181 --replication-factor 1 --partitions 2 --topic xdclass-topic
  ```

- 查看topic

  ```bash
  ./kafka-topics.sh --list --zookeeper 101.132.252.118:2181
  ```

- 生产者发送消息

  ```bash
  ./kafka-console-producer.sh --broker-list 101.132.252.118:9092  --topic version1-topic
  ```

- 消费者消费消息 ( --from-beginning：会把主题中以往所有的数据都读取出来, 重启后会有这个重复消费）

  ```bash
  ./kafka-console-consumer.sh --bootstrap-server 101.132.252.118:9092 --from-beginning --topic version1-topic
  ```

  ![image-20210610114445032](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210610114448.png)

- 删除topic

  ```bash
  ./kafka-topics.sh --zookeeper 101.132.252.118:2181 --delete --topic t1
  ```

- 查看broker节点topic状态信息

  ```bash
  ./kafka-topics.sh --describe --zookeeper 101.132.252.118:2181  --topic xdclass-topic
  ```


## Kafka点对点模型和发布订阅模型

- JMS规范目前支持两种消息模型
  - 点对点（point to point)
    - 消息生产者生产消息发送到queue中，然后消息消费者从queue中取出并且消费消息
    - 消息被消费以后，queue中不再有存储，所以消息消费者不可能消费到已经被消费的消息。 Queue支持存在多个消费者，但是对一个消息而言，只会有一个消费者可以消费
  - 发布/订阅（publish/subscribe）
    - 消息生产者（发布）将消息发布到topic中，同时有多个消息消费者（订阅）消费该消息。
    - 和点对点方式不同，发布到topic的消息会被所有订阅者消费。

![image-20210610130417875](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210610130420.png)

## Kafka消费者组配置实现点对点消费模型

- 编辑消费者配置（确保同个名称group.id一样）

  - 编辑 config/consumer.properties

- 创建topic, 1个分区

  ```bash
  ./kafka-topics.sh --create --zookeeper 112.74.55.160:2181 --replication-factor 1 --partitions 2 --topic xdclass-topic
  ```

- 指定配置文件启动 两个消费者

  ```bash
  ./kafka-console-consumer.sh --bootstrap-server 101.132.252.118:9092 --from-beginning --topic xdclass-topic --consumer.config ../config/consumer.properties
  ```

- 现象

  - 只有一个消费者可以消费到数据，一个分区只能被同个消费者组下的某个消费者进行消费

<img src="https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210610132352.png" alt="image-20210610132337485" style="zoom: 67%;" />

## Kafka消费者组配置实现发布订阅消费模型

- 编辑消费者配置（确保group.id 不一样）

  - cp config/consumer.properties config/consumer-1.properties
  - cp config/consumer.properties config/consumer-2.properties
  - 编辑 config/consumer-1.properties
  - 编辑 config/consumer-2.properties

- 创建topic, 2个分区

  ```bash
  ./kafka-topics.sh --create --zookeeper 101.132.252.118:2181 --replication-factor 1 --partitions 1--topic xdclass-topic
  ```

- 生产者生产消息

  ```bash
  ./kafka-console-producer.sh --broker-list 101.132.252.118:9092  --topic xdclass-topic
  ```

- 指定配置文件启动两个消费者

  ```bash
  ./kafka-console-consumer.sh --bootstrap-server 101.132.252.118:9092 --from-beginning --topic xdclass-topic --consumer.config ../config/consumer1.properties
  
  ./kafka-console-consumer.sh --bootstrap-server 101.132.252.118:9092 --from-beginning --topic xdclass-topic --consumer.config ../config/consumer2.properties
  ```

- 现象
  - 两个不同消费者组的节点，都可以消费到消息，实现发布订阅模型

![image-20210610134550679](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613152149.png)

## Kafka数据存储流程和原理概述和LEO+HW

**Partition**

- topic物理上的分组，一个topic可以分为多个partition，每个partition是一个有序的队列
- 是以文件夹的形式存储在具体Broker本机上

![image-20210613152452852](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613152455.png)

**LEO（LogEndOffset）**

- 表示每个partition的log最后一条Message的位置。

**HW（HighWatermark）**

- 表示partition各个replicas数据间同步且一致的offset位置，即表示allreplicas已经commit的位置
- HW之前的数据才是Commit后的，对消费者才可见
- ISR集合里面最小leo

![image-20210613152600594](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613152603.png)

**offset**：

- 每个partition都由一系列有序的、不可变的消息组成，这些消息被连续的追加到partition中
- partition中的每个消息都有一个连续的序列号叫做offset，用于partition唯一标识一条消息
- 可以认为offset是partition中Message的id

**Segment**：每个partition又由多个segment file组成；

- segment file 由2部分组成，分别为index file和data file（log file），
- 两个文件是一一对应的，后缀”.index”和”.log”分别表示索引文件和数据文件
- 命名规则：partition的第一个segment从0开始，后续每个segment文件名为上一个segment文件最后一条消息的offset+1

**Kafka高效文件存储设计特点：**

- Kafka把topic中一个parition大文件分成多个小文件段，通过多个小文件段，就容易定期清除或删除已经消费完文件，减少磁盘占用。
- 通过索引信息可以快速定位message
- producer生产数据，要写入到log文件中，写的过程中一直追加到文件末尾，为顺序写，官网数据表明。同样的磁盘，顺序写能到600M/S，而随机写只有100K/S

# SpringBoot2.X项目整合-Kafka核心API-Admin实战

## **SpringBoot2.X项目搭建整合Kafka客户端依赖配置**

新版SpringBoot2.X介绍

- 官网：https://spring.io/projects/spring-boot
- GitHub地址：https://github.com/spring-projects/spring-boot
- 官方文档：https://spring.io/guides/gs/spring-boot/
- 视频地址：https://item.taobao.com/item.htm?id=618384570391

相关软件环境和作用

- JDK1.8+以上
- Maven3.5+
- 编辑器IDEA(旗舰版)

在线创建 ：https://start.spring.io/

- 注意：
  - 采用springboot2.5 + jdk11
  - 初次导入项目下载包比较慢 5~20分钟不等
    - 出问题的话: mvn clean install 试试
  - 不建议修改默认maven仓库（可以先还原默认的，防止下载包失败）
  - idea记得配置jdk11

在SpringBoot整合kafka很简单

- 添加依赖 kafka-clients

  ```xml
  <dependency>
        <groupId>org.apache.kafka</groupId>
        <artifactId>kafka-clients</artifactId>
        <version>2.4.0</version>
  </dependency>
  ```

## SpringBoot2.x整合Kafka客户端+adminApi单元测试

单元测试配置客户端+创建topic

```java
/**
 * 设置admin 客户端
 * @return
 */
public static AdminClient initAdminClient(){
    Properties properties = new Properties();
    properties.setProperty(AdminClientConfig.BOOTSTRAP_SERVERS_CONFIG,"101.132.252.118:9092");

    AdminClient adminClient = AdminClient.create(properties);
    return adminClient;
}
//创建
@Test
public  void createTopic() {
    AdminClient adminClient = initAdminClient();
    // 2个分区，1个副本
    NewTopic newTopic = new NewTopic(TOPIC_NAME, 2 , (short) 1);

    CreateTopicsResult createTopicsResult = adminClient.createTopics(Arrays.asList(newTopic));
    //future等待创建，成功不会有任何报错，如果创建失败和超时会报错。
    try {
        createTopicsResult.all().get();
    } catch (Exception e) {
        e.printStackTrace();
    }
    System.out.println("创建新的topic");
}
```

查看topic

```bash
./kafka-topics.sh --list --zookeeper 101.132.252.118:2181
```

查看broker节点topic状态信息

```bash
./kafka-topics.sh --describe --zookeeper 101.132.252.118:2181  --topic xdclass-sp-topic-test
```

## Kafka使用JavaAPI-AdminClient删除和列举topic

list列举

```java
//获取
@Test
public  void listTopic() throws ExecutionException, InterruptedException {
    AdminClient adminClient = initAdminClient();

    //是否查看内部的topic,可以不用
    ListTopicsOptions options = new ListTopicsOptions();
    options.listInternal(true);

    ListTopicsResult listTopics = adminClient.listTopics(options);
    Set<String> topics = listTopics.names().get();
    for (String topic : topics) {
        System.err.println(topic);

    }
}
```

删除

```java
//删除
@Test
public  void delTopicTest() {
    AdminClient adminClient = initAdminClient();
    DeleteTopicsResult deleteTopicsResult = adminClient.deleteTopics(Arrays.asList("xdclass-sp11-topic"));
    try {
        deleteTopicsResult.all().get();
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

## AdminClientApi查看Topic详情和增加分区数量

查看topic详情

```java
//获取指定topic的详细信息
@Test
public  void getTopicInfo() throws Exception {
    AdminClient adminClient = initAdminClient();
    DescribeTopicsResult describeTopicsResult = adminClient.describeTopics(Arrays.asList(TOPIC_NAME));

    Map<String, TopicDescription> stringTopicDescriptionMap = describeTopicsResult.all().get();

    Set<Map.Entry<String, TopicDescription>> entries = stringTopicDescriptionMap.entrySet();

    entries.stream().forEach((entry)-> System.out.println("name ："+entry.getKey()+" , desc: "+ entry.getValue()));
}
```

增加分区数量

```java
 /**
     * 增加分区数量
     *
     * 如果当主题中的消息包含有key时(即key不为null)，根据key来计算分区的行为就会有所影响消息顺序性
     *
     * 注意：Kafka中的分区数只能增加不能减少，减少的话数据不知怎么处理
     *
     * @throws Exception
     */
@Test
public  void incrPartitionsTest() throws Exception{
    Map<String, NewPartitions> infoMap = new HashMap<>();
    NewPartitions newPartitions = NewPartitions.increaseTo(5);

    AdminClient adminClient = initAdminClient();
    infoMap.put(TOPIC_NAME, newPartitions);

    CreatePartitionsResult createPartitionsResult = adminClient.createPartitions(infoMap);
    createPartitionsResult.all().get();
}
```

# Kafka核心API生产者实战

## 生产者发送到Broker分区策略和常见配置

生产者发送到broker里面的流程是怎样的呢，一个 topic 有多个 partition分区，每个分区又有多个副本

- 如果指定Partition ID,则PR被发送至指定Partition (ProducerRecord)
- 如果未指定Partition ID,但指定了Key, PR会按照hash(key)发送至对应Partition
- 如果未指定Partition ID也没指定Key，PR会按照默认 round-robin轮训模式发送到每个Partition
  - 消费者消费partition分区默认是range模式
- 如果同时指定了Partition ID和Key, PR只会发送到指定的Partition (Key不起作用，代码逻辑决定)
- 注意：Partition有多个副本，但只有一个replicationLeader负责该Partition和生产者消费者交互

生产者到broker发送流程

- Kafka的客户端发送数据到服务器，不是来一条就发一条，会经过内存缓冲区（默认是16KB），通过KafkaProducer发送出去的消息都是先进入到客户端本地的内存缓冲里，然后把很多消息收集到的Batch里面，再一次性发送到Broker上去的，这样性能才可能题高

生产者常见配置

- 官方文档 http://kafka.apache.org/documentation/#producerconfigs

```properties
#kafka地址,即broker地址
bootstrap.servers  

#当producer向leader发送数据时，可以通过request.required.acks参数来设置数据可靠性的级别,分别是0, 1，all。
acks

#请求失败，生产者会自动重试，指定是0次，如果启用重试，则会有重复消息的可能性
retries

#每个分区未发送消息总字节大小,单位：字节，超过设置的值就会提交数据到服务端，默认值是16KB
batch.size

# 默认值就是0，消息是立刻发送的，即便batch.size缓冲空间还没有满，如果想减少请求的数量，可以设置 linger.ms 大于#0，即消息在缓冲区保留的时间，超过设置的值就会被提交到服务端
# 通俗解释是，本该早就发出去的消息被迫至少等待了linger.ms时间，相对于这时间内积累了更多消息，批量发送 减少请求
#如果batch被填满或者linger.ms达到上限，满足其中一个就会被发送
linger.ms

# buffer.memory的用来约束Kafka Producer能够使用的内存缓冲的大小的，默认值32MB。
# 如果buffer.memory设置的太小，可能导致消息快速的写入内存缓冲里，但Sender线程来不及把消息发送到Kafka服务器
# 会造成内存缓冲很快就被写满，而一旦被写满，就会阻塞用户线程，不让继续往Kafka写消息了
# buffer.memory要大于batch.size，否则会报申请内存不足的错误，不要超过物理内存，根据实际情况调整
buffer.memory

# key的序列化器，将用户提供的 key和value对象ProducerRecord 进行序列化处理，key.serializer必须被设置，即使
#消息中没有指定key，序列化器必须是一个实现org.apache.kafka.common.serialization.Serializer接口的类，将#key序列化成字节数组。
key.serializer
value.serializer 
```

## Kafka核心API模块-producer API实战

封装配置属性

```java
public static Properties getProperties(){
        Properties props = new Properties();

        props.put("bootstrap.servers", "101.132.252.118:9092");
        //props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "101.132.252.118:9092");
        // 当producer向leader发送数据时，可以通过request.required.acks参数来设置数据可靠性的级别,分别是0, 1，all。
        props.put("acks", "all");
        //props.put(ProducerConfig.ACKS_CONFIG, "all");

        // 请求失败，生产者会自动重试，指定是0次，如果启用重试，则会有重复消息的可能性
        props.put("retries", 0);
        //props.put(ProducerConfig.RETRIES_CONFIG, 0);

        // 生产者缓存每个分区未发送的消息,缓存的大小是通过 batch.size 配置指定的，默认值是16KB
        props.put("batch.size", 16384);


        /**
         * 默认值就是0，消息是立刻发送的，即便batch.size缓冲空间还没有满
         * 如果想减少请求的数量，可以设置 linger.ms 大于0，即消息在缓冲区保留的时间，超过设置的值就会被提交到服务端
         * 通俗解释是，本该早就发出去的消息被迫至少等待了linger.ms时间，相对于这时间内积累了更多消息，批量发送减少请求
         * 如果batch被填满或者linger.ms达到上限，满足其中一个就会被发送
         */
        props.put("linger.ms", 1);

        /**
         * buffer.memory的用来约束Kafka Producer能够使用的内存缓冲的大小的，默认值32MB。
         * 如果buffer.memory设置的太小，可能导致消息快速的写入内存缓冲里，但Sender线程来不及把消息发送到Kafka服务器
         * 会造成内存缓冲很快就被写满，而一旦被写满，就会阻塞用户线程，不让继续往Kafka写消息了
         * buffer.memory要大于batch.size，否则会报申请内存不#足的错误，不要超过物理内存，根据实际情况调整
         * 需要结合实际业务情况压测进行配置
         */
        props.put("buffer.memory", 33554432);
        /**
         * key的序列化器，将用户提供的 key和value对象ProducerRecord 进行序列化处理，key.serializer必须被设置，
         * 即使消息中没有指定key，序列化器必须是一个实现org.apache.kafka.common.serialization.Serializer接口的类，
         * 将key序列化成字节数组。
         */
        props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
        props.put("value.serializer","org.apache.kafka.common.serialization.StringSerializer");

        return props;
    }
```

生产者投递消息API实战（同步发送）

```Java
 /**
     * send()方法是异步的，添加消息到缓冲区等待发送，并立即返回
     * 生产者将单个的消息批量在一起发送来提高效率,即 batch.size和linger.ms结合
     *
     * 实现同步发送：一条消息发送之后，会阻塞当前线程，直至返回 ack
     * 发送消息后返回的一个 Future 对象，调用get即可
     *
     * 消息发送主要是两个线程：一个是Main用户主线程，一个是Sender线程
     *  1)main线程发送消息到RecordAccumulator即返回
     *  2)sender线程从RecordAccumulator拉取信息发送到broker
     *  3)batch.size和linger.ms两个参数可以影响 sender 线程发送次数
     *
     *
     */
@Test
public void testSend(){

    Properties props = getProperties();
    Producer<String, String> producer = new KafkaProducer<>(props);
    for (int i = 1; i < 3; i++){
        Future<RecordMetadata>  future = producer.send(new ProducerRecord<>("my-topic", "xdclass-key"+i, "xdclass-value"+i));
        try {
            RecordMetadata recordMetadata = future.get();//不关心是否发送成功，则不需要这行
            System.out.println("发送状态："+recordMetadata.toString());

        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
        System.out.println(i+"发送："+LocalDateTime.now().toString());
    }
    producer.close();
}
```

## 【面试】 ProducerRecord介绍和key的作用

ProducerRecord（简称PR）：发送给Kafka Broker的key/value 值对, 封装基础数据信息

```
-- Topic （名字）
-- PartitionID (可选)
-- Key(可选)
-- Value
```

![image-20210613162104977](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613162107.png)

![image-20210613162116729](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613162119.png)

key默认是null，大多数应用程序会用到key

- 如果key为空，kafka使用默认的partitioner，使用RoundRobin算法将消息均衡地分布在各个partition上
- 如果key不为空，kafka使用自己实现的hash方法对key进行散列，决定消息该被写到Topic的哪个partition，拥有相同key的消息会被写到同一个partition，实现顺序消息

## Kafka核心API模块-producerAPI回调函数实战

- 生产者发送消息是异步调用，怎么知道是否有异常？
  - 发送消息配置回调函数即可， 该回调方法会在 Producer 收到 ack 时被调用，为异步调用
  - 回调函数有两个参数 RecordMetadata 和 Exception，如果 Exception 是 null，则消息发送成功，否则失败
- 异步发送配置回调函数

```Java
@Test
public void testSendWithCallback(){
    Properties props = getProperties();
    Producer<String, String> producer = new KafkaProducer<>(props);
    for (int i = 1; i < 3; i++){
        producer.send(new ProducerRecord<>("my-topic", "xdclass-key" + i, "xdclass-value" + i), new Callback() {
            @Override
            public void onCompletion(RecordMetadata metadata, Exception exception) {
                if (exception == null) {
                    System.out.println("发送状态："+metadata.toString());
                } else {
                    exception.printStackTrace();
                }
            }
        });
        System.out.println(i+"发送："+LocalDateTime.now().toString());
    }
    producer.close();
}

```

## producer生产者发送指定分区实战

创建topic，配置5个分区，1个副本

发送代码

```Java
@Test
public void testSendWithCallbackAndPartition(){
    Properties props = getProperties();
    Producer<String, String> producer = new KafkaProducer<>(props);
    for (int i = 0; i < 6; i++){
        producer.send(new ProducerRecord<>("my-topic",i, "xdclass-key" + i, "xdclass-value" + i), new Callback() {
            @Override
            public void onCompletion(RecordMetadata metadata, Exception exception) {
                if (exception == null) {
                    System.out.println("发送状态："+metadata.toString());
                } else {
                    exception.printStackTrace();
                }
            }
        });
        System.out.println(i+"发送："+LocalDateTime.now().toString());
    }
    producer.close();

}
```

## Kafka生产者自定义partition分区规则实战

源码解读默认分区器

```
org.apache.kafka.clients.producer.internals.DefaultPartitioner
```

自定义分区规则

- 创建类，实现Partitioner接口，重写方法
- 配置 partitioner.class 指定类即可

```java
public class XdclassPartitioner implements Partitioner {

    @Override
    public int partition(String topic, Object key, byte[] keyBytes, Object value, byte[] valueBytes, Cluster cluster) {
        List<PartitionInfo> partitions = cluster.partitionsForTopic(topic);
        int numPartitions = partitions.size();

        //返回编号为0的分区
        if("xdclass".equals(key)) {
            return 0;
        }
        //使用hash值取模，确定分区(默认的也是这个方式)
        return Utils.toPositive(Utils.murmur2(keyBytes)) % numPartitions;
    }
    @Override
    public void close() {

    }

    @Override
    public void configure(Map<String, ?> configs) {

    }
}
@Test
public void testSend(){
    Properties props = getProperties();
    //自定义partition分区规则
    props.put("partitioner.class", "net.xdclass.xdclassredis.XdclassPartitioner");
    Producer<String, String> producer = new KafkaProducer<>(props);
    for (int i = 0; i < 3; i++){
        Future<RecordMetadata>  future = producer.send(new ProducerRecord<>(TOPIC_NAME, "xdclass", "xdclass-value"+i));
        try {
            RecordMetadata recordMetadata = future.get();
            System.out.println("发送状态："+recordMetadata.toString());
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
        System.out.println(i+"发送："+LocalDateTime.now().toString());
    }
    producer.close();
}
```

# Kafka核心API消费者模块实战

## 【面试】Consumer消费者机制和分区策略

消费者根据什么模式从broker获取数据的？

为什么是pull模式，而不是broker主动push？

- 消费者采用 pull 拉取方式，从broker的partition获取数据
- pull 模式则可以根据 consumer 的消费能力进行自己调整，不同的消费者性能不一样
  - 如果broker没有数据，consumer可以配置 timeout 时间，阻塞等待一段时间之后再返回
- 如果是broker主动push，优点是可以快速处理消息，但是容易造成消费者处理不过来，消息堆积和延迟。

消费者从哪个分区进行消费？

- 一个 topic 有多个 partition，一个消费者组里面有多个消费者，那是怎么分配?
  - 一个主题topic可以有多个消费者，因为里面有多个partition分区 ( leader分区)
  - 一个partition leader可以由一个消费者组中的一个消费者进行消费
  - 一个 topic 有多个 partition，所以有多个partition leader，给多个消费者消费，那分配策略如何？

消费者从哪个分区进行消费？两个策略

- 顶层接口

```
org.apache.kafka.clients.consumer.internals.AbstractPartitionAssignor
```

策略一：round-robin （**RoundRobinAssignor**非默认策略）轮训

- 【按照消费者组】进行轮训分配，同个消费者组监听不同主题也一样，是把所有的 partition 和所有的 consumer 都列出来， 所以消费者组里面订阅的主题是一样的才行，主题不一样则会出现分配不均问题，例如7个分区，同组内2个消费者
- topic-p0/topic-p1/topic-p2/topic-p3/topic-p4/topic-p5/topic-p6
- c-1: topic-p0/topic-p2/topic-p4/topic-p6
- c-2:topic-p1/topic-p3/topic-p5
- 弊端
  - 如果同一消费者组内，所订阅的消息是不相同的，在执行分区分配的时候不是轮询分配，可能会导致分区分配的不均匀
  - 有3个消费者C0、C1和C2，他们共订阅了 3 个主题：t0、t1 和 t2
  - t0有1个分区(p0)，t1有2个分区(p0、p1)，t2有3个分区(p0、p1、p2))
  - 消费者C0订阅的是主题t0，消费者C1订阅的是主题t0和t1，消费者C2订阅的是主题t0、t1和t2

![image-20210613163948757](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613163952.png)

策略二：range （**RangeAssignor**默认策略）范围

- 【按照主题】进行分配，如果不平均分配，则第一个消费者会分配比较多分区， 一个消费者监听不同主题也不影响，例如7个分区，同组内2个消费者
- topic-p0/topic-p1/topic-p2/topic-p3/topic-p4/topic-p5//topic-p6
- c-1: topic-p0/topic-p1/topic-p2/topic-p3
- c-2:topic-p4/topic-p5/topic-p6
- 弊端
  - 只是针对 1 个 topic 而言，c-1多消费一个分区影响不大
  - 如果有 N 多个 topic，那么针对每个 topic，消费者 C-1 都将多消费 1 个分区，topic越多则消费的分区也越多，则性能有所下降

## 【面试】Consumer重新分配策略和offset维护机制

什么是Rebalance操作？

- kafka 怎么均匀地分配某个 topic 下的所有 partition 到各个消费者，从而使得消息的消费速度达到最快，这就是平衡（balance），前面讲了 Range 范围分区 和 RoundRobin 轮询分区，也支持自定义分区策略。
- 而 rebalance（重平衡）其实就是重新进行 partition 的分配，从而使得 partition 的分配重新达到平衡状态

面试：例如70个分区，10个消费者，但是先启动一个消费者，后续再启动一个消费者，这个会怎么分配？

- Kafka 会进行一次分区分配操作，即 Kafka 消费者端的 Rebalance 操作 ，下面都会发生rebalance操作
  - 当消费者组内的消费者数量发生变化（增加或者减少），就会产生重新分配patition
  - 分区数量发生变化时(即 topic 的分区数量发生变化时)

面试：当消费者在消费过程突然宕机了，重新恢复后是从哪里消费，会有什么问题？

- 消费者会记录offset，故障恢复后从这里继续消费，这个offset记录在哪里？
- 记录在zk里面和本地，新版默认将offset保证在kafka的内置topic中，名称是 __consumer_offsets
  - 该Topic默认有50个Partition，每个Partition有3个副本，分区数量由参数offset.topic.num.partition配置
  - 通过groupId的哈希值和该参数取模的方式来确定某个消费者组已消费的offset保存到__consumer_offsets主题的哪个分区中
  - 由 消费者组名+主题+分区，确定唯一的offset的key，从而获取对应的值
  - 三元组：**group.id+topic+分区号**，而 value 就是 offset 的值

## Consumer配置和Kafka调试日志配置

springboot关闭kafka调试日志

1. yml配置文件修改

```yml
logging:
  config: classpath:logback.xml
```

2. logback.xml内容

```xml
<configuration>
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <!-- 格式化输出： %d表示日期， %thread表示线程名， %-5level: 级别从左显示5个字符宽度 %msg:日志消息, %n是换行符 -->
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS}[%thread] %-5level %logger{50} - %msg%n</pattern>
        </encoder>
    </appender>
    <root level="info">
        <appender-ref ref="STDOUT" />
    </root>
</configuration>
```

消费者配置

```properties
#消费者分组ID，分组内的消费者只能消费该消息一次，不同分组内的消费者可以重复消费该消息
group.id

#为true则自动提交偏移量
enable.auto.commit

#自动提交offset周期
auto.commit.interval.ms

#重置消费偏移量策略，消费者在读取一个没有偏移量的分区或者偏移量无效情况下（因消费者长时间失效、包含偏移量的记录已经过时并被删除）该如何处理，
#默认是latest，如果需要从头消费partition消息，需要改为 earliest 且消费者组名变更 才可以
auto.offset.reset

#序列化器
key.deserializer
```

 ## Kafka消费者Consumer消费消息配置实战

配置

```java
public static Properties getProperties() {
        Properties props = new Properties();

        //broker地址
        props.put("bootstrap.servers", "101.132.252.118:9092");

        //消费者分组ID，分组内的消费者只能消费该消息一次，不同分组内的消费者可以重复消费该消息
        props.put("group.id", "xdclass-g1");

        //开启自动提交offset
        props.put("enable.auto.commit", "true");

        //自动提交offset延迟时间
        props.put("auto.commit.interval.ms", "1000");

        //反序列化
        props.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
        props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");

        return props;
    }
```

消费订阅

```java
@Test
public void simpleConsumerTest(){
    Properties props = getProperties();
    KafkaConsumer<String, String> consumer = new KafkaConsumer<>(props);

    //订阅topic主题
    consumer.subscribe(Arrays.asList(KafkaProducerTest.TOPIC_NAME));

    while (true) {
        //拉取时间控制，阻塞超时时间
        ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(500));
        for (ConsumerRecord<String, String> record : records) {
            System.err.printf("topic = %s, offset = %d, key = %s, value = %s%n",record.topic(), record.offset(), record.key(), record.value());
        }
    }
}
```

## Consumer从头消费配置和手工提交offset配置

如果需要从头消费partition消息，怎么操作？

- auto.offset.reset 配置策略即可
- 默认是latest，需要改为 earliest 且消费者组名变更 ，即可实现从头消费

```Java
//默认是latest，如果需要从头消费partition消息，需要改为 earliest 且消费者组名变更，才生效 
props.put("auto.offset.reset","earliest");
```

自动提交offset问题

- 没法控制消息是否正常被消费
- 适合非严谨的场景，比如日志收集发送

手工提交offset配置和测试

- 初次启动消费者会请求broker获取当前消费的offset值

手工提交offset

- 同步 commitSync 阻塞当前线程 (自动失败重试）
- 异步 commitAsync 不会阻塞当前线程 (没有失败重试，回调callback函数获取提交信息，记录日志)

```Java
@Test
public void simpleConsumerTest(){
    Properties props = getProperties();
    KafkaConsumer<String, String> consumer = new KafkaConsumer<>(props);

    //订阅topic主题
    consumer.subscribe(Arrays.asList(KafkaProducerTest.TOPIC_NAME));

    while (true) {
        //拉取时间控制，阻塞超时时间
        ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(500));
        for (ConsumerRecord<String, String> record : records) {
            System.err.printf("topic = %s, offset = %d, key = %s, value = %s%n",record.topic(), record.offset(), record.key(), record.value());
        }
        if(!records.isEmpty()){
            //异步 commitAsync 手工提交
            consumer.commitAsync(new OffsetCommitCallback() {
                @Override
                public void onComplete(Map<TopicPartition, OffsetAndMetadata> offsets, Exception exception) {
                    if(exception == null){
                        System.err.println("手工提交offset成功"+offsets.toString());
                    }else {
                        System.err.println("手工提交offset失败"+offsets.toString());
                    }
                }
            });
        }
    }
}
```

# kafka数据文件存储-可靠性保证-ISR核心知识

## Kafka数据存储流程和log日志

- Kafka 采取了**分片**和**索引**机制，将每个partition分为多个segment，每个segment对应2个文件 log 和 index
- 新增备注

```
index文件中并没有为每一条message建立索引，采用了稀疏存储的方式
每隔一定字节的数据建立一条索引，避免了索引文件占用过多的空间和资源，从而可以将索引文件保留到内存中
缺点是没有建立索引的数据在查询的过程中需要小范围内的顺序扫描操作。
```

![image-20210613165723685](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613165726.png)

配置文件 server.properties

```properties
# The maximum size of a log segment file. When this size is reached a new log segment will be created. 默认是1G,当log数据文件大于1g后，会创建一个新的log文件（即segment，包括index和log）
log.segment.bytes=1073741824
```

![image-20210613170738940](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613170743.png)

例子

```
#分段一
00000000000000000000.index  00000000000000000000.log
#分段二 数字 1234指的是当前文件的最小偏移量offset，即上个文件的最后一个消息的offset+1
00000000000000001234.index  00000000000000001234.log
#分段三
00000000000000088888.index  00000000000000088888.log
```

## 【核心】分布式系统的CAP理论

- CAP定理: 指的是在一个分布式系统中，Consistency（一致性）、 Availability（可用性）、Partition tolerance（分区容错性），三者不可同时获得
  - 一致性（C）：所有节点都可以访问到最新的数据；锁定其他节点，不一致之前不可读
  - 可用性（A）：每个请求都是可以得到响应的，不管请求是成功还是失败；被节点锁定后 无法响应
  - 分区容错性（P）：除了全部整体网络故障，其他故障都不能导致整个系统不可用,；节点间通信可能失败，无法避免
- CAP理论就是说在分布式存储系统中，最多只能实现上面的两点。而由于当前的网络硬件肯定会出现延迟丢包等问题，所以分区容忍性是我们必须需要实现的。所以我们只能在一致性和可用性之间进行权衡

![image-20210613170936248](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613170939.png)

```
CA： 如果不要求P（不允许分区），则C（强一致性）和A（可用性）是可以保证的。但放弃P的同时也就意味着放弃了系统的扩展性，也就是分布式节点受限，没办法部署子节点，这是违背分布式系统设计的初衷的

CP: 如果不要求A（可用），每个请求都需要在服务器之间保持强一致，而P（分区）会导致同步时间无限延长(也就是等待数据同步完才能正常访问服务)，一旦发生网络故障或者消息丢失等情况，就要牺牲用户的体验，等待所有数据全部一致了之后再让用户访问系统

AP：要高可用并允许分区，则需放弃一致性。一旦分区发生，节点之间可能会失去联系，为了高可用，每个节点只能用本地数据提供服务，而这样会导致全局数据的不一致性。
```

结论：

- 分布式系统中P,肯定要满足，所以只能在CA中二选一
- 没有最好的选择，最好的选择是根据业务场景来进行架构设计
- CP ： 适合支付、交易类，要求数据强一致性，宁可业务不可用，也不能出现脏数据
- AP: 互联网业务，比如信息流架构，不要求数据强一致，更想要服务可用

## Kafka数据可靠性保证原理之副本Replica+ACK

- 背景
  - Kafka之间副本数据同步是怎样的？一致性怎么保证，数据怎样保证不丢失呢
- kafka的副本（replica）
  - topic可以设置有N个副本, 副本数最好要小于broker的数量
  - 每个分区有1个leader和0到多个follower，我们把多个replica分为Learder replica和follower replica

![image-20210613171204652](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613171206.png)

生产者发送数据流程

- 保证producer 发送到指定的 topic， topic 的每个 partition 收到producer 发送的数据后
- 需要向 producer 发送 ack 确认收到，如果producer 收到 ack， 就会进行下一轮的发送否则重新发送数据

问题点：Partition什么时间发送ack确认机制（要追求高吞吐量，那么就要放弃可靠性）

 当producer向leader发送数据时，可以通过request.required.acks参数来设置数据可靠性的级别

- 副本数据同步策略 , ack有3个可选值，分别是0, 1，all。

  - ack=0

    - producer发送一次就不再发送了，不管是否发送成功
    - 发送出去的消息还在半路，或者还没写入磁盘， Partition Leader所在Broker就直接挂了，客户端认为消息发送成功了，此时就会导致这条消息就丢失

  - ack=1(默认)

    - 只要Partition Leader接收到消息而且写入【本地磁盘】，就认为成功了，不管他其他的Follower有没有同步过去这条消息了
    - 问题：万一Partition Leader刚刚接收到消息，Follower还没来得及同步过去，结果Leader所在的broker宕机了

  - ack= all（即-1）

    - producer只有收到分区内所有副本的成功写入全部落盘的通知才认为推送消息成功

    - 备注：leader会维持一个与其保持同步的replica集合，该集合就是ISR，leader副本也在isr里面
    - 问题一：如果在follower同步完成后，broker发送ack之前，leader发生故障，那么会造成数据重复
      - 数据发送到leader后 ，部分ISR的副本同步，leader此时挂掉。比如follower1和follower2都有可能变成新的leader, producer端会得到返回异常，producer端会重新发送数据，数据可能会重复
    - 问题二：acks=all 就可以代表数据一定不会丢失了吗
      - Partition只有一个副本，也就是一个Leader，任何Follower都没有
      - 接收完消息后宕机，也会导致数据丢失，acks=all，必须跟ISR列表里至少有2个以上的副本配合使用
      - 在设置request.required.acks=-1的同时，也要min.insync.replicas这个参数设定 ISR中的最小副本数是多少，默认值为1，改为 >=2，如果ISR中的副本数少于min.insync.replicas配置的数量时，客户端会返回异常

## Kafka的in-sync replica set机制

- 什么是ISR (**in-sync replica set** )
  - leader会维持一个与其保持同步的replica集合，该集合就是ISR，每一个leader partition都有一个ISR，leader动态维护, 要保证kafka不丢失message，就要保证ISR这组集合存活（至少有一个存活），并且消息commit成功
  - Partition leader 保持同步的 Partition Follower 集合, 当 ISR 中的Partition Follower 完成数据的同步之后，就会给 leader 发送 ack
  - 如果Partition follower长时间(replica.lag.time.max.ms) 未向leader同步数据，则该Partition Follower将被踢出ISR
  - Partition Leader 发生故障之后，就会从 ISR 中选举新的 Partition Leader。
- OSR （out-of-sync-replica set）：与leader副本分区 同步滞后过多的副本集合
- AR（Assign Replicas）：分区中所有副本统称为AR

## Kafka的HighWatermark的作用

- 背景 broker故障后
  - ACK保障了【生产者】的投递可靠性
  - partition的多副本保障了【消息存储】的可靠性
  - 备注：重复消费问题需要消费者自己处理
- HW作用：保证消费数据的一致性和副本数据的一致性

- Follower故障
  - Follower发生故障后会被临时踢出ISR（动态变化），待该follower恢复后，follower会读取本地的磁盘记录的上次的HW，并将该log文件高于HW的部分截取掉，从HW开始向leader进行同步，等该follower的LEO大于等于该Partition的hw，即follower追上leader后，就可以重新加入ISR
- Leader故障
  - Leader发生故障后，会从ISR中选出一个新的leader，为了保证多个副本之间的数据一致性，其余的follower会先将各自的log文件高于hw的部分截掉（新leader自己不会截掉），然后从新的leader同步数据

![image-20210613171900754](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613171903.png)

# kafka高可用集群和高性能

## Kafka高可用集群搭建节点需求规划

- 注意
  - 没那么多机器，采用伪集群方式搭建（端口号区分）
  - zookeeper部署3个节点
    - 2181
    - 2182
    - 2183
  - kafka部署3个节点
    - 9092
    - 9093
    - 9094
- 网络安全组记得开放端口

## Kafka + ZooKeeper

ZooKeeper 的官网是：https://zookeeper.apache.org/。在官网上是这么介绍 ZooKeeper 的：ZooKeeper 是一项集中式服务，用于维护配置信息，命名，提供分布式同步和提供组服务。当我们编写程序的时候，通常会将所有的配置信息保存在一个配置文件中，例如账号、密码等信息，后续直接修改配置文件就行了，那分布式场景下如何配置呢？如果说每台机器上都保存一个配置文件，这时候要一台台的去修改配置文件难免出错，而且要管理这些机器也会变得复杂和困难，ZooKeeper 的出现就是为了解决这类问题，实现高度可靠的分布式系统。

1. **配置管理**：ZooKeeper 为分布式系统提供了一种配置管理的服务：集中管理配置，即将全局配置信息保存在 ZooKeeper 服务中，方便进行修改和管理，省去了手动拷贝配置的过程，同时还保证了可靠和一致性。

2. **命名服务**：在分布式系统中，经常需要对应用或者服务进行统一命名，便于识别和区分开来，而 ZooKeeper 就提供了这种服务。

3. **分布式锁**：

   　锁应该都不陌生，没有用过也听说过，在多个进程访问互斥资源的时候，需要加上一道锁。在分布式系统中，分布式程序分布在各个主机上的进程对互斥资源进行访问时也需要加锁。

   　　分布式锁应当具备以下条件：

   - 在分布式系统环境下，一个方法在同一时间只能被一个机器的一个线程执行；
   - 高可用的获取锁与释放锁；
   - 高性能的获取锁与释放锁；
   - 具备可重入特性（可理解为重新进入，由多于一个任务并发使用，而不必担心数据错误）；
   - 具备锁失效机制，防止死锁；
   - 具备非阻塞锁特性，即没有获取到锁将直接返回获取锁失败。

4. **集群管理**：　在分布式系统中，由于各种各样的原因，例如机器故障、网络故障等，导致集群中的节点增加或者减少，集群中有些机器需要感知到这种变化，然后根据这种变化做出对应的决策。

5. **基本架构**：

   ![image-20210613172612357](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613172615.png)

**Kafka + ZooKeeper**

ZooKeeper 作为给分布式系统提供协调服务的工具被 kafka 所依赖。在分布式系统中，消费者需要知道有哪些生产者是可用的，而如果每次消费者都需要和生产者建立连接并测试是否成功连接，那效率也太低了，显然是不可取的。而通过使用 ZooKeeper 协调服务，Kafka 就能将 Producer，Consumer，Broker 等结合在一起，同时借助 ZooKeeper，Kafka 就能够将所有组件在无状态的条件下建立起生产者和消费者的订阅关系，实现负载均衡。

![image-20210613174057292](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613174100.png)

**Broker 信息**

　　在 ZooKeeper 上会有一个专门用来进行 Broker 服务器列表记录的节点，节点路径为 /brokers/ids。Kafka 的每个 Broker 启动时，都会在 ZooKeeper 中注册，创建 /brokers/ids/[0-N] 节点，写入 IP，端口等信息，每个 Broker 都有一个 BrokerId。Broker 创建的是临时节点，在连接断开时节点就会自动删除，所以在 ZooKeeper 上就可以通过 Broker 中节点的变化来得到 Broker 的可用性。

**Topic 信息**

　　在 Kafka 中可以定义很多个 Topic，每个 Topic 又被分为很多个 Partition。一般情况下，每个 Partition 独立在存在一个 Broker 上，所有的这些 Topic 和 Broker 的对应关系都由 ZooKeeper 进行维护。

**负载均衡**

　　生产者需要将消息发送给 Broker，消费者需要从 Broker 上获取消息，通过使用 ZooKeeper，就都能监听 Broker 上节点的状态信息，从而实现动态负载均衡。

**offset 信息**

　　offset 用于记录消费者消费到的位置，在老版本（0.9以前）里 offset 是保存在 ZooKeeper 中的。

**Controller 选举**

　　在 Kafka 中会有多个 Broker，其中一个 Broker 会被选举成为 Controller（控制器），在任意时刻，Kafka 集群中有且仅有一个控制器。Controller负责管理集群中所有分区和副本的状态，当某个分区的 leader 副本出现故障时，由 Controller 为该分区选举出一个新的 leader。Kafka 的 Controller 选举就依靠 ZooKeeper 来完成，成功竞选为 Controller 的 Broker 会在 ZooKeeper 中创建 /controller 这个临时节点，在 ZooKeeper 中使用 get 命令查看节点内容：

![image-20210613174645754](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613174655.png)

其中“version”在目前版本中固定为1，“brokerid”表示 Broker 的编号，“timestamp”表示竞选称为 Controller 时的时间戳。

当 Broker 启动时，会尝试读取 /controller 中的“brokerid”，如果读取到的值不是-1，则表示已经有节点竞选成为 Controller 了，当前节点就会放弃竞选；而如果读取到的值为-1，ZooKeeper 就会尝试创建 /controller 节点，当该 Broker 去创建的时候，可能还有其他 Broker 一起同时创建节点，但只有一个 Broker 能够创建成功，即成为唯一的 Controller。

## Kafka高可用集群之zookeeper集群环境准备

- cp -r 复制zk节点，一共3个

- 修改配置zoo.cfg

  ```properties
  #客户端端口，三个客户端端口分别为2181 2182 2183
  clientPort=2181
  
  #数据存储路径，/tmp/zookeeper/2181 /tmp/zookeeper/2182 /tmp/zookeeper/2183
  dataDir=/tmp/zookeeper/2181
  
  #修改AdminServer的端口：8888 8889 8890
  admin.serverPort=8888
  ```

- dataDir对应目录下分别创建myid文件，内容对应1、2、3

  ```bash
  cd /tmp/zookeeper/2181
  echo 1 > myid
  ```

- 配置集群

  ```properties
  # server.服务器id=服务器IP地址:服务器直接通信端口:服务器之间选举投票端口
  
  server.1=127.0.0.1:2881:3881
  server.2=127.0.0.1:2882:3882
  server.3=127.0.0.1:2883:3883
  ```

- zk命令

  ```bash
  #启动zk
  ./zkServer.sh  start
  
  #查看节点状态
  ./zkServer.sh status
  
  #停止节点
  ./zkServer.sh stop
  ```

## Kafka高可用集群搭建-环境准备

- 伪集群搭建，3个节点同个机器端口区分

  - 9092
  - 9093
  - 9094

- 配置

  ```properties
  #内网中使用，内网部署 kafka 集群只需要用到 listeners，内外网需要作区分时 才需要用到advertised.listeners
  listeners=PLAINTEXT://172.23.148.108:9092
  
  advertised.listeners=PLAINTEXT://101.132.252.118:9092
  
  #每个节点编号1、2、3
  broker.id=1
  
  #端口
  port=9092
  
  #配置3个
  log.dirs=/tmp/kafka-logs-1
  
  #zk地址
  zookeeper.connect=localhost:2181,localhost:2182,localhost:2183
  ```

# Kafka高可用集群搭建实战+SpringBoot项目测试

启动Kafka实战

```bash
# 守护进程
./kafka-server-start.sh -daemon ../config/server.properties &

./kafka-server-start.sh ../config/server.properties &
```

创建topic

```bash
./kafka-topics.sh --create --zookeeper 101.132.252.118:2181,101.132.252.118:2182,101.132.252.118:2183 --replication-factor 3 --partitions 6 --topic xdclass-cluster-topic
```

SpringBoot项目测试

- 连接zookeeper集群
- 创建topic
- 查看topic详情
- 发送消息

##  Kafka的中的日志数据清理

- Kafka将数据持久化到了硬盘上，为了控制磁盘容量，需要对过去的消息进行清理

- 问题：如果让你去设计这个日志删除策略，你会怎么设计？【原理思想】很重要的体现，下面是kafka答案

  - 内部有个定时任务检测删除日志，默认是5分钟 log.retention.check.interval.ms
  - 支持配置策略对数据清理
  - 根据segment单位进行定期清理

- 启用cleaner

  - log.cleaner.enable=true
  - log.cleaner.threads = 2 (清理线程数配置)

- 日志删除

  - log.cleanup.policy=delete

    ```properties
    #清理超过指定时间的消息,默认是168小时，7天
    #还有log.retention.ms, log.retention.minutes, log.retention.hours，优先级高到低
    log.retention.hours=168
    
    #超过指定大小后，删除旧的消息，下面是1G的字节数，-1就是没限制
    log.retention.bytes=1073741824
    
    #还有基于日志起始位移（log start offset)，未来社区还有更多
    ```

  - 基于【时间删除】 日志说明

    ```
    配置了7天后删除，那7天如何确定呢？
    
    每个日志段文件都维护一个最大时间戳字段，每次日志段写入新的消息时，都会更新该字段
    
    一个日志段segment写满了被切分之后，就不再接收任何新的消息，最大时间戳字段的值也将保持不变
    
    kafka通过将当前时间与该最大时间戳字段进行比较，从而来判定是否过期
    ```

  - 基于【大小超过阈值】 删除日志 说明

    ```
    假设日志段大小是500MB，当前分区共有4个日志段文件，大小分别是500MB，500MB，500MB和10MB
    
    10MB那个文件就是active日志段。
    
    此时该分区总的日志大小是3*500MB+10MB=1500MB+10MB
    
    如果阈值设置为1500MB，那么超出阈值的部分就是10MB，小于日志段大小500MB，故Kafka不会执行任何删除操作，即使总大小已经超过了阈值；
    
    如果阈值设置为1000MB，那么超过阈值的部分就是500MB+10MB > 500MB，此时Kafka会删除最老的那个日志段文件
    
    注意：超过阈值的部分必须要大于一个日志段的大小
    ```

  - log.retention.bytes和log.retention.minutes任意一个达到要求，都会执行删除

  - 日志压缩

    - log.cleanup.policy=compact 启用压缩策略
    - 按照消息key进行整理，有相同key不同value值，只保留最后一个

## Kafka的高性能原理分析-ZeroCopy

零拷贝ZeroCopy（SendFile）

- 例子：将一个File读取并发送出去（Linux有两个上下文，内核态，用户态）
  - File文件的经历了4次copy
    - 调用read,将文件拷贝到了kernel内核态
    - CPU控制 kernel态的数据copy到用户态
    - 调用write时，user态下的内容会copy到内核态的socket的buffer中
    - 最后将内核态socket buffer的数据copy到网卡设备中传送
  - 缺点：增加了上下文切换、浪费了2次无效拷贝(即步骤2和3)

![image-20210613182527789](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613182530.png)

- ZeroCopy：
  - 请求kernel直接把disk的data传输给socket，而不是通过应用程序传输。Zero copy大大提高了应用程序的性能，减少不必要的内核缓冲区跟用户缓冲区间的拷贝，从而减少CPU的开销和减少了kernel和user模式的上下文切换，达到性能的提升
  - 对应零拷贝技术有mmap及sendfile
    - mmap:小文件传输快
    - sendfile:大文件传输比mmap快
  - 应用：Kafka、Netty、RocketMQ等都采用了零拷贝技术

##  Kafka的高性能原理分析归纳总结

kafka高性能

- 存储模型，topic多分区，每个分区多segment段
- index索引文件查找，利用分段和稀疏索引
- 磁盘顺序写入
- 异步操作少阻塞sender和main线程，批量操作(batch)
- 页缓存Page cache，没利用JVM内存，因为容易GC影响性能
- 零拷贝ZeroCopy（SendFile）

# SpringBoot项目整合Spring-kafka和事务消息实战

## Springboot项目整合spring-kafka依赖包配置

添加pom文件

```xml
<dependency>
	<groupId>org.springframework.kafka</groupId>
	<artifactId>spring-kafka</artifactId>
</dependency>
```

配置文件修改增加生产者信息

```yml
spring:
  kafka:
    bootstrap-servers: 101.132.252.118:9092,101.132.252.118:9093,101.132.252.118:9094
    producer:
      # 消息重发的次数
      retries: 0
      # 一个批次可以使用的内存大小
      batch-size: 16384
      # 设置生产者内存缓冲区的大小
      buffer-memory: 33554432
      # 键的序列化方式
      key-serializer: org.apache.kafka.common.serialization.StringSerializer
      # 值的序列化方式
      value-serializer: org.apache.kafka.common.serialization.StringSerializer
      acks: all
```

发送消息

```java
@RestController
public class UserController {

    private static  final String TOPIC_NAME = "user.register.topic";
    @Autowired
    private KafkaTemplate<String, Object> kafkaTemplate;

    @GetMapping("/api/v1/{num}")
    public void sendMessage(@PathVariable("num") String num){

        kafkaTemplate.send(TOPIC_NAME,"这是一个消息，num="+num).addCallback(success->{
            String topic = success.getRecordMetadata().topic();
            int partition = success.getRecordMetadata().partition();
            long offset = success.getRecordMetadata().offset();
            System.out.println("发送成功：topic="+topic+", partition="+partition+", offset="+offset);
        },failure->{
            System.out.println("发送失败："+failure.getMessage());
        });
    }
}
```

## Springboot项目整合spring-kafka监听消费消息

配置文件修改增加消费者信息

```yml
spring:
  kafka:
    bootstrap-servers: 101.132.252.118:9092,101.132.252.118:9093,101.132.252.118:9094
    producer:
      # 消息重发的次数。
      retries: 1
      #一个批次可以使用的内存大小
      batch-size: 16384
      # 设置生产者内存缓冲区的大小。
      buffer-memory: 33554432
      # 键的序列化方式
      key-serializer: org.apache.kafka.common.serialization.StringSerializer
      # 值的序列化方式
      value-serializer: org.apache.kafka.common.serialization.StringSerializer
      acks: all
      #事务id
      transaction-id-prefix: xdclass-tran-
    consumer:
      # 自动提交的时间间隔 在spring boot 2.X 版本是值的类型为Duration 需要符合特定的格式，如1S,1M,2H,5D
      auto-commit-interval: 1S
      # 该属性指定了消费者在读取一个没有偏移量的分区或者偏移量无效的情况下该作何处理：
      auto-offset-reset: earliest
      # 是否自动提交偏移量，默认值是true,为了避免出现重复数据和数据丢失，可以把它设置为false,然后手动提交偏移量
      enable-auto-commit: false
      # 键的反序列化方式
      key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
      # 值的反序列化方式
      value-deserializer: org.apache.kafka.common.serialization.StringDeserializer
    listener:
      #手工ack，调用ack后立刻提交offset
      ack-mode: manual_immediate
      #容器运行的线程数
      concurrency: 4
```

代码编写

```java
@Component
public class MQListener {
    /**
     *  消费监听
     * @param record
     */
    @KafkaListener(topics = {"user.register.topic"},groupId = "xdlcass-test-gp")
    public void onMessage1(ConsumerRecord<?, ?> record, Acknowledgment ack, @Header(KafkaHeaders.RECEIVED_TOPIC) String topic){
        // 打印出消息内容
        System.out.println("消费："+record.topic()+"-"+record.partition()+"-"+record.value());
        ack.acknowledge();
    }
}
```

## Kafka事务消息-整合SpringBoot实战

- Kafka 从 0.11 版本开始引入了事务支持

  - 事务可以保证对多个分区写入操作的原子性
  - 操作的原子性是指多个操作要么全部成功，要么全部失败，不存在部分成功、部分失败的可能

- 配置

  ```properties
  spring:
    kafka:
      bootstrap-servers: 112.74.55.160:9092,112.74.55.160:9093,112.74.55.160:9094
      producer:
        # 消息重发的次数。 配置事务的话：如果用户显式地指定了 retries 参数，那么这个参数的值必须大于0
        #retries: 1
        #一个批次可以使用的内存大小
        batch-size: 16384
        # 设置生产者内存缓冲区的大小。
        buffer-memory: 33554432
        # 键的序列化方式
        key-serializer: org.apache.kafka.common.serialization.StringSerializer
        # 值的序列化方式
        value-serializer: org.apache.kafka.common.serialization.StringSerializer
        #配置事务的话：如果用户显式地指定了 acks 参数，那么这个参数的值必须-1 all
        #acks: all
  
        #事务id
        transaction-id-prefix: xdclass-tran
      consumer:
        # 自动提交的时间间隔 在spring boot 2.X 版本是值的类型为Duration 需要符合特定的格式，如1S,1M,2H,5D
        auto-commit-interval: 1S
  
        # 该属性指定了消费者在读取一个没有偏移量的分区或者偏移量无效的情况下该作何处理：
        auto-offset-reset: earliest
  
        # 是否自动提交偏移量，默认值是true,为了避免出现重复数据和数据丢失，可以把它设置为false,然后手动提交偏移量
        enable-auto-commit: false
  
        # 键的反序列化方式
        key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
        # 值的反序列化方式
        value-deserializer: org.apache.kafka.common.serialization.StringDeserializer
  
      listener:
        # 在侦听器容器中运行的线程数。
        concurrency: 4
        #listner负责ack，手动调用Acknowledgment.acknowledge()后立即提交
        ack-mode: manual_immediate
        #避免出现主题未创建报错
        missing-topics-fatal: false
  ```

- SpringBoot代码编写

```java
/**
     * 注解方式的事务
     * @param i
     */
    @GetMapping("/kafka/transaction1")
    @Transactional(rollbackFor = RuntimeException.class)
    public void sendMessage1(int i) {

        kafkaTemplate.send(TOPIC_NAME, "这个是事务里面的消息：1  i="+i);
            if (i == 0) {
                throw new RuntimeException("fail");
            }
        kafkaTemplate.send(TOPIC_NAME, "这个是事务里面的消息：2  i="+i);
    }
    /**
     * 声明式事务支持
     * @param i
     */
    @GetMapping("/kafka/transaction2")
    public void sendMessage2(int i) {
        kafkaTemplate.executeInTransaction(new KafkaOperations.OperationsCallback() {
            @Override
            public Object doInOperations(KafkaOperations kafkaOperations) {
                kafkaOperations.send(TOPIC_NAME,"这个是事务里面的消息：1  i="+i);
                if(i==0)
                {
                    throw new RuntimeException("input is error");
                }
                kafkaOperations.send(TOPIC_NAME,"这个是事务里面的消息：2  i="+i);
                return true;
            }
        });
    }
```

## 关于 Kafka的其他特性和技术选型建议

Kafka很多内容，但是不一定都要学，看自己的需求，有些功能是比较鸡肋的

- 比如kafka streams 虽然轻量级
  - 但是与Kafka 紧密联系，无法在没有Kafka 的场景下使用
  - 相较于实时计算工具Spark Streaming、Flink等，kafka streams不适用于大型业务场景
  - 有些功能的话虽然kafka有，但还是用更好的工具比较好，且技术更新换代快，掌握设计思想才主要
  - 更主要的是没有万能的框架，技术选型多数都是基于【 业务需求】出发，选出最合适的技术
  - kafka/rabbitmq/rocketmq
