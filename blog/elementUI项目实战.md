---
title: elementUI项目实战
categories: 前端
tags: [elementUI]
---

# 核⼼知识

需要对webpack进⾏配置的话，要⼿动在根 ⽬录新建⼀个vue.config.js⽂件

## vue.config.js常⽤配置

```js
// vue.config.js 常⽤配置
module.exports = {
 // 基本路径, vue.cli 3.3以前请使⽤baseUrl
 publicPath: '/',
 // 输出⽂件⽬录
 outputDir: 'dist',
// ⽤于嵌套⽣成的静态资产（js，css，img，fonts）的⽬录。
 assetsDir: '',
 // ⽣产环境sourceMap
 productionSourceMap: true,
 // webpack配置
 configureWebpack: () => {},
 chainWebpack: () => {},
 // css相关配置
 css: {
 // 启⽤ CSS modules
 modules: false,
 // 是否使⽤css分离插件
 extract: true,
 // 开启 CSS source maps?
 sourceMap: false,
 // css预设器配置项
 loaderOptions: {},
 },
 // webpack-dev-server 相关配置
 devServer: {
 host: '0.0.0.0',
 port: 8080,
 proxy: {}, // 设置代理
 },
 // 第三⽅插件配置
 pluginOptions: {
 // ...
 }
}
```



## vue中组件间传值常用的几种方式

```
⽗⼦组件传值 

props / $emit ⼦组件中通过定义props接收⽗组件中通过v-bind绑定的数据 ⽗组件中通过监听⼦组件中$emit的⾃定义事件接收数据 
$parent / children ⼦组件中通过this.$parent这个对象获取⽗组件中的数据 ⽗组件中通过this.$children这个数组获取⼦组件中的数据 
$ref ⽗组件中定义⼦组件中的ref属性后，通过this.$refs.定义的属性名获取⼦组件数据
```



![image-20211222171457144](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211222171459.png)

![image-20211222170611468](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211222170612.png)

在mounted里面的两种方式都可以得到子组件，这两种方式不一样的地方就是ref给组件命名了，容易找到这个组件

![image-20211222171033507](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211222171036.png)

⾮⽗⼦间传值

```js
//事件总线
// 原理上就是建⽴⼀个公共的js⽂件，专⻔⽤来传递消息
// bus.js
import Vue from 'vue'
export default new Vue;
// 在需要传递消息的地⽅引⼊
import bus from './bus.js'
// 传递消息
bus.$emit('msg', val)
// 接受消息
bus.$emit('msg', val => {
 console.log(val)
})
```

![image-20211222220637800](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211222220647.png)

![image-20211222220919287](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211222220920.png)

![image-20211222221148881](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211222221150.png)

## 单页面应用的控制中心—vue-router

**路由的基本配置** 

基本参数 path 路由的访问路径。即url 。component 访问路径对应的组件 

扩展参数 name 路由指定命名，设置后可⽤params传参及使⽤name进⾏路由跳转

**路由的跳转** 

- router-link标签跳转 

- 编程式导航

  ```js
  // route可以是对象，或者是字符串
  // 对象的时候可通过路由的path或者name进⾏跳转
  // 字符串的话只能是路由的path
  this.$router.push(route)
  // 路由传递参数, query和path配合， params和name配合
  query: this.$router.push({path: '/', query: {id: 2}})
  params: this.$router.push({name: 'home', params: {id: 2}})
  ```

**动态路由** 

什么是动态路由 组件是同⼀个，只是通过不同的url参数渲染不同的数据 路径参数"使⽤冒号" : 标记

```
{
 path: '/home/:id',
 component: home
}
```

在path⾥显式声明后，通过params传参后，参数不丢失同时参数被设置成必传参数

![image-20211223104336642](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223104338.png)

![image-20211223104254111](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223104256.png)

![image-20211223104414389](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223104415.png)

**嵌套路由**

⽬的： 组件中嵌套不同组件 

实现

```
// 在需要嵌套的路由中补充children字段
{
 path: '/home/:id',
 component: home,
 children: []
}
```

