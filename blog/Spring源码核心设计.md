---

title: Spring源码核心设计
---

# Bean的生命周期和IOC

## BeanFactory和BeanFactoryPostProcessor

![image-20220103150213437](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103150215.png)

访问Spring bean容器的根接口

![image-20220103150840336](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103150841.png)

![image-20220103151051256](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103151052.png)

![image-20220103151157688](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103151159.png)

beanFactory就是入口，容器对象，applicationContext就是beanFactory更加完善的继承接口罢了

defaulListableBeanFactory待会提到

![image-20220103151311416](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103151312.png)

能不能通过beanFactory对象找到BeanDefinition

![image-20220103151608080](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103151609.png)

![image-20220103151629256](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103151631.png)

![image-20220103151641980](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103151643.png)

![image-20220103151708713](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103151709.png)

![image-20220103152449126](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103152450.png)

BeanFactoryPostProcessor的实现子类

![image-20220103152623566](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103152625.png)

![image-20220103152730249](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103152731.png)

![image-20220103152825859](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103152827.png)

![image-20220103154649659](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103154651.png)

refresh() 有13个方法

![image-20220103154955494](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103154956.png)

![image-20220103155111149](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103155112.png)

![image-20220103160320162](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103160321.png)

![image-20220103162619904](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103162621.png)

![image-20220103162723069](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103162724.png)

![image-20220103162937834](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103162939.png)

![image-20220103163042586](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103163043.png)

![image-20220103163311358](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103163312.png)

![image-20220103164153130](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103164154.png)

![image-20220103164358730](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103164400.png)

![image-20220103165736508](C:/Users/86158/AppData/Roaming/Typora/typora-user-images/image-20220103165736508.png)

![image-20220103165813649](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103165814.png)

![image-20220103165927371](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103165928.png)

![image-20220103170138944](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103170140.png)

![image-20220103170252250](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103170253.png)

## springboot自动装配相关

![image-20220103170400077](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103170401.png)

![image-20220103171407039](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103171408.png)

![image-20220103171458003](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103171459.png)

![image-20220103171600693](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103171602.png)

![image-20220103171711798](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103171713.png)

![image-20220103171808512](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103171809.png)

![image-20220103171937686](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103171939.png)

![image-20220103172016776](C:/Users/86158/AppData/Roaming/Typora/typora-user-images/image-20220103172016776.png)

![image-20220103172242724](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103172243.png)

![image-20220103172407976](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103172409.png)

![image-20220103172601114](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103172602.png)

## aware

一堆给实现aware的bean设置一些东西，如beanName 

![image-20220104180541124](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104180544.png)

![image-20220104112631861](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104112633.png)

![image-20220104110220444](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104110221.png)

![image-20220104110137361](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104110138.png)

![image-20220103173154396](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103173155.png)

![image-20220103173301280](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103173302.png)

![image-20220103173344716](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103173346.png)

![image-20220103173529903](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103173531.png)

![image-20220104112501205](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104112503.png)

![image-20220104113851167](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104113853.png)

![image-20220104114048644](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104114050.png)

进入prepareBeanFactory可以看到

![image-20220104114159848](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104114201.png)

继续从invokeAwareMethods往下执行可以看到，执行完applyBeanPostProcessorBeforeInitialization这个enviroment有值了

![image-20220104114502117](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104114503.png)

![image-20220103173817031](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103173818.png)

![image-20220104181219258](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104181220.png)



## aop是IOC的一个扩展实现

BeanPostProcessor after中可以看到aop

![image-20220103174032270](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103174033.png)

![image-20220103174242700](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103174244.png)

![image-20220103174417583](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103174419.png)



![image-20220103175157322](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103175158.png)

![image-20220103175248128](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103175249.png)

![image-20220103175344587](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103175346.png)

![image-20220103175454657](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103175456.png)

![image-20220103175530809](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103175532.png)

继续点进去

![image-20220103175651763](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103175653.png)

![image-20220103175722567](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103175723.png)

继续点进去结果是个接口的方法

![image-20220103175839762](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103175841.png)

