---
title: Linux智能DNS
categories: 计算机网络
tags: [DNS]
---

# Linux之智能DNS大纲速览

Linux之智能DNS是我今天在慕课网意外看到的一门课，试听了一下，感觉还行。讲师把它分成了三个部分讲。

DNS学习三部曲：

```
第一部Bind服务：掌握Bind的服务的搭建过程及DNS测试方法
第二部Bind负载均衡：DNS负载均衡的实现过程
第三部智能DNS：智能DNS的实现原理
```

# 第一部Bind服务

DNS，简单地说，就是Domain Name System，翻成中文就是“域名系统”。 
在一个TCP/IP架构的网络（例如Internet）环境中，DNS是一个非常重要而且常用的系统。主要的功能就是将人易于记忆的Domain Name与人不容易记忆的IP Address作转换。而上面执行DNS服务的这台网络主机，就可以称之为DNS Server。基本上，通常我们都认为DNS只是将Domain Name转换成IP Address，然后再使用所查到的IP Address去连接（俗称“正向解析”）。事实上，将IP Address转换成Domain Name的功能也是相当常使用到的，当login到一台Unix工作站时，工作站就会去做反查，找出你是从哪个地方连线进来的（俗称“逆向解析”）。

网关(Gateway)又称网间连接器、协议转换器。网关在传输层上以实现网络互连，是最复杂的网络互连设备，仅用于两个高层协议不同的网络互连。网关既可以用于广域网互连，也可以用于局域网互连。 网关是一种充当转换重任的计算机系统或设备。

## Bind介绍

Bind是开源、稳定、应用广泛的DNS服务
Bind的组成：域名解析服务；权威域名服务；DNS工具

## DNS中的域名

www.imooc.com == www.imooc.com.
注释：www.imooc.com.中，最后的“.”是根域；com.是一级域名；imooc.com.是二级域名

![image-20210613195107812](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613195132.png)

## 域名解析及权威域名解析

```
1、域名服务器存在域名记录，则直接返回IP（递归查询），否则进行迭代查询（如图）
2、图中名字服务器都可以用到bind服务，红框中的bind具有权威解析，因为他返回域名对应的权威解析IP地址
```

![image-20210613195439983](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613195442.png)

## DNS解析记录分类

```
1、A记录：由域名服务器返回IP地址（基本、最多的记录）
2、CNAME：方便多个域名解析同一个IP地址（如图创建一个CNAME记录指向有A记录的域名）
3、NS记录：bind服务器不能进行权威解析时，会回一个NS记录给用户，这时用户再发起另一台bind服务器的权威解析请求。
4、MX记录：全称是邮件交换记录，在使用邮件服务器的时候，MX记录是无可或缺的，比如A用户向B用户发送一封邮件，那么他需要向DNS查询B的MX记录，DNS在定位到了B的MX记录后反馈给A用户，然后A用户把邮件投递到B用户的MX记录服务器里。
```

<img src="https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210614000326.png" alt="image-20210613195931321" style="zoom:80%;" />

<img src="https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210614000322.png" alt="image-20210613195942389" style="zoom:67%;" />

![image-20210613195959194](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210614000314.png)

## Bind安装
redhat：\#yum install bind bind-chroot
ubuntu：\$sudo apt-get install bind9
确认安装：\#rpm -qa | grep bind
查看安装内容：\#rpm -ql bind | more

## Bind服务默认配置文件

```
建议一开始把bind默认配置文件做一个备份mv /etc/named.comf /etc/named.comf_default
1、启动bind服务：#/etc/init.d/named start

2、主配置文件：/etc/named.conf(配置项如下)
options{}-整个bind使用的全局选项（监听端口；数据文件、缓存存储位置；权限加密的控制）
logging{}-服务日志选项（日志输出；日志输出级别；日志输出位置等）
zone.{}-DNS域解析（解析记录位置）

listen-on port 53 {127.0.0.1}   //默认监听所有地址
directory   //存放着数据库的控制文件，配置的zone，还有主的配置文件目录
dump-file   //DNS解析过的一些缓存信息存放位置；
statistics-file   //静态解析文件；
memstatistics-file   //内存的统计信息
allow-query   //权限信息
dnssec-enable、dnssec-validation、dnssec-lookaside   //加密信息

channel   //控制日志输出
file   //输出文件位置
severity   //控制日志输出详细级别以及安全重要级别
```

