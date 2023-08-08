---
title: Javascript基础到实战
categories: 前端
tags: [Javascript]
---

# 初识Javascript

![image-20211218092900681](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218092902.png)

等价于

![image-20211218092925232](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218092926.png)

![image-20211218094751169](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218094752.png)

![image-20211218100330968](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218100334.png)

![image-20211218100418752](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218100420.png)

![image-20211218101024779](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218101026.png)

![image-20211218101243450](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218101244.png)

![image-20211218101412778](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218101413.png)

![image-20211218102629980](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218103530.png)

![image-20211218103603777](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218103605.png)

**构造函数的第一个字母要大写**

![image-20211218105726866](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218105728.png)

# Javascript对象

![image-20211218110122754](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218110124.png)

![image-20211218111112075](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218111113.png)

![image-20211218111841798](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218111843.png)

![image-20211218111916002](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218111917.png)

![image-20211218111942136](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218111943.png)

![image-20211218112055053](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218112056.png)

![image-20211218112209421](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218112211.png)

![image-20211218112355298](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218112356.png)

# Javascript中重要数据结构之数组

![image-20211218130602557](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218130603.png)

![image-20211218130733124](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218130734.png)

![image-20211218130946675](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218130948.png)

**splice**

- ⽤于删除或替换元素
- 函数有返回值，返回的是被删除的元素
- 这个⽅法还会改变原来的数组

```
// 第⼀个参数是控制从第几位（包含）开始删除或者替换(得看第三个参数有没有值)
// 第⼆个参数控制删除的数量
// 第三个参数将删除了的元素替换掉，可⽤逗号隔开
list.splice(2, 2, 'hello','xd')
```

- 使⽤场景
  - 替换数组中的元素
  - 删除数组的⼀部分内容
  - 清空的数组的作⽤

**join**

- 将数组类型的数据转换成字符串
- 和toString的区别 可以⾃定义元素之间⽤自定义符号隔开

```js
var list=[1,2,3]
console.log(list.join('*')) // 1*2*3
```

**concat**

- 用于连接两个或多个数组
- 不会更改现有数组，而是返回一个新数组，其中包含已连接数组的值

```js
array1.concat(array2, array3, ..., arrayX)
```

- 题⽬⼀：将字符串`"张三,李四,小明,小红"`转化成数组，并且删除`"李四"`

  - 提示：split,indexOf

  - ```js
    var str = '张三,李四,小明,小红';
    var list = str.split(',');
    // console.log(list);
    var index = list.indexOf('李四');
    // console.log(index);
    list.splice(1, 1);
    console.log(list);
    ```

- 题⽬⼆：在排好序的数组⾥，按照⼤⼩顺序插⼊数据

```js
// 排好序的数组
var list = [23, 32, 45, 53, 62, 68]
// 插⼊的是44 则插⼊到32和45之间

// 提示 splice
var num = 44;
var index = null;
for (var i = 0; i < list.length; i++) {
  if (list[i] < num) {
    index = i;
  }
}

// console.log(index);
list.splice(2, 0, num);
console.log(list);
```

# 运算符和流程控制

![image-20211218133828996](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218133831.png)

![image-20211218134139894](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218134141.png)

![image-20211218134211664](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218134213.png)

![image-20211218134244635](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218134246.png)

![image-20211218135649781](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218135651.png)

```
//&& 的优先级高所以结果是 true
console.log(true || false && false);   // true
```

![image-20211218140544099](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218140545.png)

![image-20211218140613434](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218140614.png)

# 调试技巧

![image-20211218141219823](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218141221.png)

- switch 内部严格按照===的规则，⼀定要值和类型相等才⾏ 使⽤break语句打断程序

![image-20211218143852624](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218143853.png)

# Javascript函数

- 形参

  - 声明函数的时候定义的参数

    ```js
    function xd(a,b){  // a、b是形参
      // 要执行的代码
    }
    ```

- 实参

  - 调⽤函数的时候传的⼀个参数

    ```
    xd(1,2)  // 1、2就是实参
    ```

- 特点

  - 形参和实参是⼀⼀对应的
  - 数量可以不对应
  - 参数的类型不确定
  - 函数可以设置默认参数
  - 实参可以是字⾯量也可以是变量