点击旁边的向下箭头可以看到具体的两个实现类

![image-20220103175936421](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103175937.png)

![image-20220103180119336](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103180120.png)

## invokeInitMethods

invokeInitMethods检测你当下的bean是否实现了initializingBean接口

![image-20220103180503344](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103180507.png)

![image-20220103180732304](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103180733.png)

![image-20220103180807655](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103180808.png)

![image-20220103180936514](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103180937.png)

![image-20220103181325981](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103181327.png)

![image-20220103181357906](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103181359.png)

beanPostProcessor的实现子类

![image-20220103181545111](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103181546.png)

![image-20220103181723574](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103181724.png)

![image-20220103181843175](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103181844.png)

## BeanFactory和FactoryBean的接口对比与实现

![image-20220103182348146](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103182350.png)

![image-20220103190851340](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103190852.png)

![image-20220103191024340](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103191025.png)

![image-20220103191311789](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103191313.png)

![image-20220103191506485](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103191507.png)

在其他地方搬了几张图过来对比

![image-20220103193347578](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103193349.png)

![image-20220103192743043](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103192744.png)

# 循环依赖问题中的三级缓存和提前暴露对象原理

![image-20220103195843892](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103195846.png)

![image-20220103200605522](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103200607.png)

![image-20220103200719658](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103200720.png)

![image-20220103202032914](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103202034.png)

![image-20220103202123049](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103202124.png)

![image-20220103202304323](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103202305.png)

![image-20220103202536673](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103202540.png)

![image-20220103202709657](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103202711.png)

![image-20220103203004408](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103203006.png)

![image-20220103203118188](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103203119.png)

![image-20220103203455116](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103203456.png)

三级缓存解决了这个问题

![image-20220103203756679](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103203758.png)

![image-20220103203954127](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103203955.png)

![image-20220103204118945](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103204120.png)

![image-20220103204402752](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103204403.png)

![image-20220103204525386](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103204526.png)

![image-20220103204654611](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103204655.png)

![image-20220103204752994](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103204754.png)

![image-20220103204851595](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103204852.png)

![image-20220103205052351](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103205053.png)

![image-20220103205343740](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103205344.png)

往下走可以看到这里开始调用singletonFactory.getObject方法，就是lambda表达式，也就是createBean方法

![image-20220103205419217](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103205420.png)

进入createBean方法查看

![image-20220103210154884](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103210156.png)

进入doCreateBean方法查看

![image-20220103210345812](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103210348.png)

![image-20220103210537952](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103210539.png)

进入createBeanInstance查看

![image-20220103210654420](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103210655.png)

进入instantiateBean方法

![image-20220103210757372](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103210758.png)

进入instantiate方法

![image-20220103211001904](C:/Users/86158/AppData/Roaming/Typora/typora-user-images/image-20220103211001904.png)

进入instantiateClass方法查看

![image-20220103211135502](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103211136.png)

![image-20220103211309647](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103211310.png)

A有了，但是属性b没有值

![image-20220103211333696](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103211334.png)

回到doCreateBean方法中

![image-20220103211633576](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103211634.png)

![image-20220103211920753](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103211922.png)

![image-20220103212049063](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103212050.png)

![image-20220103212208621](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103212209.png)

A还是半成品，需要给b属性赋值了

![image-20220103212407658](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103212412.png)

进入populateBean方法

![image-20220103212506684](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103212507.png)

进入applyPropertyValues方法

![image-20220103212912272](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103212913.png)

这个b的值是运行时bean应用

![image-20220103213028503](C:/Users/86158/AppData/Roaming/Typora/typora-user-images/image-20220103213028503.png)

往下走

![image-20220103213145274](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103213146.png)

进入这个resolveValueIfNecessary方法

![image-20220103213408327](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103213409.png)

进入resolveReference方法

![image-20220103213609631](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103213611.png)

![image-20220103213738164](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103213739.png)

进入doGetBean方法

![image-20220103214032138](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103214033.png)

![image-20220103214125362](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103214126.png)

![image-20220103214311452](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103214312.png)

![image-20220103214332415](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103214451.png)

往三级缓存里头放东西

