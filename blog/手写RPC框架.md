---
title: 手写RPC框架
---

# 项目架构变化

![image-20220102103803973](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102103805.png)

# RPC简介

## RFC

![image-20220102105527153](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102105528.png)

## RPC

<img src="C:/Users/86158/AppData/Roaming/Typora/typora-user-images/image-20220102105643306.png" alt="image-20220102105643306" style="zoom:150%;" />

<img src="https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102105704.png" alt="image-20220102105703175" style="zoom:150%;" />

## RPC和HTTP对比

![image-20220102110346126](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102110347.png)

![image-20220102110406727](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102110407.png)

所谓的效率主要取决于网络的基础建设和数据量级

现在http都是支持这种长连接的

![image-20220102111710457](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102111711.png)

![image-20220102112120129](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102112121.png)

<img src="C:/Users/86158/AppData/Roaming/Typora/typora-user-images/image-20220102132242355.png" alt="image-20220102132242355" style="zoom:150%;" />

![image-20220102132540779](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102132542.png)

![image-20220102133020743](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102133022.png)

# HttpClient 实现RPC

![image-20220102141347225](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102141348.png)

## HttpClient Get

![image-20220102141252060](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102141254.png)

![image-20220102141541626](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102141542.png)

![image-20220102142200279](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102142201.png)

![image-20220102142420776](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102142422.png)

![image-20220102142507916](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102142509.png)

![image-20220102142712350](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102142713.png)

![image-20220102144955180](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102144956.png)

![image-20220102145307485](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102145309.png)

## HttpClient Post

![image-20220102150736121](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102150737.png)

![image-20220102151310022](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102151311.png)

![image-20220102153453013](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102153454.png)

![image-20220102153728606](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102153730.png)

![image-20220102154111018](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102154112.png)

![image-20220102154424669](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102154425.png)

![image-20220102155444968](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102155446.png)

![image-20220102155547245](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102155548.png)

![image-20220102155756074](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102155758.png)

![image-20220102155850247](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102155851.png)

字符串转对象

![image-20220102160635448](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102160636.png)

![image-20220102160854163](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102160855.png)

# AJAX跨域请求

添加CrossOrigin

![image-20220102163456453](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102163458.png)

![image-20220102163618908](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102165158.png)

![image-20220102164952641](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102164953.png)

# 基于RMI开发RPC实现

![image-20220102165529292](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102165530.png)

![image-20220102165726319](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102165727.png)

![image-20220102171043360](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102171045.png)

![image-20220102171225830](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102171227.png)

接口。

![image-20220102171548670](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102171550.png)

server实现接口

![image-20220102172251321](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102172252.png)

![image-20220102172542547](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102172544.png)

![image-20220102173110543](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102173112.png)

![image-20220102173610254](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102173611.png)

![image-20220102173739702](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102173741.png)

![image-20220102173824124](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102173825.png)

![image-20220102174827307](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102174829.png)

![image-20220102174954828](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102174956.png)

![image-20220102175215583](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102175217.png)

# Zookeeper

## Zookeeper zkCli

![image-20220102175854654](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102175856.png)

![image-20220102181150781](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102181152.png)

![image-20220102181236564](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102181237.png)

![image-20220102181442072](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102181443.png)

![image-20220102182839775](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102182841.png)

![image-20220102183107611](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102183108.png)

## Zookeeper 创建节点

![image-20220102200140887](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102200142.png)

![image-20220102200706169](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102200707.png)

![image-20220102201252733](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102201254.png)

![image-20220102201615487](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102201618.png)

![image-20220102201656746](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102201658.png)

![image-20220102201900809](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102202312.png)

## Zookeeper 遍历节点

![image-20220102202332618](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102202334.png)

![image-20220102202908880](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102202910.png)

![image-20220102202927783](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102202929.png)

## Zookeeper 查询数据

![image-20220102203148785](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102203150.png)

## Zookeeper 删除节点

![image-20220102204821833](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102204823.png)

![image-20220102204850495](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102204851.png)

# 自定义RPC框架 

## 开发连接类型和注册器类型

![image-20220102204941348](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102204943.png)

![image-20220102205718405](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102205720.png)

![image-20220102210113388](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102210114.png)

![image-20220102210231715](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102210233.png)

编写注册服务

![image-20220102212124872](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102212128.png)

![image-20220102210556876](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102210558.png)



![image-20220102210956675](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102210958.png)

![image-20220102212458696](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102212500.png)

![image-20220102212719249](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102212720.png)

![image-20220102212854363](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102212856.png)

## 开发框架入口工厂类型

![image-20220102213202205](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102213203.png)

![image-20220102213742917](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102213744.png)

![image-20220102213837495](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102213840.png)

![image-20220102214118635](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102214120.png)

## 测试服务端开发

![image-20220102214214255](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102214215.png)

![image-20220102214320685](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102214322.png)

![image-20220102214419112](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102214420.png)

![image-20220102214458952](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102214500.png)

![image-20220102214616746](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102214618.png)

![image-20220102214917096](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102214918.png)

![image-20220102214957988](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102214959.png)

![image-20220102215141225](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220102215142.png)

![image-20220102215315960](C:/Users/86158/AppData/Roaming/Typora/typora-user-images/image-20220102215315960.png)

![image-20220103094100764](C:/Users/86158/AppData/Roaming/Typora/typora-user-images/image-20220103094100764.png)

![image-20220103094132717](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103094133.png)

![image-20220103094152827](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103094154.png)

再写一个服务接口

![image-20220103094501111](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103094502.png)

![image-20220103094626985](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103095036.png)

## 优化注册逻辑

![image-20220103095213068](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103095215.png)

![image-20220103095254714](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103095255.png)

![image-20220103095343171](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103095344.png)

![image-20220103095645507](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103095646.png)

![image-20220103095754414](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103095755.png)



![image-20220103095958307](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103100000.png)

## 基于自定义框架结合SpringBoot开发应用

![image-20220103100329022](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103100330.png)

![image-20220103100511405](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103100512.png)

![image-20220103100629611](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103100631.png)

![image-20220103100834137](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103100835.png)

![image-20220103100907676](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103100909.png)

![image-20220103101127656](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103101129.png)

![image-20220103101154427](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103101155.png)

![image-20220103101442230](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103101443.png)

![image-20220103101537218](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103101538.png)

![image-20220103101715548](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103101716.png)

![image-20220103101919944](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103101921.png)

![image-20220103102114347](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103102115.png)

![image-20220103102152360](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103102153.png)

![image-20220103102321726](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103102323.png)

![image-20220103102345420](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103102347.png)

![image-20220103102414891](C:/Users/86158/AppData/Roaming/Typora/typora-user-images/image-20220103102414891.png)

![image-20220103102509728](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103102511.png)

![image-20220103102712705](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103102714.png)

![image-20220103103049338](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103103050.png)

![image-20220103102938331](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103102939.png)
