---
ttitle: Spring Boot的配置文件和自动配置原理
categories: Spring Boot
---

# 使用Spring Initializer快速创建Spring Boot项目

IDEA：使用 Spring Initializer快速创建项目 

IDE都支持使用Spring的项目创建向导快速创建一个Spring Boot项目； 选择我们需要的模块；向导会联网创建Spring Boot项目； 默认生成的Spring Boot项目； 

- 主程序已经生成好了，我们只需要我们自己的逻辑 
- resources文件夹中目录结构 
  - static：保存所有的静态资源； js css images； 
  - templates：保存所有的模板页面；（Spring Boot默认jar包使用嵌入式的Tomcat，默认不支持JSP页 面）；可以使用模板引擎（freemarker、thymeleaf）
  - application.properties：Spring Boot应用的配置文件；可以修改一些默认设置；

# 自定义SpringApplication

如果SpringApplication默认设置不符合您的喜好，则可以创建一个本地实例并对其进行自定义。例如，要关闭横幅，您可 以编写：

```java
public static void main(String[] args) {
    SpringApplication app = new SpringApplication(MySpringConfiguration.class);
    app.setBannerMode(Banner.Mode.OFF);
    app.run(args);
}
```

通过构造者模式流式构造SpringApplication：

```java
public static void main(String[] args) {
    new SpringApplicationBuilder()
        .bannerMode(Banner.Mode.OFF)
        .run(args);
}
```

# 配置文件的使用

## 配置文件介绍

SpringBoot使用一个全局的配置文件核心配置文件，配置文件名在约定的情况下名字是固定的； 

配置文件的作用：修改SpringBoot自动配置的默认值；SpringBoot在底层都给我们自动配置好；

```
application.properties
application.yml 
application.yaml

YAML（YAML Ain't Markup Language） 
YAML A Markup Language：是一个标记语言 
YAML isn't Markup Language：不是一个标记语言；
```

两种配置文件的格式

在springboot框架中，resource文件夹里可以存放配置的文件有两种：properties和yml。

- application.properties的用法：扁平的k/v格式

```properties
server.port=8081
server.servlet.context‐path=/tuling
```

- application.yml的用法：树型结构

```yml
server:
	port: 8088
	servlet:
	context‐path: /tuling
```

两种前者是properties，而后者是yml的，建议使用后者，因为它的可读性更强。 可以看到要转换成YML我们只需把properies里按. 去拆分即可。

## yml基本语法

k:(空格)v：表示一对键值对（空格必须有）； 

以空格的缩进来控制层级关系；只要是左对齐的一列数据，都是同一个层级的 

属性和值也是大小写敏感； 如果有特殊字符% & 记得用单引号（‘）包起来

## 配置文件的加载顺序 

```xml
<includes>
<include>**/application*.yml</include>
<include>**/application*.yaml</include>
<include>**/application*.properties</include>
</includes>
```

如果同时存在不同后缀的文件按照这个顺序加载主配置文件；互补配置；

#### 外部约定配置文件加载顺序

springboot 启动还会扫描以下位置的application.properties或者application.yml文件作为Spring boot的默认配置文件

1.  classpath根目录下的

   ![image-20211030000148331](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130095236.png)

2. classpath根config/

   ![image-20211030000221224](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130095238.png)

3. 项目根目录

   如果当前项目是继承/耦合 关系maven项目的话，项目根目录=父maven项目的根目录

   ![image-20211030000259215](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130095242.png)

4. 项目根目录/config

   ![image-20211030000426420](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130095244.png)

5. 直接子目录/config

   ```powershell
    java ‐jar configuration_file‐0.0.1‐SNAPSHOT.jar ‐‐spring.config.location=D:\config/
   ```

优先级由低到高，高优先级的配置会覆盖低优先级的配置；互补配置；

官网：

```
 optional:classpath:/
 optional:classpath:/config/
 optional:file:./
 optional:file:./config/*/
 optional:file:./config/
 optional:classpath:custom‐config/ ‐‐spring.config.location
 optional:file:./custom‐config/ ‐‐spring.config.location
```

## Profile文件的加载

Profile的意思是配置，对于应用程序来说，不同的环境需要不同的配置。

SpringBoot框架提供了多profile的管理功能，我们可以使用profile功能来区分不同环境的配置。

### 多Profile文件

Spring官方给出的语法规则是application-{profile}.properties（.yaml/.yml）。

如果需要创建自定义的的properties文件时，可以用application-xxx.properties的命名方式， 根据实际情况，我创建了一个开发环境下使用的properties文件和一个生产环境下使用的properties文件，其中只对端口进行了配置，如下图所示

开发环境如下：

![image-20211030001210390](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130095252.png)

生产环境如下：

![image-20211030001408893](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130095255.png)

若我们需要在两种环境下进行切换，只需要在application.properties中加入如下内容即可

![image-20211030001353546](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130095257.png)

**先按照位置来读取优先级， 在同一位置下profile优先级最高， 如果没有指定profile, 先yml‐‐yaml‐‐properties**

### 激活指定profile

1.  在配置文件中指定 spring.profiles.active=dev

2. 命令行：

   ```powershell
   java ‐jar configuration_file‐0.0.1‐SNAPSHOT.jar ‐‐spring.profiles.active=dev；
   ```

   1. 还可以通过spring.config.location来改变默认的配置文件 使用spring.config.location 指定的配置文件，是不会进行互补

      ```powershell
      java ‐jar configuration_file‐0.0.1‐SNAPSHOT.jar ‐‐spring.config.location=D:/application.properties
      ```

   2. 还可以通过spring.config.name来改变默认的配置文件，是不会进行互补

      ```powershell
      java ‐jar configuration_file‐0.0.1‐SNAPSHOT.jar ‐‐spring.config.name=application‐prod
      ```

### 所有配置文件按以下顺序考虑： 优先级从低到高

打包在jar中配置文件 

打包在jar中profile 

打包的jar之外的配置文件 

打包的jar之外的profile

```
 java ‐jar configuration_file‐0.0.1‐SNAPSHOT.jar
 jar包之外的配置文件 yml‐‐>yaml‐‐>properties
 optional:classpath:/config/ yml‐‐>yaml‐‐>properties
 optional:classpath:/ yml‐‐>yaml‐‐>properties
```