![image-20220103214438512](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103214454.png)

![image-20220103214604016](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103214605.png)

applyPropertyValue方法 给b的属性a赋值啦 

![image-20220103214653485](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103214654.png)

![image-20220103214924564](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103214925.png)

![image-20220103215112922](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103215114.png)

进入看看逻辑

![image-20220103215345305](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103215346.png)

![image-20220103215514265](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103215515.png)

但是不是对象，是lambda表达式

![image-20220103215639218](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103215641.png)

进入getEarlyBeanReference方法内部

![image-20220103223828934](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103223830.png)

![image-20220103211333696](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103211334.png)

![image-20220103214332415](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103214451.png)

![image-20220103224329638](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103224330.png)

![image-20220103224408159](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103224409.png)

![image-20220103224540469](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103224541.png)

![image-20220103224638762](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103224640.png)

![image-20220103224703022](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103224704.png)

![image-20220103225051987](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103225053.png)

往回走，已经给b的属性a对象取到了，给b设置上a

![image-20220103225213820](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103225215.png)

![image-20220103225310615](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103225311.png)

![image-20220103225417482](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103225418.png)

![image-20220103225540242](C:/Users/86158/AppData/Roaming/Typora/typora-user-images/image-20220103225540242.png)

![image-20220103225718682](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103225719.png)

![image-20220103225801534](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103225802.png)

移除三级缓存

![image-20220103225824341](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103225825.png)

![image-20220103230018653](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103230020.png)

![image-20220103230145266](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103230146.png)

![image-20220103230215356](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103230216.png)

![image-20220103230412071](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103230413.png)

![image-20220103230529026](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103230530.png)

![image-20220103230740015](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103230741.png)

![image-20220103230850600](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103230851.png)

![image-20220103231709907](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220103231711.png)

![image-20220104000542778](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104000546.png)

![image-20220104001622855](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104001624.png)

![image-20220104000952375](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104000953.png)

![image-20220104001043512](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104001050.png)

![image-20220104001208311](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104001209.png)

![image-20220104001308724](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104001309.png)

![image-20220104001350060](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104001351.png)

![image-20220104001429474](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104001430.png)

![image-20220104001553401](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104001554.png)

这就是为什么三级缓存存在的意义

![image-20220104001911073](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104001912.png)

# 环境对象和监听器

![image-20220104111045241](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104111046.png)

# 总结

**ioc容器**

![image-20220104120942219](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104120944.png)

![image-20220104121018882](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104121021.png)

**bean生命周期**

![image-20220104121338054](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104121339.png)

![image-20220104122020600](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104122022.png)

**循环依赖**

![image-20220104122610462](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104122611.png)

![image-20220104123142563](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104123143.png)

**缓存放置时间和删除时间**

![image-20220104131900239](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104131901.png)

**BeanFactory和FactoryBean有什么区别**

![image-20220104133020456](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104133021.png)

**spring用到的设计模式**

![image-20220104133432521](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104133441.png)

**aop底层实现**

![image-20220104134426984](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104134428.png)

**spring的事务是如何回滚的**

![image-20220104145930665](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104145931.png)

**事务的传播**

![image-20220104151221547](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104151222.png)

![image-20220104151242420](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104151244.png)

# ConfigurationClassPostProcessor解析

![image-20220104162523044](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104162524.png)

进入这个BeanDefinitionRegistryPostProcessor，里面有个方法

![image-20220104162639625](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104162642.png)

![image-20220104163213277](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104163214.png)

![image-20220104163351776](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104163353.png)

![image-20220104163307695](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104163309.png)

![image-20220104163613869](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104163615.png)

从上面蓝色处的invokeBeanFactoryPostProcessor方法进去

# 源码一

![image-20220104171443958](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104171445.png)

![image-20220104171928734](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104171930.png)

不同格式的解析类

![image-20220104171945495](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104171947.png)

![image-20220104175245955](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104175247.png)

![image-20220104175623857](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104175630.png)

![image-20220104181653308](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104181654.png)

![image-20220104181750588](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104181752.png)

![image-20220104182959173](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104183000.png)