![image-20211223104538169](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223104539.png)

![image-20211223105034678](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223105036.png)

**导航守卫** 

通过router中的beforeEach注册全局守卫，每次切换路由时触发

```
// to, from是路由对象，我们在路由⾥定义的参数都可以在这⾥取到，例如to.path或
from.name
router.beforeEach((to, from, next) => {
 // ...
next()
})
```

参数 

to： 将进⼊的路由对象 

from： 将离开的路由对象 

next() 确认完成操作，最后⼀定要调⽤，不然路由就不会进⾏切换

![image-20211223105238643](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223105240.png)

**路由懒加载** 

提⾼⻚⾯加载速度 

避免进⼊项⽬后加载全部组件 

在路由中的component中设置函数，⽤import⽅式进⾏使⽤

```
component: () => import('./views/Home.vue'),
```

## 状态管理中心—vuex的基础和高级用法

State 数据，存放⼀些公⽤部分的数据 

Mutations 数据怎么改变，定义改变state的⼀些⽅法 

Actions 异步改变， 如果需要异步改变state，则在这书写

![image-20211223094219835](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223094221.png)

vuex⾥包含的基本参数

```
export default {
 // 组件间公共数据部分
 state: {},
 // 需要改变state中的数据时，要在mutation⾥定义改变的⽅法
 mutations: {},
 // 当改变state中的数据是异步操作时，在action⾥定义
 actions: {}
}

```

![image-20211223105726975](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223105728.png)

![image-20211223110017668](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223110019.png)

![image-20211223110148065](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223110149.png)

![image-20211223110426847](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223110428.png)

![image-20211223110655125](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223110656.png)

![image-20211223110741436](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223110743.png)

**vuex中的计算属性—Getters** 

当你需要依赖vuex⾥的state中的数据，做进⼀步处理时使⽤

```
state: {
 count: 0,
},
// 根据state中的count进⼀步处理，计算双倍值
getters: {
 doubleCount (state) {
 return state.count * 2
 }
},
```

![image-20211223111300102](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223111301.png)

**模块化概念—Modules**

当vuex⾥的数据⼗分庞⼤时，可根据存放的数据所属模块进⾏划分

```js
import Vue from 'vue'
import Vuex from 'vuex'
// 第⼀步 引⼊模块
import text from './text'
Vue.use(Vuex)
// 第⼆步 在初始化store时，加载模块
export default new Vuex.Store({
    modules: {
        text
    }
})
```

**count模块的内容**

![image-20211223111957042](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223111958.png)

![image-20211223111526182](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223111527.png)

![image-20211223111639677](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223111641.png)

![image-20211223111808504](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211225183818.png)

![image-20211223111842214](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223111843.png)

# Element常⽤组件

## 布局组件

yarn add element-ui -S

![image-20211223112235430](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223112236.png)

![image-20211223112433090](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211225183832.png)

![image-20211223112528114](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223112529.png)

![image-20211223112942353](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223112944.png)

![image-20211223113054109](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223113055.png)

![image-20211223113337737](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223113339.png)

布局组件 el-row、el-col el-container、el-header、el-aside、el-main、el-footer

常⽤属性 span gutter(分隔作用的)

如何构建5等分布局

布局⼩试

## 弹出类型组件

常⽤弹出组件 el-dialog el-popover 

sync修饰符的作⽤ 示例：ElementUI中的el-dialog

```
// 第⼀种写法
<el-dialog :visible.sync="dialogVisible">
// 第⼆种写法
<el-dialog :visible="dialogVisible" :before-close="beforeClose">
 
// 第⼀种写法关闭或是点击空⽩处⽆需特别处理，el-dialog组件内部会修改当前值状态，通
过.sync修饰符传递给⽗组件；
// 第⼆种写法，需要再beforeClose⽅法内⼿动处理this.dialogVisible = false。
```

dialog组件中的插槽 ：title footer

![image-20211223114305665](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223114306.png)

![image-20211223114238643](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223114239.png)