```
function xd(参数) {
 // 执⾏语句
}

// 匿名函数表达式
var xd=function(参数){
  // 执⾏语句
}

// 构造函数声明
var xd =new Function([arg1[, arg2[, ...argN]],] functionBody)
```

- 注意
  - 声明函数过程中，函数⾥的语句是不会执⾏

![image-20211218144447188](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218144450.png)

![image-20211218145940388](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218145942.png)

![image-20211218150019550](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218150020.png)

![image-20211218150152691](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218150154.png)

- 变量
  - 全局变量
    - 挂载到window对象上的
  - 局部变量
    - 函数体内部声明的变量
- 作⽤域链
  - 内层函数是可以访问外层函数声明的⼀个变量，反之，则不可以

![image-20211218151921092](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218151923.png)

- arguments
  - 是⽤来取参的
  - 传⼊的实参都能在函数体⾥通过arguments类数组取到
  - 具有数组的⼀些特点
    - 通过索引取参
    - 有⻓度

![image-20211218152153428](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218152154.png)

**⽴即执⾏函数**

![image-20211218154008345](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218154009.png)

⽴即执⾏函数（IIFE）

- 括号的作⽤

  - 帮助我们调⽤函数

- 特点

  - 匿名函数
  - ⾃动执⾏
  - 执⾏完成以后销毁

- 写法

  ```js
  (function() {
  ...
  })();
  
  (function() {
  ...
  }());
  ```

- 可以传参数

- 注意

  - 前⾯的语句必须加分号，否则会解析错误

    ![image-20211218154328317](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218154330.png)

- 题⽬⼀：编写⼀个函数，排列任意元素个数的数字数组，按从⼩到⼤顺序输出

  提示：原生js逻辑或者用数组的sort方法

  ```js
  // 数组sort方法实现
  var list = [81, 132, 24, 51, 1, 2];
  function sorts(array) {
    array.sort(function (a, b) {
      return a - b;
    });
    return array;
  }
  console.log(sorts(list));
  
  // 原生js逻辑去实现
  function sort(array) {
    for (var i = 0; i < array.length - 1; i++) {
      for (var j = 0; j < array.length - 1 - i; j++) {
        if (array[j] > array[j + 1]) {
          var a = array[j];
          array[j] = array[j + 1];
          array[j + 1] = a;
        }
      }
    }
    return array;
  }
  
  console.log(sort([32, 1121, 232, 5, 4]));
  ```

   

- 题⽬⼆：输⼊某个数字，计算数字的阶乘

  - 递归函数实现这道题
  - 6！ = 1 * 2 * 3 * 4 * 5 * 6
  - n! = 1 * 2 * 3 * 4 * … … * n

```js
function test(n) {
  if (n === 1) {
    return n;
  } else {
    return n * test(n - 1);
  }
}
// n * test(n - 1)
// 3 * 2 * 1  

console.log(test(3));
```

# Javascript闭包

```js
// 初始化计数器
var count = 0;

// 递增计数器的函数
function add() {
 count += 1;
}

// 调⽤三次 add()
add();
add();
add();
```

![image-20211218160720056](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218160721.png)

- 闭包
  - 闭包是指有权访问另⼀个函数作⽤域中的变量的函数
- 闭包的副作⽤
  - 产⽣内存泄漏
    - ⽐如说我本来要销毁函数的数据，被强⾏保存下来了，保存在内存当中
- 通过闭包能实现什么
  - 实现外界访问函数体内部的变量

# Javascript的原型和原型链

原型（`prototype`）

![image-20211218161146960](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218161148.png)

**原型是function对象的一个属性，它定义了构造函数制造出的对象的公共祖先，通过该构造函数产生的对象，可以继承该原型的属性和方法，原型也是对象**

```js
Person.prototype.lastName='刘'
function Person(){
  
}

var person1=new Person()

console.log(person1.lastName) // 刘
```

![image-20211218163014448](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218163015.png)

- 原型的作⽤
  - 给我们构造函数实例化出来的对象设置公共的属性或者⽅法使⽤的

![image-20211218163319460](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218163321.png)