# 源码二

## prepareRefresh

![image-20220104193741224](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104193743.png)

![image-20220104192603054](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104192604.png)

进到refresh方法，这里有个关键点 getEnvironment

![image-20220104192709773](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104192711.png)

![image-20220104192857484](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104192905.png)

![image-20220104193003244](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104193004.png)

![image-20220104193040573](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104193042.png)

![image-20220104193201300](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104193202.png)

![image-20220104194151225](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104194152.png)

springMVC有个父子容器的概念，每次我们进行bean查询的时候，第一步会先去当前容器里查找，接着去父容器查找

![image-20220104201108955](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220104201110.png)

![image-20220104211614006](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105103553.png)

## **ListaleBeanFactory**

![image-20220105101944402](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105103549.png)

## ConfigurableBeanFactory

![image-20220105102444286](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105103547.png)  

## DefaultListableBeanFactor以及AbstractApplicationContext

这个是AbstractApplicationContext类里头的refresh方法

![image-20220105103845880](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105103847.png)

进入obtainFreshBeanFactory方法

![image-20220105103639524](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105103641.png)

进入refreshBeanFactory方法

![image-20220105103501573](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105103509.png)

![image-20220105103539195](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105103540.png)

refreshBeanFactory方法内部往下看是loadBeanDefinitions方法

![image-20220105104034198](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105104035.png)

进去之后可以看到很多实现类

![image-20220105104126055](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105104127.png)

![image-20220105104205195](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105104206.png)

![image-20220105104313857](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105104315.png)

![image-20220105104347334](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105104348.png)

![image-20220105104416997](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105104418.png)

![image-20220105104554540](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105104556.png)

![image-20220105104619917](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105104621.png)

![image-20220105104852569](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105104853.png)

![image-20220105104923203](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105104924.png)

![image-20220105105034587](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105105036.png)

![image-20220105105121302](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105105122.png)

![image-20220105110013535](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105110015.png)

**来到这个prepareFactory**

![image-20220105110451432](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105110452.png)

![image-20220105110541188](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105110542.png)

![image-20220105110708833](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105110710.png)

进到setBeanExpressionResolve

![image-20220105111208886](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105111210.png)

![image-20220105110749215](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105110750.png)

![image-20220105111343170](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105111344.png)

![image-20220105111720258](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105111721.png)

![image-20220105111835598](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105111837.png)

来到这个postProcessBeanFactory方法内部了

![image-20220105112105046](C:/Users/86158/AppData/Roaming/Typora/typora-user-images/image-20220105112105046.png)

![image-20220105112211447](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105112212.png)

![image-20220105112723483](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105112724.png)

![image-20220105112758872](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105112800.png)

创建好BeanFactory和BeanDefinition后开始执行BeanFactoryPostProcessor部分了 BFPP也是bean，要在单例实例化之前调用

![image-20220105115256996](C:/Users/86158/AppData/Roaming/Typora/typora-user-images/image-20220105115256996.png)

![image-20220105115559479](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105115600.png)

![image-20220105115800594](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105115801.png)

![image-20220105115844164](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105115845.png)

注册BPP

![image-20220105120855235](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105120856.png)

实例化和注册，没有执行BPP

![image-20220105120929826](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105120931.png)

![image-20220105121054316](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105121056.png)

![image-20220105121255669](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105121256.png)

点进去

![image-20220105121423979](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105121425.png)

涉及一些类型转换的操作

![image-20220105121537988](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105121539.png)

![image-20220105121941859](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105121943.png)

![image-20220105123009209](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105123010.png)

![image-20220105123255210](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105123256.png)

![image-20220105123342563](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105123343.png)

**获取bean描述信息**

![image-20220105131512688](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105131513.png)

将父类的和子类合并在一起

![image-20220105131956803](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105131958.png)

![image-20220105132151906](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105132152.png)

![image-20220105132330709](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105132332.png)

如果不是单例，循环依赖问题直接抛异常

![image-20220105133221378](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105133222.png)

![image-20220105133356346](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105133357.png)

![image-20220105133438966](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105133440.png)

