---
title: vue2.5核心技术
categories: 前端
tags: [Vue]
---

# Vue核心知识

Vue框架是以数据驱动和组件化开发为核心

## 用vue做出自己的第一个网页

1. **引包**

   - **确认已经下载了node,然后执行命令 npm install vue (如需下载自己要的版本在vue后面加上@版本号)**

   - **页面引入刚下载的包**

     ```
     <script type="text/javascript" src="vue.js"></script>
     ```

2. **留坑 即留一个vue模板插入的地方或者是vue代码对其生效的地方**

3. **实例化 即启动Vue**

   **启动： new Vue({el:目的地,template:模板内容});实例化传入的是一个对象options**

   - **options**

     - **目的地 el 对应上面留坑的坑位，可通过id名，类名，标签名来查找 。方式和jq一样**

     - **内容 template**

     - **数据 data 值为函数形式也可是对象，但是都是用函数，因为用的函数最后也是return一个对象**

       ![image-20211219203752125](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211219203800.png)

       ![image-20211219203835528](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211219203837.png)

```
插值表达式{{ }}

插值表达式内填入data里面的变量即可在页面取到变量值{{ data里的变量 }}
```



## vue的常用指令以及使用场景

1. 什么是指令
   在vue中提供一些对于页面+数据的更为方便的操作，这些操作就叫做指令。
   譬如在HTML页面中这样使用<div v-xxx=''></div>
   在vue中v-xxx就是vue的指令

指令就是以数据去驱动DOM行为的,简化DOM操作

2. 常用的指令有哪些，及怎么使用这些指令
   v-text 不可解析html标签

   ![image-20211219204414939](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211219204416.png)

   ![image-20211219204707402](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211219204708.png)
   ![image-20211219204438951](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211219204440.png)

  ![image-20211219204720924](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211219204722.png)

   v-html 可解析html标签
  ![image-20211219204829941](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211219204832.png)

  ![image-20211219204813217](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211219204814.png)

v-if 做元素的插入（append）和移除（remove）操作

  ![image-20211219205014113](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211219205015.png)
  **按钮不见了**

  ![image-20211219205133774](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211219205135.png)

v-else-if

  ![image-20211219205259761](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211219205301.png)

v-else

v-show display:none 和display：block的切换
  ![image-20211219205518014](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211219205519.png)

  ![image-20211219205534697](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211219205536.png)

  ![image-20211219205749767](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211219205751.png)

  当checkshow为false时，自动添加display:none

v-for

数组 item，index
![image-20211219212438820](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211219212440.png)

![image-20211219212402374](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211219212403.png)

![image-20211219212530827](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211219212532.png)

![image-20211219212501207](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211219212502.png)

对象 value，key ，index
![image-20211219212800634](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211219212803.png)

![image-20211219212818008](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211219212819.png)

## vue单双向数据流及事件绑定

1. vue单向数据流绑定属性值 v-bind: (属性) 简写 :(属性)
   例子：<input v-bind:value="name" v-bind:class="name">
   单向数据绑定 内存改变影响页面改变
   
   ![image-20211219220952400](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211219220954.png)
   
   ![image-20211219221129831](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220093141.png)
   
2. v-bind就是对属性的简单赋值,当内存中值改变，还是会触发重新渲染
   vue双向数据流 v-model 只作用于有value属性的元素
   例子：<input v-model="name" v-bind:class="name">
   双向数据绑定 页面对于input的value改变，能影响内存中name变量
   内存js改变name的值，会影响页面重新渲染最新值
   
   ![image-20211219221413895](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220093138.png)
   
   ![image-20211219221431151](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220093135.png)
   
   ![image-20211219221548161](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220093133.png)
   
   3. 事件绑定v-on:事件名="表达式||函数名" 简写 @事件名="表达式||函数名"
      事件名可以是原生也可以是自定义的
   
   ![image-20211219221711063](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220093130.png)
   
   ![image-20211219221835716](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220093128.png)
   
4. 总结
   v-model 双向数据绑定
   vue页面改变影响内存（js）
   内存（js）改变影响vue页面
   v-bind 单向数据绑定只是内存（js）改变影响vue页面

## 过滤器

如何给数据添加一个管道进行进一步处理再输出

- 过滤器就是可以对我们的数据进行添油加醋然后再显示