![image-20210613200557083](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613200601.png)

![image-20210613200811312](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613200828.png)

## Bind服务实战场景一配置

![image-20210613201638271](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210614000308.png)

创建/etc/named.conf文件并编辑，配置option和zone

![image-20210613201001526](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613201054.png)

zone的file配置，imooc.come.zone 这个文件就写在 /var/named这个文件夹下

![image-20210613201049309](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613201051.png)

```
配置imooc.com的一个起始SOA记录。 jeson.imooc.com是邮箱, @表示当前域名（当书写邮箱地址的时候必须将@改成.代替）。 后面括号的时间用于DNS主从。
配置 imooc.com的NS记录，告诉bind我的解析是靠哪一台DNS服务器来解析的。
给imooc.com的权威解析DNS配一个A记录
给www.imooc.com 配置一个A记录

重启named服务：/etc/init.d/named restart
本机测试：dig @192.168.199.202 www.imooc.com
补充：重启服务如有报错查看tail -f /var/log/messages
```

![image-20210613201605542](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613201725.png)

Bind服务配置文件的重点总结：

1.严格注意语法书写，其格式非常严格
2.@是DNS记录中的保留字，表示当前域名（当书写邮箱地址的时候必须将@改成.代替）
3.记录不准折行书写
4.单行记录开头不准空格或tab开头

## Bind服务实战场景二配置

![image-20210613201846931](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210614000300.png)

```properties
#实战场景2代码：
1.先ping imooc的ip
ping www.imooc.com-->117.121.101.41

#2.修改/var/named/imooc.com.zone文件，将www的A记录IP地址替换成imooc的IP地址
$TTL 7200
imooc.com. IN SOA imooc.com. ho.imooc.com. (222 1H 15M 1W 1D)
imooc.com. IN NS dns1.imooc.com.
dns1.imooc.com. IN A 192.168.199.202
www.imooc.com. IN A 117.121.101.41

#3.修改/etc/named.conf文件，添加zone "iaskjob.com"
options{
	directory "/var/named";
};
zone "imooc.com"{
	type master;
	file "imooc.com.zone";
};
zone "iaskjob.com"{
	type master;
	file "iaskjob.com.zone";
};

#4.新建iaskjob.com.zone文件并编辑
vim /var/named/iaskjob.com.zone

$TTL 7200
iaskjob.com. IN SOA isakjob.com. iaskjob.163.com. (4012100 1H 15M 1W 1D)
iaskjob.com. IN NS dns1.iaskjob.com.
dns1.iaskjob.com. IN A 192.168.199.202
imooc.iaskjob.com. IN CNAME www.imooc.com.

#5.重启named服务
/etc/init.d/named restart

#6.本机测试
dig @127.0.0.1 imooc.iaskjob.com

#7.远程测试
物理机设置DNS为虚拟机的IP（192.168.199.202）
物理机运行nslookup imooc.iaskjob.com
能解析到imooc的IP地址就表示正确

#补充：重启服务如有报错查看/var/log/messages
tail -f /var/log/messages
```

## 正向解析与反向解析

![image-20210613204404470](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613204406.png)

PTR通常用于邮件系统

![image-20210613204450624](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613204453.png)

```properties
#配置正向解析：编辑/var/named/imooc.com.zone文件，在最后加上两行, (10 为优先级)
@ IN MX 10 mail
mail IN A 10.156.11.233
#重启named服务，本机测试
dig @192.168.5.107 mail.imooc.com


#反向解析：编辑/etc/named.conf文件，在最后加入
zone "5.168.192.in-addr.arpa"{
	type master;
	file "10.156.11.zone";
};
#在/var/named/下新建10.156.11.zone文件
$TTL 7200
@ IN SOA 11.156.10.in-addr.arpa. iaskjob.163.com. (2014012200 1H 15M 1W 1D)
@ IN NS dns1.imooc.com.
232 IN PTR dns1.imooc.com.
233 IN PTR mail.imooc.com.
#重启named服务，本机测试
dig -x 10.156.11.233 @127.0.0.1
```