![image-20220105133620742](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105133622.png)

![image-20220105133748277](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105133749.png)

![image-20220105133911284](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105133912.png)

![image-20220105135549796](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105135551.png)

![image-20220105135700011](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105135701.png)

![image-20220105135755571](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105135756.png)

无参的构造方法

![image-20220105135843045](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105135850.png)

![image-20220105140004378](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105140007.png)

![image-20220105140029472](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105140030.png)

![image-20220105140337039](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105140338.png)

![image-20220105140416109](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105140417.png)

![image-20220105140456677](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105140457.png)

完成实例化了

现在要开始初始化了，提前暴露，暴露已经实例化，但是还没初始化的值

![image-20220105144808096](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105144809.png)

![image-20220105145013884](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105145014.png)

属性填充

![image-20220105145645550](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105145646.png)

![image-20220105145816350](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105145817.png)

![image-20220105145853858](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105145855.png)

![image-20220105145919393](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105145920.png)

![image-20220105150008628](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105150009.png)

注册销毁bean的函数 钩子函数

![image-20220105150140702](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105150141.png)

![image-20220105150347573](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105150348.png)

![image-20220105150317344](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105150318.png)

![image-20220105150847336](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105150848.png)

进去看看

![image-20220105153450154](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105153451.png)

# 源码三

![image-20220105154537221](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105154538.png)

![image-20220105154628573](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105154629.png)

![image-20220105154658751](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105154659.png)

![image-20220105154714034](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105154715.png)

![image-20220105154733829](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105154735.png)

![image-20220105155031739](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105155032.png)

refresh和destroy的同步监视器

![image-20220105155246636](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105155247.png)

![image-20220105162221267](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105162222.png)

![image-20220105162418488](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105162420.png)

![image-20220105162434711](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105162436.png)

![image-20220105163052373](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105163053.png)

![image-20220105163200509](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105163201.png)

往下执行

![image-20220105163458826](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105163500.png)

![image-20220105163516911](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105163518.png)

![image-20220105163943909](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105163945.png)

![image-20220105164134785](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105164136.png)

![image-20220105164246975](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105164248.png)

![image-20220105164407254](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105164408.png)

![image-20220105164731045](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105164732.png)

![image-20220105164824638](C:/Users/86158/AppData/Roaming/Typora/typora-user-images/image-20220105164824638.png)

记住这个类AbstactRefreshableConfigApplicationContext

![image-20220105165023507](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105165024.png)

跳到自己扩展的这个类上去了

![image-20220105165928932](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105165930.png)

![image-20220105170104124](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105170106.png)

![image-20220105171700527](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105171706.png)

![image-20220105170125950](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105170127.png)

![image-20220105170311122](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105170313.png)

![image-20220105170439318](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105170440.png)

springboot中的有值，spring没有值

![image-20220105171936822](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105171938.png)

![image-20220105172042257](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105172043.png)

![image-20220105172350147](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105172351.png)

![image-20220105172515551](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105172516.png)

![image-20220105173622579](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105173623.png)

![image-20220105175223975](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105175225.png)

![image-20220105174500776](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105174502.png)

![image-20220105175356435](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105175357.png)

![image-20220105185936657](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105185938.png)

![image-20220105190042986](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105190044.png)

点super 适配器模式

![image-20220105190202976](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105190212.png)

![image-20220105190730333](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105190748.png)

![image-20220105190940784](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105190942.png)

本来是可以从互联网取的，这里设置了entityResolver就是读取本地的xsd文件的

![image-20220105191132353](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105191134.png)

![image-20220105191314892](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105191315.png)

![image-20220105191417857](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105191419.png)

![image-20220105191458608](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105191500.png)

![image-20220105191627481](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105191629.png)

PluggableSchemaResolver是为了解决这个

![image-20220105194514487](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105194515.png)

![image-20220105195121308](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105195122.png)

![image-20220105195245631](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105195247.png)

![image-20220105200755547](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105200757.png)

![image-20220105201157386](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105201158.png)

![image-20220105201308806](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105201310.png)

![image-20220105202425592](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105202427.png)