![image-20211223115121222](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223115122.png)

![image-20211223115225599](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223115227.png)

![image-20211223115514904](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223115516.png)

## 表格组件

基础表格 在 Table 组件中，每⼀个表格由⼀个 Table-Column 组件构成，也就是表格的列

**表格常⽤属性介绍**

height 给表格设置⾼度，同时固定表头 

show-header 设置是否显示表头 

row-class-name 设置⼀个函数或者固定的名字作为⾏的类名 

border 是否显示表格竖直⽅向的边框，设置后可通过改变边框设置列宽

**列常⽤属性介绍**

label 当前列的表头名称 

prop 传⼊的表格json数据的key值 

show-overflow-tooltip 是否设置⽂字超出列宽时悬浮显示完整内容

**通过v-for封装适⽤性更好的表格**

![image-20211223130225793](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223130228.png)

![image-20211223130445126](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223130446.png)

![image-20211223130852963](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223130856.png)

![image-20211223131019863](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223131021.png)

```
props / $emit ⼦组件中通过定义props接收⽗组件中通过v-bind绑定的数据 ⽗组件中通过监听⼦组件中$emit的⾃定义事件接收数据 
```

![image-20211223131806913](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223131808.png)

![image-20211223132104087](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223132105.png)

## 表单组件

**基础表单** 在 Form 组件中，每⼀个表单域由⼀个 Form-Item 组件构成，Form-item可以是下拉框、输 ⼊框、⽇期选择器等各种表单组件 

**添加表单验证** 封装表单组件 观察基础表单 **总结⼀个表单组件动态的参数有哪些** 

最基础：label，model、type 

扩展：rule、placeholder、其他配置(⾃动补全，可清除等) 定义循环的数据结构 数组对象

**封装表单组件**

![image-20211223140616559](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223140617.png)

![image-20211223140755520](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223140756.png)

![image-20211223141247697](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223141248.png)

**form对象访问属性可以用[]这个来访问，有点奇特**

![image-20211223143142875](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223143147.png)

![image-20211223142934169](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223142935.png)





# 实战项⽬之环境准备及配置改装

## 项⽬搭建及技术选型

通过命令⾏创建项⽬ Vue create <项⽬名> ⼿动配置项⽬，上下键移动光标后，回⻋即可

![image-20211223095044017](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223095045.png)

选择需要的配置 

babel转译js的新特性，兼容低版本浏览器 

CSS预处理器，设置全局变量 

ESLint检查代码写法是否规范

![image-20211223095110670](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223095111.png)

是否使⽤

## 配置项⽬的基本环境及项⽬⽬录结构

配置项⽬按eslint规范格式化代码 vscode下载 ESlint , Prettier , Vetur 插件 

打开vscode的设置

![image-20211223095158155](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223095159.png)

选择扩展中的ESLint，之后点击在setting.json中编辑

![image-20211223095212669](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223095214.png)

添加如图配置

![image-20211223095238734](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223095240.png)

**可⾃定义eslint的⼀些规则** 

在 .eslintrc 中覆盖prettier规则即可，覆盖是为了防⽌冲突 

在rules⾥配置

```
rules: {
    'no-console': process.env.NODE_ENV === 'production' ? 'error'
    : 'off',
    'no-debugger': process.env.NODE_ENV === 'production' ? 'error'
    : 'off',
    // 添加⾃定义规则
    'prettier/prettier': [
        // eslint校验不成功后，error或2则报错，warn或1则警告，off或0则⽆提示
        'error',
         {
           singleQuote: true,
           semi: false
         }
	]
},
```

- 配置完成后可运⾏ npm run lint 格式化全部⽂件，或者保存后⾃动格式化代码 
- router部分模块化 
- vuex部分模块化 
- 调整项⽬⽬录 
- 删除多余代码

**项⽬⽬录结构调整** 

**配置scss全局变量** 

- 在项⽬的根⽬录下新建 vue.config.js 
- 新建 _variable.scss ⽂件 
- 在 vue.config.js ⽂件进⾏如下配置