![image-20210613204823250](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613204829.png)

逆向解析重点总结：

```
1.逆向解析域in-addr.arpa的书写格式
2.常用于邮件服务的域名解析
3.配置文件权限需要named用户可读取，没有权限启动named服务会报错
-rw-r--r--. 1 root  root   191 Aug 29 19:40 10.156.11.zone
-rw-r--r--. 1 root  root   202 Aug 29 17:12 iaskjob.com.zone
-rw-r--r--. 1 root  root   219 Aug 29 19:43 imooc.com.zone
```

## Bind服务常用客户端工具：

![image-20210613211233466](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613211237.png)

```bash
host文件位置：/etc/resolv.conf

# host www.baidu.com
www.baidu.com is an alias for www.a.shifen.com.
www.a.shifen.com has address 14.215.177.38
www.a.shifen.com has address 14.215.177.37

#nslookup www.baidu.com
Server:		114.114.114.119
Address:	114.114.114.119#53

Non-authoritative answer:
www.baidu.com	canonical name = www.a.shifen.com.
Name:	www.a.shifen.com
Address: 14.215.177.38
Name:	www.a.shifen.com
Address: 14.215.177.37

#dig www.baidu.com
; <<>> DiG 9.8.2rc1-RedHat-9.8.2-0.17.rc1.el6 <<>> www.baidu.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 33977
;; flags: qr rd ra; QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 0
;; QUESTION SECTION:
;www.baidu.com.			IN	A
;; ANSWER SECTION:
www.baidu.com.		546	IN	CNAME	www.a.shifen.com.
www.a.shifen.com.	199	IN	A	14.215.177.38
www.a.shifen.com.	199	IN	A	14.215.177.37
;; Query time: 42 msec
;; SERVER: 114.114.114.119#53(114.114.114.119)
;; WHEN: Mon Aug 29 20:33:36 2016
;; MSG SIZE  rcvd: 90
```

host命令使用

```bash
#host www.baidu.com

#host -t SOA baidu.com
baidu.com has SOA record dns.baidu.com. sa.baidu.com. 2012132847 300 300 2592000 7200

#host -t NS baidu.com
baidu.com name server dns.baidu.com.
baidu.com name server ns2.baidu.com.
baidu.com name server ns3.baidu.com.
baidu.com name server ns4.baidu.com.
baidu.com name server ns7.baidu.com.

#host -t A baidu.com
baidu.com has address 220.181.57.217
baidu.com has address 123.125.114.144
baidu.com has address 111.13.101.208
baidu.com has address 180.149.132.47
```

nslookup命令使用

```bash
#nslookup www.baidu.com
Server:114.114.114.119
Address:114.114.114.119#53
Non-authoritative answer:
www.baidu.com	canonical name = www.a.shifen.com.
Name:www.a.shifen.com
Address:14.215.177.37
Name:www.a.shifen.com
Address:14.215.177.38

## nslookup可以进入交互模式单独查找SOA，A等等记录

#nslookup
>set q=soa
>baidu.com
Server:114.114.114.119
Address:114.114.114.119#53
Non-authoritative answer:
baidu.com
origin = dns.baidu.com
mail addr = sa.baidu.com
serial = 2012132847
refresh = 300
retry = 300
expire = 2592000
minimum = 7200
Authoritative answers can be found from:

>set q=a
>baidu.com
Server:114.114.114.119
Address:114.114.114.119#53
Non-authoritative answer:
Name:baidu.com
Address:180.149.132.47
Name:baidu.com
Address:123.125.114.144
Name:baidu.com
Address:111.13.101.208
Name:baidu.com
Address:220.181.57.217
```

dig命令使用

```
## 反向解析：dig -x 网址IP地址 @DNS地址
#dig -x 14.215.177.38 @114.114.114.119
```

![image-20210613213402523](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613213426.png)