- 方法写在哪

  - ⽅法写在原型上

    - 写在构造函数⾥的⽅法和属性会重新克隆⼀次，会导致占⽤内存较⾼

  - 需要配置的属性是写在构造函数上

  - **只有构造函数才能对原型上的属性进⾏改动**

    ![image-20211218163527740](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218163538.png)

**⾯试常考知识点之原型链**

- **只有构造函数才能对原型上的属性进⾏改动**

- 函数才有`prototype`属性,对象有`__proto__ || [[prototype]]`属性
- 原型链
  - js⾥万物皆对象，所以⼀直访问`__proto__`属性就会产⽣⼀条链条
  - 链条的尽头是null
  - 当js引擎查找对象的属性时，会先判断对象本身是否存在该属性
  - 不存在的属性就会沿着原型链往上找

![image-20211218152715923](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218152717.png)

```
function Car() {}
var car = new Car()
```

![image-20211218170601012](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218170611.png)

什么是原型链?

```
原型链解决的主要是继承问题
每个对象拥有一个原型对象，通过 proto 指针指向其原型对象，并从中继承方法和属性，同时原型对象也可能拥有原型，这样一层一层，最终指向
null(Object.proptotype.__proto__指向的是null)。这种关系被称为原型链(prototype chain)，通过原型链一个对象可以拥有定义在其他对象中的属性和方法
```

**类型检测**typeof

用于判断基础数据的类型，无法区分对象与数组

```js
var a = 1;
console.log(typeof a); //number

var b = "1";
console.log(typeof b); //string

var xd;
console.log(typeof xd); //undefined


function fun() {}
console.log(typeof fun); //function

var c = [1, 2, 3];
console.log(typeof c); //object

var d = { name: "小滴课堂" };
console.log(typeof d); //object
```

instanceof

用于判断复杂数据的类型，可以区分对象与数组

`instanceof` 运算符用于检测构造函数的 `prototype` 属性是否出现在某个实例对象的原型链上，可以理解为是否为某个对象的实例

```js
var xd = [];
var xdclass = {};
console.log(xd instanceof Array); //true
console.log(xdclass instanceof Array); //false

var a = [1, 2, 3];
console.log(c instanceof Array); //true

var b = { name: "houdunren.com" };
console.log(d instanceof Object); //true

function Fun() {}
var xd = new Fun();
console.log(xd instanceof Fun); //true
```

# 插件化开发初体验之写⼀个插件

- 需求：写⼀个对两个数字进⾏加法运算计算器
- 为什么要写在函数⾥
  - 因为函数⾥声明的变量或者函数，对外界⽆影响
  - 为了让函数在浏览器加载的时候执⾏，那么还要⽤⽴即函数
- **步骤**
  - **声明一个构造函数**
  - **将公共⽅法写在原型上**
  - **将构造函数挂载到window上**
  - **写⼀个⽴即执⾏函数**

```js
(function () {
  function Sum() {}
  Sum.prototype.add = function (num1, num2) {
    return num1 + num2;
  };
  window.Sum = Sum;
})();

var sum = new Sum();
var a = sum.add(4, 5);
console.log(a);
```

# ajax

**ajax的前置知识—JSON**

- JSON是什么
  - JSON(JavaScript Object Notation)是⼀种轻量级的数据交换格式，它基于JavaScript的⼀个⼦集，易于⼈的编写和阅读，也易于机器解析。 JSON采⽤完全独⽴于语⾔的⽂本格式，但是也使⽤了类似于C语⾔家族的习惯（包括C, C++, C#, Java, JavaScript, Perl, Python等。 这些特性使JSON成为理想的数 据交换语⾔
  - ⼀⾔以蔽之
    - JSON是⽤来做数据交换的⼀种语⾔
- JSON的语法格式
  - 属性名称必须是双引号括起来的字符串
  - 最后⼀个属性后不能有逗号
- JSON的作⽤
  - ⽤于传输数据
- 序列化和反序列化
  - 对象序列化后可以在⽹络上传输，或者保存到硬盘(浏览器)上。

```js
var obj = {
  name: '张三',
  age: '18',
};

var a = JSON.stringify(obj); // 序列化、转换成JSON格式
var b = JSON.parse(a); // 反序列化 、转换成对象格式
```

 ![image-20211218193644822](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218193646.png)

