---
title: Tomcat底层源码深度分析
---

# Tomcat中的四大Servlet容器

![image-20220103110954904](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103110956.png)

![image-20220103105045049](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103105046.png)

![image-20220103105111067](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103105112.png)

![image-20220103105241798](C:/Users/86158/AppData/Roaming/Typora/typora-user-images/image-20220103105241798.png)

![image-20220103110412675](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103110415.png)

![image-20220103110617658](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103110618.png)

# 用文件夹部署应用的方式

![image-20220103110800949](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103110802.png)

运行startup脚本和运行这个类是一样的

部署一个应用的方法有多少种？

conf

![image-20220103111217142](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103111219.png)

![image-20220103111258831](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103111300.png)

![image-20220103111543337](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103111544.png)

![image-20220103111921308](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103111922.png)

启动tomcat

![image-20220103111941083](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103111942.png)

![image-20220103112053070](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103112054.png)

![image-20220103112144067](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103112145.png)

![image-20220103112324160](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103112325.png)

![image-20220103112851946](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103112853.png)

![image-20220103113858488](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103113900.png)

<img src="https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103113921.png" alt="image-20220103113916391" style="zoom:150%;" />

![image-20220103114040737](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103114042.png)

![image-20220103114134418](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103114136.png)

# 用server.xml部署应用的方式

tomcat外部

![image-20220103114240213](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103114241.png)

就是个servlet容器

![image-20220103114827915](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103114829.png)

![image-20220103115405309](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103115406.png)

![image-20220103115647283](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103115648.png)

![image-20220103115727802](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103115729.png)

# wrapper容器

![image-20220103120008768](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103120010.png)

![image-20220103120511654](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103120513.png)

# 阀门Valve

![image-20220103120618056](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103120620.png)

阀门——记录访问日志

![image-20220103120846124](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103120847.png)

![image-20220103120953892](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103120955.png)

每个容器都有阀门

![image-20220103121059477](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103121101.png)

# Tomcat解析Http请求

![image-20220103121247963](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103121249.png)

![image-20220103121514826](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103121516.png)

![image-20220103121654117](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103121655.png)

![image-20220103121729927](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103121732.png)



![image-20220103121847860](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103121849.png)

![image-20220103122114767](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103122119.png)

![image-20220103122303531](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103122305.png)

![image-20220103122413597](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103122415.png)

![image-20220103132455629](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103132457.png)

![image-20220103132813991](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103132815.png)

![image-20220103132734893](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103132736.png)

![image-20220103134241343](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103134242.png)

# Tomcat与Socket的关系

![image-20220103134329424](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103134330.png)

![image-20220103134559738](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103134601.png)

![image-20220103134950810](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103134951.png)

# 整体结构及组件

![image-20220107093752388](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220107093801.png)

![image-20220107093928500](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220107093929.png)

![image-20220107094003380](C:/Users/86158/AppData/Roaming/Typora/typora-user-images/image-20220107094003380.png)

![image-20220107094058667](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220107094100.png)

![image-20220107094133513](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220107094134.png)

![image-20220107094657832](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220107094658.png)