# 启动nacos

```bash
cd /usr/local/nacos/bin
sh startup.sh -m standalone
```

注意：nacos一旦重启，和izipkin一样，一旦重启，nacos的配置数据就会消失，zipkin的链路追踪数据也一样消失，所以一定要进行持久化配置，或者进行阿里云+docker容器化部署就可以解决这样的问题。





# 启动sentinel

```bash
cd /usr/local/software

java -Dserver.port=8080 -Dcsp.sentinel.dashboard.server=localhost:8080 -Dproject.name=sentinel-dashboard -jar sentinel-dashboard-1.8.1.jar
```

查看 WSL ip

```powershell
ip addr

22: eth0: <> mtu 1500 group default qlen 1
    link/ether 7c:2a:31:3a:b6:22
    inet 169.254.121.148/16 brd 169.254.255.255 scope global dynamic
       valid_lft forever preferred_lft forever
    inet6 fe80::d41c:f320:871a:7994/64 scope link dynamic
       valid_lft forever preferred_lft forever
```

[![gk1HnH.png](https://z3.ax1x.com/2021/04/29/gk1HnH.png)](https://imgtu.com/i/gk1HnH)

下面的三个都获取不到服务的实时监控内容，终端报错

[![gk159K.png](https://z3.ax1x.com/2021/04/29/gk159K.png)](https://imgtu.com/i/gk159K)

```yml
server:
  port: 8000
spring:
  application:
    name: xdclass-order-service
  cloud:
    nacos:
      discovery:
        server-addr: 101.132.252.118:8848
    sentinel:
      transport:
        dashboard: 127.0.0.1:8080
        port: 9999
        clientIp: localhost
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://127.0.0.1:33061/cloud_order?useUnicode=true&characterEncoding=utf-8&useSSL=false
    username: root
    password: 2012T20Tyear
# 控制台输出sql、下划线转驼峰
mybatis:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
    map-underscore-to-camel-case: true
# 使用随机负载均衡策略
xdclass-video-service:
  ribbon:
    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule
```

尽管这样，终端还是依旧报错，不过sentinel是看得到各个服务的监控信息





# zipkin持久化报错

在cmd窗口输入以下命令报错

```cmd
java -jar zipkin-server-2.12.9-exec.jar --STORAGE_TYPE=mysql --MYSQL_HOST=127.0.0.1 --MYSQL_TCP_PORT=33061 --MYSQL_DB=zipkin_log --MYSQL_USER=root --MYSQL_PASS=root
```

报错原因

```
Caused by: java.sql.SQLException: Access denied for user 'root'@'localhost' (using password: NO) Current charset is GBK. If password has been set using other charset, consider using option 'passwordCharacterEncoding'
```

 报错解决方案

https://stackoverflow.com/questions/54188309/mariadb-connection-client-access-denied-for-user-using-password-no-on-mysql

在MySQL 8.0 Command Line Client输入

```sql
ALTER USER 'root'@'localhost'   IDENTIFIED WITH mysql_native_password   BY 'password'; # 这里的password填上你的密码即可
```

MySQL 8.0 | 身份验证插件

https://www.modb.pro/db/53686

相关文档：

### Changes in MySQL 8.0

https://dev.mysql.com/doc/refman/8.0/en/upgrading-from-previous-series.html#upgrade-caching-sha2-password

#### Caching SHA-2 Pluggable Authentication

https://dev.mysql.com/doc/refman/8.0/en/caching-sha2-pluggable-authentication.html



# docker

docker 安装 nginx 后执行 

```bash
docker run --name nginx-xd -p 8080:80 -d nginx
```

返回容器的id

```
61f2f35faceabb351bbb271707f8343ae2a2048f289ef9522bf32f45ee821d45
```



记录以下实战的过程中的设置：

1. docker中的nacos端口为8849
2. docker中的sentinel端口为8858



# 防火墙：

```bash
# 启动一个服务：
systemctl start firewalld.service

# 关闭一个服务：
systemctlstop firewalld.service

# 显示状态： 
firewall-cmd --state

# 查看所有打开的端口： 
firewall-cmd --zone=public --list-ports

# 添加： （--permanent永久生效，没有此参数重启后失效）
firewall-cmd --zone=public --add-port=80/tcp --permanent 

# 重新载入：
firewall-cmd --reload

# 查看：
firewall-cmd --zone=public --query-port=80/tcp

# 删除：
firewall-cmd --zone=public --remove-port=80/tcp --permanent
```



阿里云ECS服务器的 MySQL root密码：yourpassword

注意：mysql不是用docker安装的



# 远程访问mysql8.0

https://segmentfault.com/a/1190000039707605