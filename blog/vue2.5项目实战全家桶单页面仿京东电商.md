---
title: vue2.5项目实战全家桶单页面仿京东电商
categories: 前端
tags: [Vue]
---

# 简介

## 全家桶项目成员及其在项目中的定位与作用

**项目构建工具vue-cli**
vue-cli是一个脚手架工具，为我们搭建了开发所需要的环境和生成目录架构
**路由vue-router**
创建单页应用，我们的单页应用只做路由切换，组件拼凑成的页面映射成路由
路由是我们单页应用的核心插件
**状态管理vuex**
状态管理库，可理解为全局数据集中地
推荐小项目尽量别用vuex，会显得有点繁琐，bus总线机制完全可以处理了
**http请求工具axios（ vue-resource官方已停止维护）**
一个经过封装的ajax,可以根据自己的项目情况再进行封装
axios是经过了ES6的promise封装的

## 前后端联调必备技术之Mock

- 什么是Mock数据？
  - 处于开发环境模拟接口返回的数据（用于开发状态后端还没给接口）
  - 不会影响生产环境，只是方便我们还没与后端交互时不阻塞我们开发流程
- Mock数据的好处
  - 团队可以并行工作（后端进度不至于影响前端开发进度）
  - 可以用来演示开发成果，实时反馈开发进度
  - 模拟测试并简单了解接口编写为全栈打基础

# 搭建vue单页面应用工程架构

## 对比vue-cli2.x和vue-cli3.x的搭建

搭建前提条件：

1. node环境

2. 安装webpack

3. 安装vue-cli 2.x

   npm install vue-cli -g
   创建项目：vue init webpack 项目名(不要取中文名字)

4. 安装vue-cli 3.x

   npm install @vue/cli -g
   创建项目：vue create 项目名（不要取中文名字）
   
   ![image-20211221104645463](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211221104647.png)

不添加指定版本号就是下载最新稳定版本

## vue-cli2.x和vue-cli3.x的项目架构

vuecli3.x:
去掉了2.x build和config等目录，大部分配置 都集成到vue.config.js这里了

vue.config.js里大概包括了配置常用的输出路径名、跟目录、预处理、devServer配置、pwa、dll、第三方插件等等