- 什么是ajax

  - 以前
    - 前后端不分离，后端返回整个html
    - 每次更新⼀些数据，他都会整个⽹⻚刷新
  - 现在
    - ajax帮助我们向服务器发异步请求

- 同步/异步

  两个术语与我们生活中的意思是相反的

  - 同步

    煮着开⽔，在旁边盯着，等到⽔开了，你才做下件事

  - 异步

    煮着开⽔，同时你继续做了其他事

    ![image-20211218194254346](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218194255.png)

- 原理

  - 通过XmlHttpRequest对象向服务器发异步请求，从服务器获取数据
  - 然后通过js来操作DOM⽽更新⻚⾯
  - 它是在 IE5 中⾸先引⼊的，是⼀种⽀持异步请求的技术
  - 简单的说，也就是 javascript 可以及时向服务器提出请求和处理响应，⽽不阻塞程序运行，达到⽆刷新的效果

- 注意

  - JavaScript是单线程的，会阻塞代码运⾏，所以引⼊XmlHttpRequest请求处理异步数据

 **ajax请求**

创建ajax对象

```js
var xhr
if (window.XMLHttpRequest){
 xhr = new XMLHttpRequest();
} else {// code for IE6, IE5 兼容
 xhr = new ActiveXObject("Microsoft.XMLHTTP");
}
```

设置请求地址及⽅式

```js
/*
第⼀个参数是⽤于指定请求的⽅式，⼀般⽤⼤写
第二个参数代表请求的URL
第三个参数是表示是否异步发送请求的布尔值，如果不填写，默认为true，表示异步发送，同步已经被弃用
*/
          POST
xhr.open("GET","https://api.xdclass.net/pub/api/v1/web/index_card")
```

- 发送请求(可选参数， null)

  ```js
  xhr.send()
  ```

（注册事件）等到浏览器返回结果接受响应

```js
/* 
注册事件。 onreadystatechange事件，状态改变时就会调用。
如果要在数据完整请求回来的时候才调用，我们需要手动写一些判断的逻辑。
*/
xhr.onreadystatechange = function () {
  // 为了保证数据完整返回，我们一般会判断两个值
  if (xhr.readyState === 4 && xhr.status === 200) {
    alert(xhr.responseText);
  } else {
    alert('出错了，Err:' + xhr.status);
  }
};
```

![image-20211218195930846](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218195933.png)

# Javascript操作DOM

- DOM(w3c提的⼀个标准)

  - DOM就是⽂档对象模型，是⼀个抽象的概念
  - 定义了访问和操作HTML⽂档的⽅法

- HTML和txt⽂本的区别

  - HTML是有组织的结构化⽂件

  ![image-20211218200322962](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218200324.png)

  ![image-20211218200439129](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218200440.png)

- 什么是DOM树

  - 浏览器将结构化的⽂档以"树"的结构存储在浏览器内存⾥
  - 每个HTML元素被定义为节点
  - 这个节点有⾃⼰的属性(名称、类型、内容… ...)
  - 有⾃⼰的层级关系(parent, child, sibling)

- 图解html树

![image-20211218153229565](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218153232.png)

查找节点

|                 ⽅法                  |                描述                 |
| :-----------------------------------: | :---------------------------------: |
|      document.getElementById(id)      |       通过元素 id 来查找元素        |
|  document.getElementsByTagName(name)  |        通过标签名来查找元素         |
| document.getElementsByClassName(name) |         通过类名来查找元素          |
|   document.querySelector(selector)    | 通过css选择器选择元素，⽆法选择伪类 |

![image-20211218201254965](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218201256.png)

![image-20211218201451200](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218201452.png)

![image-20211218201636901](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218201638.png)

改变元素内容

|                  ⽅法                  |          描述          |
| :------------------------------------: | :--------------------: |
|  element.innerHTML = new html content  | 改变元素的 inner HTML  |
|     element.attribute = new value      | 改变 HTML 元素的属性值 |
| element.setAttribute(attribute, value) | 改变 HTML 元素的属性值 |
|   element.style.property = new style   |  改变 HTML 元素的样式  |

![image-20211218202123831](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218202125.png)

![image-20211218202323511](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218202325.png)

添加和删除元素