```vue
module.exports = {
    // 配置项⽬启动端⼝及⾃动打开浏览器
    devServer: {
        port: 3333,
        open: true
    },
    // 配置scss全局变量
    css: {
        loaderOptions: {
            sass: {
                data: `@import "@/assets/scss/_variable.scss";`
            }
        }
    }
}

```

# 后台视频管理系统之公⽤部分开发

## 需求分析及模块划分

**分析视频管理后台需要的功能** 

- 可视化展示数据 

  - 视频的成交量 

  - ⽤户总量 

  - 订单总额 

- 登录⻚ 

- 视频管理 

  - 上传视频 

  - 更新视频 

  - 删除视频 

  - 查看已有视频 

- ⽤户管理 

  - 更新⽤户信息 

  - 删除⽤户

  - 新增⽤户 

  - 权限管理 

**设计对应⻚⾯** 

⾸⻚⽤来展示数据 

- 使⽤Echart柱状图、折线图及饼图展示 

视频管理⻚、⽤户管理⻚ 

- 选⽤el-table及el-form展示和编辑数据 
- el-dialog组件实现编辑和新增功能

## 路由设计及左侧公⽤导航菜单开发

新建 Main.vue 组件放置公共部分组件

```vue
<template>
<el-container>
    <el-aside><common-aside></common-aside></el-aside>
    <el-container>
        <el-header><common-header></common-header></el-header>
        <common-tab></common-tab>
        <el-main>
            <router-view />
    </el-main>
    </el-container>
    </el-container>
</template>
<script>
    import CommonHeader from '../components/CommonHeader'
    import CommonAside from '../components/CommonAside'
    import CommonTab from '../components/CommonTab'
</script>
```

建⽴⻚⾯组件 

- 在 views/Home 下建⽴ Home.vue 
- 在 views/UserManage 下建⽴ UserManage.vue 
- 在 views/VideoManage 下建⽴ VideoManage.vue 
- 在 views/Other 下建⽴ PageOne.vue 、 PageTwo.vue 

在 router/index.js 中引⼊路由

```
{
 // 主要配置3个参数,访问的路由地址，路由名字及懒加载加载哪个组件
 path: '',
 // name属性⽤于路由跳转及params传参
 name: '',
 component: ''
}
```

1. ```js
   // 完整路由代码
   export default new Router({
       routes: [
           {
               path: '/',
               component: () => import('@/views/Main'),
               children: [
                   {
                       path: '/',
                       name: 'home'
                       component: () => import('@/views/Home/Home'),
           		},
                   {
                       path: '/user',
                       name: 'user'
                       component: () => import('@/views/UserManage/UserManage'),
   			   },
                   {
                       path: '/video',
                       name: 'video'
                       component: () => import('@/views/VideoManage/VideoManage'),
   			   },
                   {
                       path: '/page1',
                       name: 'page1'
                       component: () => import('@/views/Other/PageOne'),
                   },
                   {
                       path: '/page2',
                       name: 'page2'
                       component: () => import('@/views/Other/PageTwo'),
                   },
               ]
           }
       ]
   })
   ```

## 顶部导航菜单及与左侧导航联动的⾯包屑实现(上)

核⼼点 使⽤vuex进⾏传值

```js
// tab.js
export default {
    state: {
        isCollapse: false,
        currentMenu: null,
        menu: [],
        tabsList: [
            {
                path: '/',
                name: 'home',
                label: '⾸⻚',
                icon: 'home'
            }
        ]
    },
    mutations: {
        selectMenu(state, val) {
            if (val.name !== 'home') {
                state.currentMenu = val
                let result = state.tabsList.findIndex(item => item.name === val.name)
                result === -1 ? state.tabsList.push(val) : ''
            } else {
                state.currentMenu = null
            }
            // val.name === 'home' ? (state.currentMenu = null) : (state.currentMenu
            = val)
        },
    },
    actions: {}
}
```

## 顶部导航菜单及与左侧导航联动的⾯包屑实现(下)