具体配置可参考(https://www.cnblogs.com/zjhr/p/9472648.html)

因为绝大部分的配置和扩展尤大大已经做好了封装的了，我们常用的开发基本可以满足，不满足的我们自己可以自行去扩展

webpack的配置在这个属性里修改configureWebpack（Mock也是在这里面的）

## 搭建项目→启动项目

```powershell
Vue CLI v4.5.15
? Please pick a preset: Manually select features
? Check the features needed for your project: Choose Vue version, Babel, Router, Vuex, CSS Pre-processors
? Choose a version of Vue.js that you want to start the project with 
2.x
? Use history mode for router? (Requires proper server setup for index fallback in production) Yes
? Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported by default): Stylus
? Where do you prefer placing config for Babel, ESLint, etc.? In dedicated config files
? Save this as a preset for future projects? No


Vue CLI v4.5.15
✨  Creating project in F:\ideaworkspace\vuedemo\xiaod3.x.
🗃  Initializing git repository...
⚙️  Installing CLI plugins. This might take a while...


19 packages are looking for funding
  run `npm fund` for details
🚀  Invoking generators...
📦  Installing additional dependencies...


added 16 packages in 6s

19 packages are looking for funding
  run `npm fund` for details
⚓  Running completion hooks...

📄  Generating README.md...

🎉  Successfully created project xiaod3.x.
👉  Get started with the following commands:

 $ cd xiaod3.x
 $ npm run serve

PS F:\ideaworkspace\vuedemo> cd xiaod3.x
PS F:\ideaworkspace\vuedemo\xiaod3.x> npm run serve

> xiaod3.x@0.1.0 serve
> vue-cli-service serve

 INFO  Starting development server...
98% after emitting CopyPlugin

 DONE  Compiled successfully in 3481ms             11:48:10 ├F10: AM┤

  App running at:
  - Local:   http://localhost:8080/
  - Network: http://10.23.24.24:8080/

  Note that the development build is not optimized.
  To create a production build, run npm run build.
```



## ui库选择



分析市场上各种各样的ui框架及如何选择适合自己项目的ui框架，vue是一套渐进式的框架，设计的时候就是自底层向上组层应用的。

PC端的ui库基本不用做选型了，ElementUI的霸主地位无人能撼动

移动端的选型看好几点即可

- 能否自定义皮肤
- 是否使用rem控制尺寸，完美适应不同分辨率移动设备
- 组件类型风格是否与自己的项目相同或类似
- 单元测试覆盖率
- 更新频率的快慢

移动端的框架有哪些：

mint-ui vux vant cube-ui

有赞：https://youzan.github.io/vant/#/zh-CN/intro

滴滴：https://didi.github.io/cube-ui/#/zh-CN/docs/switch

## 安装使用ui框架-----cube-ui

vuecli3.x版本安装：vue add cube-ui

cube-ui:https://didi.github.io/cube-ui/#/zh-CN/docs/quick-start

安装cube-ui的配置选择：https://github.com/cube-ui/cube-template/wiki



# 注册登录

## 实现注册页面

## vue.config.js配置Mock数据

进行webpack配置扩展之配置Mock

## 本地配置mock编写注册接口

## 请求插件axios模拟完成注册

## 撰写路由配置文件并复习cube-ui表单实现登录页面

## Mock编写登录接口并返回token

## 深入剖析vuex并存储token

引入全家桶成员vuex并进行token的处理

vuex与本地存储的配合

## 编写axios请求拦截配置

全局拦截配置及内部可实现内容(把token在请求头带回给后端以及可以根据状态码进行一系列处理)



# 商城首页+分类页

## 使用cube-ui实现轮播图并进行内部扩展

## cube-ui实现商品类别滑动组件

## 嵌套路由的使用

## 路由跳转如何加上一个过渡效果提高用户体验

**更贴近原生app的体验优化**

## cube-ui如何仿照京东实现分类页

## 根据不同屏幕动态获取滚动盒子高度

**动态获取滚动盒子高度，兼容不同屏幕高度场景**

## 路由拦截搭配路由属性使用方案

**使用路由配置之meta属性配合路由守卫进行页面权限拦截**



# 购物车+个人中心页

## 实现购物车页面

**编写购物车页面讲解购物车内部功能**

## 分类页商品添加基本逻辑及显示购物车数量

**使用vuex配合实现添加商品到购物车以及底部导航栏实时显示购物车数量**

## 配合vuex实现购物车内部功能

**使用vuex实现购物车内部的增加、减少以及清空购物车功能**

## vuex核心知识之配合内置方法实现购物车数据持久化

## 添加购物车动画实现思路

## 个人中心实现及注销登录功能



# 项目打包

## 打包项目给后端进行处理

## 理解MVVM和组件带出来的面试题

**MVVM开发模式及组件通信**

**谈谈你对MVVM开发模式的理解?**

- MVVM分为Model、View、ViewModel三者
  - Model：代表数据模型，数据和业务逻辑都是在Model层中定义
  - View：代表UI视图，负责对数据的展示
  - ViewModel：负责监听Model中数据的改变并控制视图的更新，处理用户交互操作

Model和View并无直接关联，而是通过ViewModel来进行联系的，Model和ViewModel之间有着双向数据绑定的联系。因此当Model中的数据改变时会触发View层的刷新，View中由于用户交互操作而改变的数据也会在Model中同步。

这种模式实现了Model和View的数据自动同步，因此开发者只需要专注对数据的维护操作即可，而不需要自己操作dom。

**对于组件通信你了解多少，请描述一下你是怎么完成组件的通信的?**

- 父传子用 props传递
- 子传父用$emit传递
- 非父子之间的传值 建立一个空实例进行传值，中央事件总线机制
- 祖孙之间的传值可以利用provide inject模式

**VUEX可以处理上述的每一个情况**

## 单页面应用首屏加载时间过长如何优化及输入网址到渲染完成过程

关于单页应用首屏加载速度慢，出现白屏时间过长问题你怎么处理？

- 将公用的JS库通过script标签在index.html进行外部引入，减少我们打包出来的js文件的大小，让浏览器并行下载资源文件，提高下载速度
- 在配置路由的时候进行路由的懒加载，在调用到改路由时再加载次路由相对应的js文件
- 加一个首屏loading图或骨架屏，提高用户的体验
- 尽可能使用CSS Sprites和字体图标库
- 图片的懒加载等

从输入网址到网页渲染完成经历了什么？

- 输入网址按回车键或点击跳转
- 发送到DNS服务器进行DNS解析，获取到我们对应web服务器对应的ip地址
- 与Web服务器建立TCP连接
- 浏览器向web服务器发送http请求
- Web服务器进行响应请求并返回指定的url数据（当然这里也可能是错误信息或者重定向到新的url地址等）
- 浏览器下载web服务器返回的数据及解析html源文件
- 根据文件生成DOM树和样式树合成我们的渲染树，解析js，最后渲染我们的页面然后显示出来

## 修改数据视图试图不更新情况该怎么处理和如何监听数据

```
1. 关于修改了数据，视图不更新的理解和处理方式
Vue中给data中的对象属性添加一个新的属性时会发生什么
经过打印发现数据是已经改变了，但是由于在Vue实例创建时， 新添加的属性并未声明，因此就没有被Vue转换为响应式的属性，自然就不会触发视图的更新，这时就需要使用Vue的全局api——> $set()

$set()使用方法：

$set(需要修改的对象，"对象的属性",值)

2. 在vue里面你如何做数据的监听
watch里面监听：第一种写法

watch:{
	obj(newval,oldval){
		console.log(newval,oldval)
	},
}

第二种写法可设置deep为true对数据进行深层遍历监听

watch:{
	obj:{
		handler(newval,oldval){
			console.log(222) 
			console.log(newval,oldval)
		},
		deep:true
	}
}

computed 里面监听：computed里面的依赖改变时，所计算的属性或作出事实的改变
```

# vue全家桶项目总结

- 全家桶成员登场及介绍各自的作用（vue-cli、vue-router、axios、vuex）
- 新知识Mock自己编写本地运行接口返回数据
- 开始利用脚手架快速构建我们的工程项目（分别介绍了vue-cli2.x和vue-cli3.x构建命令的区别）
- 讲解vue-cli2.x和vue-cli3.x构建出来的项目架构的区别
- 面对市场上花样百出的ui库如何选择质量高适合自己项目的ui库
- 使用cube-ui开始我们的项目之旅（tips：多看api文档就能熟练使用ui库）
- 使用Mock编写我们的接口并返回我们需要的信息
- 介绍那种场景下使用嵌套路由以及如何实现
- 介绍token的用途和编写axios请求的全局拦截和路由守卫（轻易实现权限控制）
- 利用vuex和本地存储的配合实现购物车的功能
- 进行项目的性能优化及体验优化(优化首屏加载、路由跳转过渡效果、购物车动画效果)
- 打包项目及详解面试关于vue的高频面试题

路线：

html5→css3→js→es6→ps切图基础→vue→react→angular→小程序生态→webpack→http知识→node.js→express、koa→Linux→mysql、mongoDB、redis→Git和持续集成→Docker→JS高级→前端性能优化→浏览器知识→前端安全性问题→项目实战

#### 

 