```bash
## dig -t 指定要查询的记录类型
#dig -t a baidu.com
; <<>> DiG 9.8.2rc1-RedHat-9.8.2-0.17.rc1.el6 <<>> -t a baidu.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 48165
;; flags: qr rd ra; QUERY: 1, ANSWER: 4, AUTHORITY: 0, ADDITIONAL: 0
;; QUESTION SECTION:
;baidu.com.			IN	A
;; ANSWER SECTION:
baidu.com.		227	IN	A	220.181.57.217
baidu.com.		227	IN	A	180.149.132.47
baidu.com.		227	IN	A	123.125.114.144
baidu.com.		227	IN	A	111.13.101.208
;; Query time: 36 msec
;; SERVER: 114.114.114.119#53(114.114.114.119)
;; WHEN: Mon Aug 29 20:51:30 2016
;; MSG SIZE  rcvd: 91
```

# 第二部Bind负载均衡

## DNS递归迭代查询

![image-20210613214403233](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613214405.png)

DNS递归查询：DNS服务器接收到客户机请求，必须使用一个准确的查询结果回复客户机。如果DNS服务器本地没有存储查询DNS 信息，那么该服务器会询问其他服务器；
迭代查询：DNS 服务器会向客户机提供其他能够解析查询请求的DNS服务器地址，当客户机发送查询请求时，DNS服务器并不直接回复查询结果，而是告诉客户机另一台DNS服务器地址，客户机再向这台DNS服务器提交请求，依次循环直到返回查询的结果为止。
一般默认禁止递归查询，服务器只要告诉客户端本地是否有数据即可，而非请求寻找其他服务器再给客户端答案，出于安全和系统资源考虑。

**DNS递归参数**

![image-20210613214444902](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613214901.png)

```bash
# dig最基本的使用方式就是
dig www.oolec.com
# 即查询域名的A记录，查询的dns服务器将采用系统配置的服务器，即/etc/resovle.conf 中的。
# 此外，如果你是一个系统管理员，部署好了一台dns服务器之后想对它进行解析测试，就必须要显式指定待测试的dns服务器地址了，例如
dig @202.106.0.20 www.oolec.com 
# 如果要查询其他类型的记录，比如MX，CNAME，NS，PTR等，只需将类型加在命令后面即可
dig www.oolec.com mx
dig www.oolec.com ns
vim /etc/named.conf
```

![image-20210613214816071](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613214818.png)

## 子域授权

NS记录，子域授权，父域，子域

![image-20210613214938049](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613220454.png)

创建/etc/named.conf文件并编辑，配置option和子域的zone

![image-20210613215750676](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613220457.png)

编辑imooc.com.zone，添加子域的授权

![image-20210613215048385](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613215202.png)

配置test.imooc.com.zone文件

![image-20210613220102189](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613220509.png)

![image-20210613220735327](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613221211.png)

## DNS转发

![image-20210613221207089](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613221221.png)

DNS转发参数：

![image-20210613221249774](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613221257.png)

DNS转发配置

全局转发，如果本服务器无法解析，自动向forwarders中服务器进行请求转发

![image-20210613221444563](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613221447.png)

DNS指定特定域的转发配置

![image-20210613221529264](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613221531.png)

## DNS主从区域传输

*8DNS区域**

![image-20210613221808873](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613221814.png)

**DNS主从同步原理**

![image-20210613221933360](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210614000231.png)

**主从同步配置**

vim /etc/named.conf

DNS主从同步配置（Master服务器）：

```properties
zone "imooc.com"{
	type master;
	notify yes;
	also-notify {30.96.8.233;};
	file "imooc.com.zone";
};
```

DNS主从同步配置（Slave服务器）：

```properties
zone "imooc.com"{
	type slave;
	file "slaves/imooc.com.zone";
	masters {30.96.8.232;};
};
```

DNS主从同步配置注意问题：

```
1.确保防火墙规则开放（建议关闭）
2.确保目录权限（系统默认named用户）
3.保持主从服务器时钟一致
4.搭建完毕后，若修改主从服务器域配置，Serail number必须递增
```

**DNS区域传输限制**

区域传输限制是为了服务器的安全，保护信息的敏感性

实现区域传输限制的两种方法：
1.基于主机的访问控制
2.事务签名