|              ⽅法               |      描述      |
| :-----------------------------: | :------------: |
| document.createElement(element) | 创建 HTML 元素 |
|  document.removeChild(element)  | 删除 HTML 元素 |
|  document.appendChild(element)  | 添加 HTML 元素 |
| document.replaceChild(element)  | 替换 HTML 元素 |
|      document.write(text)       |  可写⼊ HTML   |

![image-20211218202657638](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218202850.png)

![image-20211218202840474](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218202841.png)

**DOM中事件的概念**

- 什么是事件

  - 事件指的是在html元素上发⽣的事情
  - ⽐如图⽚元素被点击
  - 事件触发时，可设置执行⼀段js代码

- 常⻅的HTML事件

  |        描述        |    事件     |
  | :----------------: | :---------: |
  |  html元素发生改变  |  onchange   |
  |   用户点击了元素   |   onclick   |
  | 鼠标移动到了元素上 | onmouseover |
  |    鼠标离开元素    | onmouseout  |
  |      按下键盘      |  onkeydown  |
  |  页面已经完成加载  |   onload    |

- 怎么对事件作出反应

  - 通过元素的事件属性

  - 启⽤事件监听器

    - 什么是事件监听器

      - addEventListener给DOM对象添加事件处理程序

        ![image-20211218203632257](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218203633.png)

      - removeEventListener 删除给DOM对象的事件处理程序

         ![image-20211218203928483](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218203930.png)

- onclick和addEventListener的区别

  - onclick会被覆盖

    ![image-20211218204150113](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218204151.png)

  - addEventListener可以同时注册多个，根据注册顺序，先后执⾏

    ![image-20211218204329311](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218204331.png)

 **JS事件中的⼀些机制**

- 事件传播的两种机制

  - 冒泡

    ![image-20211218204646585](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218204648.png)

  - 捕获

- 图解事件捕获和事件冒泡

![image-20211218153326706](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218153334.png)

![image-20211218205044726](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218205046.png)

- 什么是事件代理

  - 思考：⽗级那么多⼦元素，怎么区分事件本应该是哪个⼦元素的 找到target中的textcontent

    ![image-20211218205227395](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218205228.png)

    ![image-20211218205335069](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218205339.png)

    - 事件代理就是利⽤事件冒泡，只指定⼀个事件处理程序，就可以管理某⼀类型的所有事件。

- 怎么取消冒泡或者捕获

```js
event.stopPropagation();
```

![image-20211218205633711](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218205634.png)

**JavaScript中的定时器**

- 延迟执⾏

  ```js
  setTimeout(function(){}, 毫秒)
  ```

  - 停止

    ```js
    clearTimeout(timer) // 参数必须是由 setTimeout() 返回的timer
    ```

- 定时执⾏

  ```js
  setInterval(function(){}, 毫秒)
  ```

  - 停止

    ```js
    clearInterval(timer) // 参数必须是由 setInterval() 返回的timer
    ```

# Javascript操作BOM

- 什么是BOM

  - 浏览器对象模型（Browser Object Model (BOM)）

- 内置对象

  - window

  - screen

  - location

    ![image-20211218210318812](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218210320.png)

  - localStorage

    ![image-20211218210410936](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218210412.png)

  - history

- 警告框

  ```js
  alert('hello') //可以作为断点使用
  ```

- 确认框

  ```js
  var isConfirm = confirm('请确认')
  console.log('下⼀步', isConfirm)// 有返回值的
  ```

- 提示框

  ```js
  var isPrompt = prompt('请输⼊姓名')js
  ```

  ```js
  console.log(isPrompt) // 是null 或者 ⽤户输⼊的值
  ```

  ![image-20211218210910660](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218210912.png)

**local storage 没有手动删除将永远存在**

**session storage 关闭浏览器就没有了**

**cookies storage 容量比较小 只有4kB**



![image-20211218211805166](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218211806.png)

- 什么是Cookie

  - Cookie 是计算机上存储浏览器的数据⽤的

  - 容量⼤⼩⼤概4KB

  - 基本语法

    ```
    document.cookie
    ```

- Cookie作用

  - 浏览器关闭后，服务器会忘记⽤户的⼀切
  - 让浏览器记住⽤户信息：登录信息

- cookie操作

  - 如何创建和读取cookie