- 过滤器有全局过滤器和组件内的过滤器he
  
  ```
  全局过滤器Vue.filter('过滤器名',过滤方式fn );
  组件内的过滤器 filters:{ '过滤器名',过滤方式fn }
  {{ msg | 过滤器名}}heo
  ```
  
- 最终都是在过滤方式fn里面return产出最终你需要的数据

- vue中的this是vue封装好给我们使用的，跟平常方法里面的this是不同的

![image-20211219222224531](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220093121.png)

![image-20211219222245592](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220093124.png)

跟上面的等价

![image-20211219222354205](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220093117.png)

![image-20211219222718379](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220093114.png)

![image-20211219222918776](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220093111.png)

![image-20211219222822620](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220093107.png)

## 数据监听watch计算属性computed
数据的单个监听以及多个监听还有深度监听的不同

- watch监听单个，computed监听多个


思考业务场景：

类似淘宝，当我输入某个人名字时，我想触发某个效果
利用vue做一个简单的计算器
当watch监听的是复杂数据类型的时候需要做深度监听（写法如下）

```js
watch:{
            msg:{
              handler(val){
               if(val.text=='love'){
                alert(val.text)
               }
              },
              deep:true//开启深度监听
            }
          }
```

- computed 监视对象,写在了函数内部, 凡是函数内部有this.相关属性,改变都会触发当前函数

![image-20211219223316089](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220093101.png)

![image-20211219223241741](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220093058.png)

![image-20211219223407513](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220093055.png)

![image-20211219223348157](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220093051.png)

![image-20211219223518183](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220093048.png)

现在输入love没有弹窗说明，watch无法监听复杂数据类型

![image-20211220092918867](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220093044.png)

![image-20211220092934009](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220093040.png)

# 组件化开发

## 组件化开发

页面拆分为组件进行开发和维护

**创建组件的两种方式**

```js
var Header = { template:'模板' , data是一个函数,methods:功能,components:子组件们 }//局部声明
Vue.component('组件名',组件对象);//全局注册 等于注册加声明了
```

**组件类型**

- 通用组件（例如表单、弹窗、布局类等)
- 业务组件（抽奖、机器分类）
- 页面组件（单页面开发程序的每个页面的都是一个组件、只完成功能、不复用）

- 组件开发三步曲：声明、注册、使用

  ![image-20211220094044721](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220094046.png)

  ![image-20211220094120046](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220094122.png)

  上面第一页本来是这样写的，语法糖来着

  ![image-20211220094152386](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220094157.png)

![image-20211220094236130](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220094237.png)

全局创建一个尾部

![image-20211220094407041](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220094408.png)

## slot插槽和ref、$parent

设计可扩展组件及获取子组件和父组件实例

- slot插槽

slot就是子组件里给DOM留下的坑位
<子组件>DOM</子组件>
slot是动态的DOM

- ref获取子组件实例

识别：在子组件或元素上使用属性ref="xxxx"
获取：this.\$refs.xxxx 获取元素
\$el 是拿其DOM

 ```
 $parent获取父组件实例（可在子组件直接使用this.$parent即可）
 ```

![image-20211220095846805](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220095848.png)

![image-20211220100016718](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220100018.png)

![image-20211220100037782](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220100038.png)

![image-20211220100205308](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220100206.png)

![image-20211220100233572](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220100234.png)

![image-20211220100221840](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220100223.png)

![image-20211220100334074](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220100335.png)

![image-20211220100357320](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220100358.png)

![image-20211220100852624](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220100853.png)

![image-20211220101053735](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220101055.png)

![image-20211220100813591](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220100815.png)

下面这两个都是生命周期钩子函数

![image-20211220101337290](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220101338.png)

![image-20211220101258850](C:/Users/86158/AppData/Roaming/Typora/typora-user-images/image-20211220101258850.png)

获取父组件

![image-20211220101428982](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220101430.png)

![image-20211220101501422](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220101502.png)

![image-20211220101535302](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220101555.png)

![image-20211220101621811](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220101622.png)

## 父子组件的通信

**父传子**

- 父传子的时候通过属性传递
- 子要声明props:['属性名'] 来接收
- 收到就是自己的了，随便你用
  - 在template中 直接用
  - 在js中 this.属性名 用

**子传父**

- 子组件里通过$emit('自定义事件名',变量1，变量2)触发