![image-20220105203138356](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105203139.png)

# 源码四

![image-20220105205658435](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105205659.png)

![image-20220105205737833](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105205739.png)

![image-20220105205932817](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105205934.png)

![image-20220105210004239](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105210006.png)

![image-20220105214809761](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105214811.png)

![image-20220105215038070](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105215046.png)

![image-20220105215541245](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105215543.png)

![image-20220105215659060](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105215700.png)

![image-20220105215944541](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105215945.png)

![image-20220105220026520](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105220027.png)

![image-20220105221328716](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105221331.png)

![image-20220105221614334](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105221615.png)

parse实现

![image-20220105221703290](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105221704.png)

![image-20220105221914337](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105221915.png)

![image-20220105222124617](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105222126.png)

![image-20220105222515575](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105222516.png)

![image-20220105222726482](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105222728.png)

![image-20220105222928311](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105222929.png)

![image-20220105223005674](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220105223007.png)

![image-20220106112421810](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106112424.png)

![image-20220106112504443](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106112505.png)

![image-20220106112632518](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106112633.png)

![image-20220106112724119](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106112725.png)

![image-20220106112753530](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106112755.png)

![image-20220106113033235](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106113035.png)

![image-20220106113234556](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106113236.png)

![image-20220106113336019](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106113337.png)

![image-20220106113432125](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106113433.png)

![image-20220106113543914](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106113545.png)

![image-20220106113941673](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106122231.png)

![image-20220106122226930](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106122228.png)

![image-20220106122413117](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106122414.png)

![image-20220106123144997](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106123146.png)

![image-20220106131230539](C:/Users/86158/AppData/Roaming/Typora/typora-user-images/image-20220106131230539.png)

![image-20220106131511543](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106131512.png)

![image-20220106131621326](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106131630.png)

![image-20220106132059358](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106132100.png)

![image-20220106132536523](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106132537.png)

# 源码五

![image-20220106135111878](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106135118.png)

![image-20220106135924752](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106135925.png)

![image-20220106140015850](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106140017.png)

![image-20220106140146385](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106140147.png)

![image-20220106140225713](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106140226.png)

![image-20220106140639457](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106140640.png)

![image-20220106141717014](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106141718.png)

![image-20220106141739224](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106141740.png)

![image-20220106141807571](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106141808.png)

![image-20220106141819346](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106141820.png)

![image-20220106141859071](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106141900.png)

![image-20220106141957895](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106141959.png)

![image-20220106142748156](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106142749.png)

这些属性怎么来的？

![image-20220106142918173](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106142919.png)

![image-20220106143027293](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106143028.png)

![image-20220106143342698](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106143344.png)

![image-20220106143926370](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106143927.png)

![image-20220106150121281](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106150124.png)

![image-20220106144735577](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106144736.png)

![image-20220106150249805](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106150251.png)

![image-20220106150409427](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106150411.png)

## 解析自定义标签

![image-20220106151221971](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106151223.png)

![image-20220106151305011](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106151306.png)

![image-20220106151545746](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106151547.png)

![image-20220106151858735](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106151900.png)

![image-20220106152100989](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106152102.png)

![image-20220106152123996](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106152125.png)

![image-20220106155248208](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106155249.png)

![image-20220106155414323](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106155415.png)

![image-20220106160637267](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106160638.png)

![image-20220106160226183](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106160227.png)

![image-20220106160808978](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106160810.png)

![image-20220106160902843](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106160904.png)

记得这里是大写

![image-20220106161320950](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106161322.png)

![image-20220106161603102](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106161604.png)

![image-20220106162502217](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106162503.png)

![image-20220106162540474](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220106162542.png)

# 源码六

![image-20220107211320979](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220107211322.png)

![image-20220107211927908](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220107211929.png)

![image-20220107212728247](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220107212729.png)

![image-20220107212740750](C:/Users/86158/AppData/Roaming/Typora/typora-user-images/image-20220107212740750.png)

![image-20220107212824701](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220107212825.png)

![image-20220107213450700](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20220107213451.png)

12a 3aa1a

132a2a1a