区域传输限制实现方法1-基于主机的访问控制：

```
参数：allow-transfer
选项：{address_list | none};
作用：允许域传输机器列表
```

## DNS数据加密方式

区域传输限制信息加密方式：
1.DES：对称加密
2.IDEA：非对称加密，私钥包括公钥和私钥，安全性较DES方式高。

![image-20210613223347951](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613223350.png)

区域传输限制方法2-DNS事务签名：
TSIG：对称方式
SIG0：非对称方式

TSIG事务签名：

![image-20210613223611093](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613223617.png)

生成dns的key -a 指定加密算法， -b指定加密位数 jeson-key为key的名称

![image-20210613224032943](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613224324.png)

![image-20210613224311426](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613224318.png)

新建文件jeson-key

![image-20210613224433956](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613224941.png)

编辑/etc/named.conf

![image-20210613224629244](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613224631.png)

将主服务器的key传给从服务器

![image-20210613224935709](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613224948.png)

编辑/etc/named.conf

![image-20210613225055410](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613225057.png)

配置完毕后，若修改主从服务器域配置，Serail number必须递增

# 第三部智能DNS

![image-20210613225521380](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613225744.png)

## 智能DNS作用

1. CDN加速  
2. 减少动态响应延时  
3. 负载均衡  
4. 防止DDOS攻击

## 智能DNS缺陷

1. 成本增加（如硬件成本、维护成本）
2. 不配套支持应用检测机制
3. 准确性欠缺

<img src="https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613225741.png" alt="image-20210613225644622" style="zoom:67%;" />

```
CNT 电信
CNU 联通
CNM 移动
```

## 智能DNS的IP库

IP库：能提供完整且准确的IP地址位置等信息

IP库获取途径

1. 商业第三方机构、ISP提供
2. 自己修正或者弥补
3. 通过APINC生成IP库

通过APINC生成IP库：

```http
ftp://ftp.apnic.net/public/apnic/stats/apnic/assigned-apnic-latest
```

![image-20210613230155512](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613230159.png)

## Bind中的ACL

ACL：媒体访问控制列表

![image-20210613230335765](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613230339.png)

求子网掩码位数：

![image-20210613230501718](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613230516.png)

具体程序实现方法：

![image-20210613230557355](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613230624.png)

shell获取智能DNS的IP库

![image-20210613230718580](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613230824.png)

![image-20210613230910410](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613230912.png)

![image-20210613231120841](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613231415.png)

保存后执行sh Download_ip_pools.sh

将三个文件改为以.acl后缀结尾的文件，把结尾替换成分号

![image-20210613231410058](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613231421.png)

顶部结尾闭合

![image-20210613231540088](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613231542.png)

其他两个文件也是如此

## Bind中的view配置

![image-20210613232214804](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613232217.png)

拷贝

![image-20210613231955467](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613232222.png)

查看imooc.com.chinanet.zone

![image-20210613234740674](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210614000201.png)

设置，如果是电信用户，访问2.2.2.2；如果是联通用户，访问1.1.1.1；如果是其他，访问3.3.3.3

测试：把当前主机地址拷贝到CHINANET.acl里面

![image-20210613232319484](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613232538.png)

reload

![image-20210613232532687](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613232535.png)

输入dig @30.96.8.232 www.imooc.com

![image-20210613232657012](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613235253.png)

## DNS安全

**DNS信息污染**

![image-20210613235052806](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613235055.png)

DNS信息污染演示

用UDP的方式查询：

![image-20210613235236922](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613235243.png)

用TCP的方式查询结果是不一样的：

![image-20210613235453961](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210613235456.png)

查找ip来源

![image-20210613235522807](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210614000141.png)

**DNS拒绝服务攻击**

1. 利用DNS软件版本漏洞攻击
2. DDOS攻击

**DNS放大攻击**

![image-20210613235747242](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20210614000110.png)

如何避免自己的DNS服务器沦为肉机

1. 关注自己DNS版本，尽量保持更新
2. 用于权威解析的DNS服务，关闭递归查询
3. 用于递归查询的服务器，尽量限制服务使用范围