- 父组件@自定义事件名=‘事件名’监听

  ```
  子组件方法里  this.$emit('sendfather',val1,val2)触发自定义事件
  父组件里  <child @sendfather='mymethods'></child>
  ```


**父传子**

![image-20211220102235432](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220102236.png)

![image-20211220102349788](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220102351.png)

**子传父**

![image-20211220102635225](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220102636.png)

![image-20211220103008383](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220103016.png)

![image-20211220102902736](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220102903.png)

按钮一点出现了

![image-20211220102909377](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211221103355.png)

## 非父子组件之间的通信

创建一个空实例（bus中央事件总线也可以叫中间组件）

利用`$emit $on`的触发和监听事件实现非父子组件的通信

```
Vue.prototype.$bus=new Vue()//在vue上面挂载一个$bus作为中央处理组件
this.$bus.$emit('自定义事件名','传递的数据')//触发自定义事件传递数据
this.$bus.$on('自定义事件名'，fn)//监听自定义事件获取数据
```

解决的方案还有vuex、provide/inject是解决同根往下派发、本地存储也可以进行非父子组件之间的通信

例子：底部传给头部信息

声明：

![image-20211220103311708](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220103313.png)

注册，使用：

![image-20211220103401664](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220103403.png)

![image-20211220103626157](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220103627.png)

![image-20211220103731134](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220103732.png)