## vuex中的 tab.js

```js
// tab.js
export default {
    state: {
        isCollapse: false,
        currentMenu: null,
        menu: [],
        tabsList: [
            {
                path: '/',
                name: 'home',
                label: '⾸⻚',
                icon: 'home'
            }
        ]
    },
    mutations: {
        closeTab(state, val) {
            let result = state.tabsList.findIndex(item => item.name === val.name)
            state.tabsList.splice(result, 1)
        },
    },
    actions: {}
}
```

## 使⽤vuex实现切换tab⻚功能

- 在 Main.vue 中引⼊ CommonTab.vue 组件 
- 在vuex⾥定义存取标签的 tagList ，⽅便⾮⽗⼦传递数据 
- 定义 vuex 中侧边栏点击后将菜单加⼊到 tagList 中的⽅法 
- 定义 vuex 中点击标签后触发删除的⽅法

## 构建⻚⾯组件，连通公共组件

- 建⽴每个⻚⾯组件 
- 连通⾯包屑 
- 连通侧边栏 
- 连通标签栏

## ⻚⾯布局整体样式优化

- 侧边栏背景⾊改变 
- tag选中样式优化 
- ⾯包屑当前激活菜单样式优化 
- 细节优化

# 后台视频管理系统之⾸⻚开发

## 介绍mock.js及axios全局配置

Mock.js 

- 作⽤：⽣成随机数据，拦截ajax请求 

- 安装：

  ```
  yarn add mockjs
  npm install mockjsMock.js
  ```

- 核⼼： Mock.mock()

```
// 根据数据模板⽣成模拟数据
Mock.mock( rurl, rtype, template)
/*
** rurl: 表示需要拦截的 URL，可以是 URL 字符串或 URL 正则
** eg. /\/domain\/list\.json/、'/domian/list.json'
*/
```

- Mock.Random() 随机⽣成数据

## 使⽤Mock随机返回主⻚数据

- 新建 mock ⽂件夹 
- 配置mock请求时间 
- 新建 home.js 存放主⻚的数据 
- 安装mock的语法⽣成数据

## 使⽤element布局组件实现⾸⻚布局

![image-20211223101054537](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223101055.png)

分析布局 选择组件

## 完成⾸⻚除图表外的内容

定义右上⻆统计数据格式

```js
countData: [
    {
        name: '今⽇⽀付订单',
        通过v-for循环，渲染⻚⾯
        value: 1234,
        icon: 'success',
        color: '#2ec7c9'
    },
    {
        name: '今⽇收藏订单',
        value: 210,
        icon: 'star-on',
        color: '#ffb980'
    },
    {
        name: '今⽇未⽀付订单',
        value: 1234,
        icon: 's-goods',
        color: '#5ab1ef'
    },
    {
        name: '本⽉⽀付订单',
        value: 1234,
        icon: 'success',
        color: '#2ec7c9'
    },
    {
        name: '本⽉收藏订单',
        value: 210,
        icon: 'star-on',
        color: '#ffb980'
    },
    {
        name: '本⽉未⽀付订单',
        value: 1234,
        icon: 's-goods',
        color: '#5ab1ef'
    }
],
```

通过v-for循环，渲染⻚⾯

```html
<div class="num">
    <el-card shadow="hover" v-for="item in countData"
             :key="item.name" :body-style="{display: 'flex', padding: 0 }">
        <i class="icon" :class="`el-icon-${item.icon}`"
           :style="{ background: item.color }"></i>
        <div class="detail">
            <p class="num">￥ {{ item.value }}</p>
            <p class="txt">{{ item.name }}</p>
        </div>
    </el-card>
</div>
```

## 完成⾸⻚table部分及ECharts

ECharts 

⼀个使⽤ JavaScript 实现的开源可视化库，通过这个库可实现可视化展示数据 

快速⼊⻔ 先设置ECharts要在哪显示，设置EChart容器

```html
<body>
    <!-- 为 ECharts 准备⼀个具备⼤⼩（宽⾼）的 DOM -->
    <div id="main" style="width: 600px;height:400px;"></div>
</body>
```