```
// 通过Document对象
document.cookie="username=Nick; expires=Thu, 18 Dec 2043 12:00:00 GMT";
```

- 删除cookie

  ```
  // 设置过期时间
  document.cookie = "username=; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/;";  //  null
  ```

- 设置cookie函数

```js
// 保存cookie
function setCookie(cname, cvalue, exdays) {
  var d = new Date();
  d.setTime(d.getTime() + exdays * 24 * 60 * 60 * 1000);
  var expires = 'expires=' + d.toGMTString();
  document.cookie = cname + '=' + cvalue + ';' + expires;
}

// 获取cookie
function getCookie(cname) {
  var name = cname + '=';
  var ca = document.cookie.split(';'); // 将多个cookie字符串以;分割数组;
  for (var i = 0; i < ca.length; i++) {
    if (ca[i].indexOf(name)>= 0) {
      return ca[i].split('=')[1]
    }
  }
  return '';
}

// 
function checkCookie(){
  var user=getCookie("username");
  if (user){
    alert("欢迎 " + user + " 再次访问");
  }
  else {
    user = prompt("请输入你的名字:","");
      if (user){
        setCookie("username",user,30);
      }
  }
}
```

# 小滴课堂轮播图项目实战

JSON

- JSON数据

  

```js
{
  "code": 0,
  "data": [
    { "name": "后端&架构" },
    { "name": "前端&全栈" },
    { "name": "测试&测开" },
    { "name": "运维&容器" },
    { "name": "架构&管理" },
    { "name": "算法|人工智能" },
    { "name": "大数据" }
  ]
}
```

axios

官网：http://www.axios-js.com/zh-cn/docs/

- 引入

  ```js
  <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
  ```

- 使用

  ```js
  axios
  .get('xxx')
  .then(function (response) {
    // 请求到数据执行的操作
  })
  .catch(function (error) { // 失败时抛出异常
    console.log(error);
  });
  ```

**轮播图左侧导航数据动态渲染**

- 导航列表数据获取

  - 调用axios请求本地JSON数据
  - 异步中加入回调函数

- 插入到html对应的节点中

  - for循环遍历插入对应节点中渲染

    ![image-20211218214546676](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218214548.png)

**轮播图图片和圆点动态渲染**

- 轮播图片动态渲染

  - 调用axios请求本地json数据
  - for循环遍历插入对应节点中渲染

- 圆点个数根据图片个数动态展示

  - 获取图片个数

  - for循环累加个数插入对应节点中渲染

    ![image-20211218214735327](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218214736.png)

    ![image-20211218215422664](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218215424.png)

    ![image-20211218222816344](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218222820.png)

**轮播图自动播放和圆点的样式自动切换**

- 图片

  - 初始化第一张展示

  - 定时器实现索引累加

  - 通过索引动态展示图片

    - 注意：索引大于图片个数减一时，重置索引为0，小于0时，重置为图片个数减一

      ![image-20211218223136560](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218223138.png)

      opacity不透明性 为0则说明透明，为1则说明不透明，展示的意思

      

      ![image-20211218223353033](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218223355.png)

      ![image-20211218231805759](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218231807.png)

      

      代码还是有问题的

      ![image-20211218231856268](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218231858.png)

      ![image-20211218232011675](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218232013.png)

      ![image-20211218232313408](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218232314.png)

- 圆点

  - 设置选中时的样式
  - 根据判断索引值来动态改变圆点样式

**轮播图箭头按钮和圆点交互**

- 箭头按钮

  - 按钮添加点击监听事件
  - 点击上一张索引减一，点击下一张索引加一
  - 在函数内加入更换图片的回调函数

- 圆点按钮

  - 遍历所有的圆点
  - 给每个圆点添加点击监听事件
  - 将点击的圆点索引赋值给公共索引
  - 在函数内加入更换图片的回调函数

  ![image-20211218232909730](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218232911.png)

**轮播图效果优化之鼠标交互**

- 轮播图效果优化
  - 获取按钮的节点
  - 给节点添加鼠标悬停与离开的监听事件
  - 鼠标悬停时清除定时器
  - 鼠标离开时重新调用图片轮播

 ![image-20211218233228726](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211218233230.png)

- 样式问题
- 功能问题
- 解决思路

 