![image-20211220104006124](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220104007.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id="app"></div>
    <script type="text/javascript" src="vue.js">

    </script>
    <script type="text/javascript">
        Vue.prototype.$bus = new Vue()
        var MyHeader={
            template:
            `<div>
            我是头部{{headermsg}}
            </div>`,
            data(){
                return{
                    headermsg:'我是头部的信息'
                }
            },
            created(){
                var self = this
                //this是MyHeader这个实例
                this.$bus.$on('sending',function(val){
                    //这个this是$bus这个空实例的this，所以不能直接这样写
                    //this.headermsg = val;
                    //self.headermsg = val;
                    this.$bus.$on('sending',val=>{
                        this.headermsg = val;
                    })
                })
            }
        }
        var MyBody={
            template:`<div>我是身体</div>`,

        }
        var MyFooter={
            template:`<div>我是底部<button @click='sendhead'>我要跟头部通信</button></div>`,
            methods:{
                sendhead(){
                    this.$bus.$emit('sending','我是底部的数据')
                }
            }
        }
        new Vue({
            el:'#app',
            template:`
            <div>
                <my-header></my-header><hr>
                <my-body></my-body><hr>
                <my-footer></my-footer>
            </div>
            `,
            components:{
                MyHeader,
                MyBody,
                MyFooter
            },
            data(){
                return{
                
                }
            }
        })
    </script>
</body>
</html>
```

点击按钮之后

![image-20211220110119987](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220110121.png)



## vue的生命周期

vue所有的生命周期钩子函数的作用

- 需要频繁的创建和销毁组件

  比如页面中部分内容显示与隐藏，但是用的是v-if

- 组件缓存

  内置组件中

  被其包裹的组件，在v-if=false的时候，不会销毁，而是停用

  v-if="true" 不会创建,而是激活

  避免频繁创建组件对象的性能损耗

  组件的激活和停用

  - activated 和 deactivated

- 成对比较

  created 和 beforeCreate

  - A 可以操作数据 B 数据没有初始化

  mounted 和 beforeMount

  - A 可以操作DOM B 还未生成DOM

  updated 和 beforeUpdate

  - A 可以获取最终数据 B 可以二次修改

  destroyed 和 beforeDestroy

  - 性能调优：频繁销毁创建的组件使用内置组件包裹

![image-20211220113517613](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220113519.png)

![image-20211220113547092](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220113549.png)

![image-20211220113745212](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220113746.png)

![image-20211220113835777](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220113836.png)



![image-20211220115051702](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220115052.png)

![image-20211220114240622](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220114241.png)

![image-20211220114336155](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220114338.png)

![image-20211220114208929](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220114210.png)

![image-20211220115604155](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220115606.png)

![image-20211220115535748](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220115537.png)

![image-20211220115346828](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220115348.png)

![image-20211220115403734](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220115405.png)

![image-20211220115638439](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220115640.png)

![image-20211220115920428](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220115921.png)

![image-20211220120045037](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220120046.png)



# Vue核心插件之路由模块

## 路由的跳转原理（哈希模式）

单页应用的路由模式有两种

- 哈希模式（利用`hashchange` 事件监听 url的hash 的改变）
- history模式（使用此模式需要后台配合把接口都打到我们打包后的index.html上）

哈希模式原理

```js
window.addEventListener('hashchange', function(e) {
  console.log(e)
})
```

核心是锚点值的改变，我们监听到锚点值改变了就去局部改变页面数据，不做跳转。跟传统开发模式url改变后 立刻发起请求，响应整个页面，渲染整个页面比路由的跳转用户体验更好

![image-20211220123420506](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220123422.png)

![image-20211220123540028](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220123542.png)

##  安装和使用路由

路由是以插件的形式引入到我们的vue项目中来的

vue-router是vue的核心插件
1:下载 npm i vue-router -S
2:安装插件Vue.use(VueRouter);
3:创建路由对象 var router = new VueRouter();
4:配置路由规则 router.addRoutes([路由对象]);
路由对象{path:'锚点值',component:要(填坑)显示的组件}
5:将配置好的路由对象交给Vue
在options中传递-> key叫做 router
6:留坑(使用组件) <router-view></router-view>

**查看源码可以发现vue-router.js返回了VueRouter**

![image-20211220125758689](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220125800.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <!-- 坑位 -->
    <div id="app"></div>
    <script type="text/javascript" src="vue.js"></script>
    <!-- 引入路由插件 -->
    <script type="text/javascript" src="vue-router.js"></script>
    
    <script type="text/javascript">
        var Login={
            template:`<div>我是登录页面</div>`,
        }
        // 安装路由插件
        Vue.use(VueRouter);

        //创建路由对象
        var router = new VueRouter({
            //配置路由
            routes:[
                {path:'/login',name:'login',component:Login}
            ]
        })
        new Vue({
            el:'#app',
            router,
            template:`
                <div>
                    <router-view></router-view>
                </div>`,
            data(){
                return{}
            },
        })
    </script>
</body>
</html>
```



![image-20211220132234753](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220132236.png)

## 路由的跳转

**路由的跳转方式有：**
通过标签：<router-link to='/login'></router-link>
通过js控制跳转this.\$router.push({path:'/login'})

**区别：**
this.\$router.push() 跳转到指定的url，会向history插入新记录
this.\$router.replace() 同样是跳转到指定的url，但是这个方法不会向history里面添加新的记录，点击返回，会跳转到上上一个页面。上一个记录是不存在的。
this.\$router.go(-1) 常用来做返回，读history里面的记录后退一个
vue-router中的对象：
\$route 路由信息对象,只读对象
\$router 路由操作对象,只写对象

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <!-- 坑位 -->
    <div id="app"></div>
    <script type="text/javascript" src="vue.js"></script>
    <!-- 引入路由插件 -->
    <script type="text/javascript" src="vue-router.js"></script>
    
    <script type="text/javascript">
        var Login={
            template:`<div>我是登录页面</div>`,
        }
        var Register={
            template:`
                <div>我是注册页面</div>
            `
        }
        var Buy={
            template:`
                <div>我要买东西</div>
            `
        }
        // 安装路由插件
        Vue.use(VueRouter);

        //创建路由对象
        var router = new VueRouter({
            //配置路由
            routes:[
                {path:'/login',name:'login',component:Login},
                {path:'/register',name:'register',component:Register},
                {path:'/buy',name:'buy',component:Buy}
            ]
        })
        new Vue({
            el:'#app',
            router,
            template:`
                <div>
                    <router-link to='/login'>去登录</router-link>
                    |
                    <router-link to='/register'>去注册</router-link>
                    <div>
                        <button @click='goregister'>我要去注册</button>
                        <button @click='back'>返回上一页</button>
                    </div>
                    <router-view></router-view>
                </div>`,
            data(){
                return{}
            },
            methods:{
                goregister(){
                    //this.$router.push({path:'/register'})
                    this.$router.replace({path:'/register'})
                },
                back(){
                    this.$router.go(-1)
                }
            }
        })
    </script>
</body>
</html>
```

## 路由的传参和取参

**查询参**
配置（传参） :to="{name:'login',query:{id:loginid}}"
获取（取参） this.\$route.query.id
**路由参数**
配置（传参） :to="{name:'register',params:{id:registerid} }"
配置路由的规则 **{ name:'detail',path:'/detail/:id'}**
获取 this.\$route.params.id
**总结：**
:to传参的属性里 params是和name配对的 query和name或path都可以
**使用路由参数必须要配置路由规则里面配置好参数名**，否则刷新页面参数会丢失

![image-20211220145456647](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220145458.png)

![image-20211220145529705](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220145531.png)

![image-20211220145550285](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220145551.png)

![image-20211220145849787](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220145851.png)

![image-20211220145947731](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220145948.png)

![image-20211220150121750](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220150123.png)

![image-20211220150134997](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220150136.png)

获取参数还可以这样写

![image-20211220150601666](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220150603.png)

![image-20211220150701580](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220150703.png)

## 嵌套路由

补充上一节知识点：js跳转路由传参和标签传参，路由相同而参数不同时页面不做刷新的问题
解决方案：<router-view :key="$route.fullPath"></router-view>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <!-- 坑位 -->
    <div id="app"></div>
    <script type="text/javascript" src="vue.js"></script>
    <!-- 引入路由插件 -->
    <script type="text/javascript" src="vue-router.js"></script>
    
    <script type="text/javascript">
        var Login={
            template:`<div>我是登录页面，
                    <span>这是我获得到的参数：{{ msg }}</span>
                        </div>`,
            data(){
                return{
                    msg:''
                }
            },
            created(){
                this.msg = this.$route.query.id
            }
        }
        var Register={
            template:`
                <div>我是注册页面</div>
            `
        }

        // 安装路由插件
        Vue.use(VueRouter);

        //创建路由对象
        var router = new VueRouter({
            //配置路由
            routes:[
                {path:'/login',name:'login',component:Login},
                {path:'/register',name:'register',component:Register},
            ]
        })
        new Vue({
            el:'#app',
            router,
            template:`
                <div>
                    <router-link :to="{name:'login',query:{id:'123'}}">去登录</router-link>
                    |
                    <router-link :to="{name:'register',params:{foo:'bar'}}">去注册</router-link>
                    <button @click='jslink'>js跳转去登录</button>

                    <router-view :key="$route.fullPath"></router-view>
                </div>`,
            data(){
                return{
                    msg:''
                }
            },
            methods:{
                jslink(){
                    this.$router.push({name:'login',query:{id:'456'}})
                }
            }
        
        })
    </script>
</body>
</html>
```

代码思想
1:router-view的细分
router-view第一层中，包含一个router-view
2:每一个坑挖好了，要对应单独的组件
路由配置

```html
routes: [
           {
               path:'/nav',
               name:'nav',
               component:Nav,
               //路由嵌套增加此属性
               children:[
               //在这里配置嵌套的子路由
               ]
           }
       ]
```

案例

进入首页下面会有导航，个人中心、首页、资讯、我的之类的

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <!-- 坑位 -->
    <div id="app"></div>
    <script type="text/javascript" src="vue.js"></script>
    <!-- 引入路由插件 -->
    <script type="text/javascript" src="vue-router.js"></script>
    <script type="text/javascript">
        
        var Nav = {
            template:`
                <div>
                    <router-view></router-view>
                    <router-link :to="{name:'nav.index'}">首页</router-link>
                    |
                    <router-link :to="{name:'nav.personal'}">个人中心</router-link>
                    |
                    <router-link :to="{name:'nav.message'}">资讯</router-link>
                    |
                    <router-link :to="{name:'nav.mine'}">我的</router-link>
                    
                </div>
            `
        }
        var Index={
            template:`
                <div>首页</div>
            `
        }
        var Personal={
            template:`
                <div>个人中心</div>
            `
        }
        var Message={
            template:`
                <div>资讯</div>
            `
        }
        var Mine={
            template:`
                <div>我的</div>
            `
        }
        // 安装路由插件
        Vue.use(VueRouter);

        //创建路由对象
        var router = new VueRouter({
            //配置路由
            routes:[
                {path:'/nav',
                name:'nav',
                component:Nav,
                //嵌套路由增加这个属性
                children:[
                    //配置我们的嵌套路由
                    // {path:'',redirect:'/nav/index'},
                    {path:'index',name:'nav.index',component:Index},
                    {path:'personal',name:'nav.personal',component:Personal},
                    {path:'message',name:'nav.message',component:Message},
                    {path:'mine',name:'nav.mine',component:Mine},
                ]
            
            }
            ]
        })
        new Vue({
            el:'#app',
            router,
            template:`
                <div>
                    <router-view></router-view>
                </div>`,
            data(){
                return{
                    msg:''
                }
            },
            methods:{
                jslink(){
                    this.$router.push({name:'login',query:{id:'456'}})
                }
            }
        
        })
    </script>
</body>
</html>
```

 ## 路由守卫

```
const router = new VueRouter({ ... }
//前置的钩子函数 最后要执行next（）才会跳转
router.beforeEach((to, from, next) => {
  // ...
})
//后置的钩子函数 已经跳转了不需要next
router.afterEach((to, from) => {
  // ...
})
```

主要是简单介绍一下，路由守卫主要用于检验是否登录了，没登录就跳转到登录页面不让他在其他页面停留，但是现在这种处理主要的都用请求的全局拦截来做了。大致了解一下路由守卫即可

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <!-- 坑位 -->
    <div id="app"></div>
    <script type="text/javascript" src="vue.js"></script>
    <!-- 引入路由插件 -->
    <script type="text/javascript" src="vue-router.js"></script>
    <script type="text/javascript">
        
        var Nav = {
            template:`
                <div>
                    <router-view></router-view>
                    <router-link :to="{name:'nav.index'}">首页</router-link>
                    |
                    <router-link :to="{name:'nav.personal'}">个人中心</router-link>
                    |
                    <router-link :to="{name:'nav.message'}">资讯</router-link>
                    |
                    <router-link :to="{name:'nav.mine'}">我的</router-link>
                    
                </div>
            `
        }
        var Index={
            template:`
                <div>首页</div>
            `
        }
        var Personal={
            template:`
                <div>个人中心</div>
            `
        }
        var Message={
            template:`
                <div>资讯</div>
            `
        }
        var Mine={
            template:`
                <div>我的</div>
            `
        }
        // 安装路由插件
        Vue.use(VueRouter);

        //创建路由对象
        var router = new VueRouter({
            //配置路由
            routes:[
                {path:'/nav',
                name:'nav',
                component:Nav,
                //嵌套路由增加这个属性
                children:[
                    //配置我们的嵌套路由
                    // {path:'',redirect:'/nav/index'},
                    {path:'index',name:'nav.index',component:Index},
                    {path:'personal',name:'nav.personal',component:Personal},
                    {path:'message',name:'nav.message',component:Message},
                    {path:'mine',name:'nav.mine',component:Mine},
                ]
            
            }
            ]
        })
        new Vue({
            el:'#app',
            router,
            template:`
                <div>
                    <router-view></router-view>
                </div>`,
            data(){
                return{
                    msg:''
                }
            },
            methods:{

            },
            mounted(){
                router.beforeEach((to,from,next)=>{
                    console.log(to);
                    if(to.path=='/nav/index'){
                        next()
                    }else{
                        setTimeout(function(){
                            next()
                        },2000)
                    }
                })
            }
        })
    </script>
</body>
</html>
```

# 购物车实战

![image-20211220203634994](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220203636.png)

**添加商品到商品库**

![image-20211220212033010](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220212034.png)

![image-20211220212209077](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220212210.png)

![image-20211220212419453](C:/Users/86158/AppData/Roaming/Typora/typora-user-images/image-20211220212419453.png)

![image-20211220212556005](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220212557.png)

**添加商品到购物车**

![image-20211220212757665](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220212759.png)

![image-20211220212915186](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220212916.png)

![image-20211220220617423](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220220618.png)

![image-20211220220837484](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220220838.png)

![image-20211220221024125](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220221025.png)

![image-20211220221118279](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220221119.png)

![image-20211220221256933](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220221258.png)

![image-20211220221544883](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220221546.png)

![image-20211220222335386](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220222337.png)

![image-20211220222431697](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220222433.png)

![image-20211220222510302](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211221103244.png)

冒号是v-bind的简写，可以随时监听

![image-20211220222949023](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220222950.png)

![image-20211220223039683](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220223040.png)

增加这一行

![image-20211220223934750](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220223954.png)

监听

![image-20211220224355264](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220224356.png)

![image-20211220224719018](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220224721.png)

![image-20211220224852398](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220224853.png)

刷新数据还在，就不会消失

![image-20211220224912354](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211220224913.png)