通过 echarts.init ⽅法初始化⼀个 echarts 实例并通过 setOption ⽅法⽣成⼀个简单的 柱状图

```js
// data中声明echart对象
// 通过ref获取dom，初始化echarts实例
this.echart = echarts.init(this.$refs.ref名字);
// 指定图表的配置项和数据
var option = {
    title: {
        text: 'ECharts ⼊⻔示例'
    },
    tooltip: {},
    legend: {
        data:['销量']
    },
    xAxis: {
        data: ["衬衫","⽺⽑衫","雪纺衫","裤⼦","⾼跟鞋","袜⼦"]
    },
    yAxis: {},
    series: [{
        name: '销量',
        type: 'bar',
        data: [5, 20, 36, 10, 10, 20]
    }]
};
// 使⽤刚指定的配置项和数据显示图表。
this.echart.setOption(option);
```

![image-20211223101355281](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223101356.png)

## 封装⼀个EChart组件的⼀些想法

- 图表的⼤分类 
- 组件的⼀些动态参数抽离 
- 组件的基本配置 
- 主题颜⾊

```
color: [
 '#2ec7c9',
 '#b6a2de',
 '#5ab1ef',
 '#ffb980',
 '#d87a80',
 '#8d98b3',
 '#e5cf0d',
 '#97b552',
 '#95706d',
 '#dc69aa',
 '#07a2a4',
 '#9a7fd1',
 '#588dd5',
 '#f5994e',
 '#c05050',
 '#59678c',
 '#c9ab00',
 '#7eb00a',
 '#6f5553',
 '#c14089'
]
```

## 上⼿封装⼀个EChart组件并处理数据展示图表

有坐标轴的图表 数据格式

```
// 类⽬轴
xAxis: {
 data: [],
 type: 'category',
},
// 数据部分
yAxis: {
 type: 'value'
},
series: []
```

没坐标轴的图表 数据格式

```
{
 series: []
}
```

## 修改EChart组件样式及⾃适应图表(上)

完善图表选项

```js
axisOption: {
    tooltip: {
        trigger: 'axis'
    },
    legend: {
         textStyle: {
             //图例⽂字的样式
             color: '#333'
         }
 },
     grid: {
         left: '20%'
     },
         color: [
             '#2ec7c9',
             '#b6a2de',
             '#5ab1ef',
             '#ffb980',
             '#d87a80',
             '#8d98b3',
             '#e5cf0d',
             '#97b552',
             '#95706d',
             '#dc69aa',
             '#07a2a4',
             '#9a7fd1',
             '#588dd5',
             '#f5994e',
             '#c05050',
             '#59678c',
             '#c9ab00',
             '#7eb00a',
             '#6f5553',
             '#c14089'
         ],
             xAxis: {
                 data: [],
                 type: 'category',
                 axisLine: {
                    lineStyle: {
                    color: '#17b3a3' //x轴颜⾊
                    }
                 },
                  // 字体倾斜
                  axisLabel: {
                     color: '#333'
                  }
             },
             yAxis: {
                  type: 'value',
                  axisLabel: {
                  	formatter: '{value} '
                  },
                  axisLine: {
                     lineStyle: {
                        color: '#17b3a3' //坐标轴颜⾊
                     }
                  },
                  splitLine: {
                      show: true,
                      lineStyle: {
                          color: ['#eee'],
                          width: 1,
                          type: 'solid'
                 	 }
				}
 			},
 		series: []
 },
```

## 修改EChart组件样式及⾃适应图表(下)

完善组件样式

```js
normalOption: {
    tooltip: {
        trigger: 'item'
    },
    color: ['#0f78f4', '#dd536b', '#9462e5', '#a6a6a6', '#e1bb22',
                '#39c362', '#3ed1cf'],
    series: []
}
```

需要⾃适应的地⽅ 浏览器窗⼝⼤⼩发⽣变化 折叠菜单的时候 

#  Echart组件封装总结

- 观察⽂档，考虑组件需要的基本参数 
- 参数筛选，分为从⽗组件传来的参数和⾃身的参数 
- 完善组件，观察设计图，找不同，在⽂档中寻找对应的配置项 
- 细节优化，考虑多种场景下，图表⾃适应的处理

# ⽤户管理⻚及详解权限管理

## ⽤户管理⻚

![image-20211223102416289](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211223102417.png)

管理⻚功能：新增⽤户 搜索⽤户 更新⽤户 删除⽤户 分⻚展示⽤户列表

分析⻚⾯组成 考虑组件设计

## 更完善的表单组件封装

分析表单组成 输⼊框 下拉框 ⽇期选择器 单选列表 多选列表

考虑基本参数 绑定的表单字段 表单描述⽂本 

插槽拓展组件 按钮组

## 通⽤表格组件封装

表格基本参数分析 

- data： 传⼊的数据列表 
- prop： 传⼊的数据字段 
- label： 列名 

表格可选参数分析 

- width：列宽 
- type：类型 

表格扩展 

- 分⻚参数 total： 数据条数总计 page： 当前⻚数 
- 加载状态 loading：布尔值

## 完成表格组件的封装

- 调整表格样式，固定表格⾼度 
- 添加操作列

```html
<el-table-column label="操作" min-width="180">
    <template slot-scope="scope">
        <el-button size="mini" @click="handleEdit(scope.row)">编辑</el-button>
        <el-button size="mini" type="danger" @click="handleDelete(scope.row)">删
            除</el-button>
    </template>
</el-table-column>
```

- 添加分⻚组件

```html
<el-pagination
               class="pager" layout="prev, pager, next"
               :total="config.total" :current-page.sync="config.page"
               @current-change="changePage">
</el-pagination>
```

## ⽤户管理⻚⻚⾯功能实现(上)

表格加载状态 

分⻚功能

编辑功能 

删除功能

## ⽤户管理⻚⻚⾯功能实现(下)

表格加载状态 elementUI中 v-loading 的使⽤ 

分⻚功能 

编辑功能 

删除功能

## 企业开发之权限管理

什么是权限管理 

- 根据不同⽤户，返回不同菜单 

- 严格控制⽤户权限 

- 实现思路 

  - 动态路由 

  - 后端返回的数据格式要求 

  - 触发时机 

    - 登陆成功的时候触发操作 

    - Cookie中存在对应数据，⾸次进⼊⻚⾯时

## 权限管理之动态返回菜单的实现

更改路由表 根据是否需要权限的路由分类 

vuex⾥补充mutation 

- 保存菜单 
- 动态添加菜单 

⽣成路由的时机 

- 登录时 
- 刷新时 

点击退出时，清除cookie后，刷新下⻚⾯

## 权限管理之路由守卫判断⽤户登录状态

permission.js 中返回token 

登录时保存token 

- 保存在vuex⾥ 
- 保存在cookie⾥ 

路由守卫根据判断token存不存在，判断⽤户⻚⾯跳转

```js
// 判断⽤户登录状态，未登录跳转到登录⻚，已登录跳转到⾸⻚
router.beforeEach((to, from, next) => {
    // 从cookie⾥获取token，防⽌刷新后token丢失
    store.commit('getToken')
    let token = store.state.user.token
    if (!token && to.name !== 'login') {
        next({ name: 'login' })
    } else {
        next()
    }
})
```

# 项⽬总结

项⽬当中遇到的坑以及解决思路

- 通过vue-devtool调试 
- 通过console输出调试

组件的封装思路 

- 判断的基本参数 

  - 哪些写死 

  - 哪些是传进来 

- 拓展 

  - ⾃定义事件，判断传出哪些参数

  - 插槽扩展 

- 优化 
  - 提⾼他的适应性 
    - vif，velse根据⽗组件传⼊的条件来⽣成对应的模板

- 学习⼀个新技术 

- EChart 
  - ⼤局观，直接看快速教程 
  - 分成⼏部分，在对应部分查找⽂
