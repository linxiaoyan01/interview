---
title: HTML+CSS基础到实战
categories: 前端
tags: [HTML,CSS]
---

# 迈入前端的第一步

- 独⽴内核的浏览器的分类
  - Chrome: Webkit / Blink
  - IE: Trident
  - Safari：Webkit
  - Firefox：Gecko
  - Opera: Presto / Blink

- 文档声明

  - 告诉浏览器网页是以html5版本编写的

- meta标签

  - 自结束标签

    ```
    <meta>
    <link>
    <hr> 一条横线
    <img> alt属性：图⽚加载失败或加载时显示的⽂字 title属性 悬浮在图⽚上⾯显示的⽂字
    <input>
    ```

  - 可以提供该网页相关信息,元数据

  - charset="utf-8"：中文的网页需要用到的声明编码，否则会出现乱码

  - name="keywords" content="静夜思,诗词"：提供网页的关键字，关键字用,隔开

  - name="Description"：描述网页的信息

- 空格

  ![image-20211217100041473](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217100740.png)

  ```
  &nbsp;
  ```

- 语义化标签

  ![image-20211217102204348](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217102206.png)

  ![image-20211217102116764](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217102118.png)

- table元素⾥常⽤的属性

  - border(边框)
  - cellspacing(规定单元格之间的空白)
  - cellpadding(规定单元边沿与其内容之间的空白)
  - colspan(⽤于合并列)
  - rowspan(⽤于合并⾏)

- a标签

  - target属性：brank：打开新的浏览器标签

  - 锚点 

    ![image-20211217102540507](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217102541.png)

    

- form标签：使⽤场景需要提交⼀些信息到服务端的时候(前后端联调过程中)

  - 核⼼元素input (核⼼元素)
  - label (提⾼交互体验的)
  - select(下拉框)
  - textarea(⽂本域)
  - button(按钮)
  - form(表单元素，提交每个表单项内容)

- 块级元素

  - 块级元素在浏览器显示时，通常会以新行来开始（和结束）

    ```
    <h1>, <p>, <ul>, <table>,<div>
    ```

- 行内元素

  - 行内元素在显示时通常不会以新行开始

    ```
    <b>, <td>, <a>, <img>,<span>
    ```

# CSS入门之基本规则

- 选择器
  - 告诉浏览器要设置样式的html元素
- 声明块
  - ⽤于设置样式
- 层叠样式表
- Viewport
  - 将网页的视口设置为完美视口，开发移动端页面时一定要加上

**CSS的⼏种写法及推荐写法**

- 写法
  - 内部样式表
    - 写在元素的style标签⾥⾯的
  - 内联样式表
    - 写在styles属性⾥⾯的
  - 外部样式表
    - link标签将css资源引⼊
- 为什么选择外部样式表
  - 解决⻚⾯当中样式重复的问题
  - 代码分离，利于代码维护和阅读
  - 浏览器会缓存起来，提⾼⻚⾯响应速度变快了

**CSS核⼼之常⻅的选择器及使⽤场景**

- 元素（标签）选择器

  ```
  组合选择器: h1,p{color:red}
  ```

- 类选择器

  - 结合标签选择器

    ```
    h1.xiaodi {color:red;}
    ```

  - 多类选择器

    ```
    class="xiaodi background"
    ```

  - 链接多个类选择器

    ```
    .xiaodi.background{color:red; background-color:black}
    ```

- id选择器

  ```
  声明：#important{} 
  属性：id="important
  注意：一个id选择器的属性值在html文档中只能出现一次，避免写js获取id时出现混淆
  ```

- 通配符选择器*

- 派⽣选择器

  - 后代选择器

    ```
    h1 p{color:red;}
    ```

  - ⼦元素选择器

    ```
    div>span{font-size:900}
    ```

  - 相邻选择器（兄弟）

    ```
    h1+p{background-color:pink;}
    ```

 **CSS中特殊的选择器—伪类选择器的使⽤**

- 特殊的选择器

  - 伪类选择器
    - 不改变任何DOM内容。只是插入了一些修饰类的元素

  ```
  :first-child {}     //第一项
  ```

  ```
  :last-child {}      //最后一项
  ```

  ```
  :nth-child(n) {}    //第n项 
  ```

  ```
  :nth-child(2n+1){}  //奇数项
  ```

  ```
  :nth-child(2n) {}   //偶数项
  ```

  ```
  :not()              //否定伪类 除了第n项
  ```

  ```
  a:link {color:#FF0000;}     /*未访问的链接*/
  ```

  ```
  a:visited {color:#00FF00;}  /*已访问的链接*/
  ```

  ```
  a:hover {color:#FF00FF;}    /*⿏标悬浮后的链接*/
  ```

  ```
  a:active {color:#0000FF;}   /*已选中的链接*/
  ```

  - 伪元素选择器

  ```
  ::first-letter  //第一个
  ```

  ```
  ::first-line    //第一行 只能用于块级元素
  ```

  ```
  ::selection     //选中
  ```

  ```
  ::before        //在开始位置
  ```

  ```
  ::after         //在结束位置
  ```

  ![image-20211217135528860](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217135530.png)

  ![image-20211217135608118](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217135609.png)

  ![image-20211217135646421](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217135647.png)

  ![image-20211217135723309](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217135724.png)

  ![image-20211217135855719](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217135857.png)

  ![image-20211217135920788](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217135925.png)

**CSS基本概念之盒⼦模型**

在CSS⾥⾯，所有的HTML元素你都可以看成是⼀个盒⼦

- 盒⼦的内容(content)
  - 元素的大小
- 盒⼦的内边距(padding)

```
padding-left:10px   //左边距10px
padding-top:10px    //上边距10px
padding-right:10px  //右边距10px
padding-bottom:10px //下边距10%，相对于父级元素的width

padding:10px 10px 10px 10% //等同于上面四行 百分比是对应父标签的大小
padding:5px 10px    //上下边距5px、左右边距10px
padding:5px 10px 20px  //上边距 左右边距 下边距
padding:10px        //上下左右边距10px
```

- 盒⼦的边框(border)

```
border-left:3px solid #000  //左边距10px dotted dashed
border-top:3px solid #000 //上边距10px
border-right:3px solid #000 //右边距10px
border-bottom:3px solid #000  //下边距10%，相对于父级元素的width

border:3px solid #000 //等同于上面四行
```

- 盒⼦的外边距(margin)

  ```
  margin-left:10px  //左边距10px
  margin-top:10px   //上边距10px
  margin-right:10px //右边距10px
  margin-bottom:10% //下边距10%，相对于父级元素的width
  
  margin:10px 10px 10px 10% //等同于上面四行
  margin:5px 10px   //上下边距5px、左右边距10px
  margin:10px       //上下左右边距10px
  ```

- 盒子怪异模型

  - W3C标准的盒子模型（标准盒模型 )

    ```
    boxWidth=contentWidth
    ```

  - IE标准的盒子模型（怪异盒模型）

  ```
  box-sizing:border-box //声明
  boxWidth=contentWidth+border+padding
  ```

- 外边距折叠

  - 上下两个兄弟盒子的margin都为正取大，都为负取小，一正一负相加

  - 父子元素盒子的margin（假设没有内边距或边框把外边距分隔开），也会合并；

    只有普通文档流中块框的垂直外边距才会发生外边距合并。行内框、浮动框或绝对定位之间的外边距不会合并

    ```
    解决父子边距合并：
    父元素{
      overflow:auto;
    }
    
    父元素::before{
      display: table;
      content: "";
    }
    ```

![image-20211217130138293](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217130140.png)

![image-20211217122310155](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217122311.png)

![image-20211217123415410](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217123417.png)

![image-20211217123626568](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217123628.png)

![image-20211217123653721](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217123655.png)

顺时针

![image-20211217123743470](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217123744.png)

![image-20211217124635120](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217124636.png)

![image-20211217124544131](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217124545.png)

- 常⽤属性

  - 盒⼦的位置和⼤⼩

    - 尺寸

      ```
      宽度 width: ⻓度|百分⽐|auto
      ```

      ```
      ⾼度 height
      ```

      ```
      边界 margin padding 上右下左|上下左右
      ```

    - 布局

      ```
      浮动：float
      ```

      ```
      定位：position
      ```

      ```
      弹性布局：flex
      ```

      ```
      盒⼦内容超出部分：overflow：hidden | scroll | auto
      ```

  - 外观，⻛格

    ```
    background-image
    ```

    ```
    background-repeat
    ```

    ```
    background-size
    ```

    ```
    background-position
    ```

    ```
    background-color
    ```

    - ⽂字属性

      ```
      字体⼤⼩ font-size
      ```

      ```
      是否加粗 font-weight
      ```

      ```
      是不是斜体 font-style
      ```

      ```
      字体是什么 font-family
      ```

![image-20211217132156124](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217132157.png)

- CSS全称：层叠样式表(Cascading Style Sheets)
- 层叠是⼀个基本特征
  - ⼀个css属性被多次声明的时候，会根据优先级或者声明顺序来计算采⽤哪个样式
- 优先级是怎么计算
  - 通配符选择器 1： *
  - 标签选择器 2：div/span/p/li
  - 类选择器 3：class
  - id选择器 6：id
  - 行内样式 5
  - !important 6（尽量不要在公⽤代码⾥使⽤）

![image-20211217133320114](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217133321.png)

**CSS中常⻅的可继承的属性**

- 什么是继承
  - 继承了⽗元素的某些属性
  - 优点：继承可以简化代码，便于维护
- 默认设置继承的属性
  - ⽂字属性，文本缩进、对齐、行高

**练习：第二个子元素鼠标悬浮时，字体为红色，背景颜色为绿色，即使子元素个数发生变化都满足**

![image-20211217135133499](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217135134.png)

# CSS进阶之布局定位

## CSS中布局前置知识

- 正常元素怎么布局
  - 默认，⼀个块级元素的内容宽度就是其⽗元素的100%，他的⾼度和其内容⾼度⼀致
  - ⾏内元素它的宽度和⾼度都是根据内容决定的(⽆法设置⾏内元素的宽⾼)
    - 可设置display属性，定义元素的类型(css3定义布局)
- 元素之间⼜是如何相互影响的呢
  - 正常的⽂档布局流
    - 每个块级元素会在上个元素下⾯另起⼀⾏
    - ⾏内元素不会另起⼀⾏

![image-20211217142345254](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217142346.png)

![image-20211217142326896](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217142328.png)

![image-20211217142411073](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217142412.png)

## CSS中必须掌握的float布局

- float布局

  - 使用

    ```
    float: none;  //默认值，元素不浮动
    float: left;  //元素像左浮动
    float: right; //元素像右浮动
    ```

  - 特点

    - 浮动元素会脱离文档流，不再占据文档流空间
    - 浮动元素彼此之间是从左往右排列，宽度超过一行自动换行
    - 在浮动元素前面元素不浮动时，浮动元素无法上移
    - 块级元素和行内元素浮动之后都变成行内块级元素
    - 浮动元素不会盖住文字，可以设置文字环绕效果

  - 清除浮动

    ![image-20211217150215607](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217150508.png)

    ```
    clear:both;
    content:'';
    display:block;
    ```

  ![image-20211217150254474](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217150256.png)

## CSS中必须掌握的flex布局

flex布局（弹性布局） CSS3知识点

- 父元素容器的属性

![image-20211217142632202](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217142638.png)

```
/* 设为 Flex 布局以后，子元素的float、clear和vertical-align属性将失效。*/
display: flex;

/* 决定主轴的方向（即项目的排列方向）*/
flex-direction: row | row-reverse | column | column-reverse;

/* 是否换行 */
flex-wrap: nowrap | wrap | wrap-reverse; 

/* 定义水平方向对齐方式 */
justify-content: flex-start | flex-end | center | space-between | space-around;

/* 定义垂直方向对齐方式 */
align-items: flex-start | flex-end | center | baseline | stretch;

/* 定义多个轴线（多行/多列）对齐方式 */
align-content: flex-start | flex-end | center | space-between | space-around | stretch;
```

![image-20211217151218111](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217151219.png)

![image-20211217151337499](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217151339.png)

![image-20211217160017322](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217160018.png)

![image-20211217154942556](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217154945.png)

![image-20211217155018341](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217155020.png)

![image-20211217155102617](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217155104.png)

![image-20211217155130743](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217155132.png)

![image-20211217155234288](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217155236.png)

![image-20211217155250811](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217155252.png)

![image-20211217155338834](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217155340.png)

![image-20211217155350226](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217155351.png)

![image-20211217155509167](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217160046.png)

![image-20211217155622917](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217160041.png)

![image-20211217160433366](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217160434.png)

![image-20211217160503362](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217160506.png)

![image-20211217160547545](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217160549.png)

![image-20211217160652401](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217160653.png)

![image-20211217161314498](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217161315.png)

![image-20211217161327428](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217161335.png)

![image-20211217161429493](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217161430.png)

![image-20211217161640159](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217161710.png)

- 子元素容器的属性

```
/* 定义子元素的排列顺序，默认为0 */
order: -10 | -1 | 0 | 1 | 10;

/* 定义子元素的放大比例，默认为0 */
flex-grow: 0 | 1 | 2 | 3;

/* 定义子元素的缩小比例，默认为1 */
flex-shrink: 0 | 1;

/* 定义了在分配多余空间之前，项目占据的主轴空间 */
flex-basis: <length> | auto;

/* flex-grow, flex-shrink 和 flex-basis的简写 */
flex: 0 1 auto;
```

- 兼容性
  - 浏览器ie9及以上
- 选择float布局 or flex布局？
  - 推荐是使用flex布局
    - flex布局易用，布局全面
    - float的创建初衷只是为了达到文字环绕的效果，另外需要清除浮动
    - 现在几乎所有公司的产品使用场景都在浏览器ie9以上

## CSS中必须掌握的position定位

- 也是为布局引⼊的属性
- position常⽤的⼏个值

```
position: static   (静态定位)   父元素
position: relative (相对绝对)   父元素
position: absolute (绝对定位)

position: fixed    (固定定位)   父元素
position: sticky   (粘性定位)   父元素
```

![image-20211217162733897](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217162735.png)

![image-20211217162751064](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217162752.png)

![image-20211217162947293](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217162949.png)

![image-20211217163348899](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217163350.png)

![image-20211217163508405](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217163510.png)

![image-20211217163648535](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217211125.png)

position : sticky

![image-20211217163733279](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217163734.png)

相关的属性

```
z-index //使⽤场景：当定位的盒⼦重叠在⼀起
```

![image-20211217163952104](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217163953.png)

## CSS之经典的三栏布局如何实现

- 问题：⾼度固定，左右两侧的盒⼦宽度为200px，中间⾃适应
  - float

![image-20211217164438224](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217164439.png)

![image-20211217164713512](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217164715.png)

![image-20211217164847623](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217164849.png)

![image-20211217164948331](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217164950.png)

- position

  ![image-20211217165136190](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217165137.png)

  - flex

  ![image-20211217165425406](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217165859.png)

  ![image-20211217165854767](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217165856.png)

- 解决
  - float实现
  - position实现
  - flex实现

## CSS之⽔平垂直居中的常用方法

如何使⽤CSS实现⽔平垂直居中

- ⾏内块元素

```
1. line-height
   text-align: center
2. display: flex;
   justify-content: center;
   align-items: center;
```

![image-20211217170118726](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217170120.png)

![image-20211217170303115](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217170304.png)

- 块级元素

```
1. position + margin    清楚子元素的宽高
2. position + transform 不清楚子元素的宽高
3. flex     
4. table-cell           兼容性较差
```

![image-20211217170656027](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217170657.png)

![image-20211217170819755](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217170822.png)

![image-20211217171005821](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217171007.png)

![image-20211217171131025](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217171132.png)

## CSS扩展⾼级知识点之BFC

- 定义

  - **块格式化上下⽂**（Block Formatting Context，BFC）是Web⻚⾯的可视化CSS渲染的⼀部分，是块盒⼦的布局过程发⽣的区域，也是浮动元素与其他元素交互的区域。
  - 即：形成了⼀块封闭的区域，能检测到区域内脱离⽂档流的元素

- 特点

  - css中隐含的属性，开启后不会被浮动的元素覆盖
  - BFC元素可以包含浮动元素
  - BFC元素的子元素和父元素外边距不重叠

- 开启（都会有副作用）

  - 设置元素浮动 float：left

  - 设置为行内块元素 display：inline-block

  - overflow：hidden（推荐）

    ![image-20211217181633482](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217181635.png)

    发生重叠

    ![image-20211217182111245](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217182113.png)

    ![image-20211217182147563](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217182149.png)

- 作⽤

  - 清除浮动带来的影响
  - 解决边距塌陷问题(外边距折叠（Margin collapsing）也只会发⽣在属于同⼀BFC的块级元素之间)

**实现小滴课堂的悬浮二维码与联系方式**

考察position: fixed定位

![image-20211217183148654](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217183150.png)

箭头的实现

![image-20211217183409094](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217183410.png)

# CSS3常用属性讲解

## CSS3边框

- 圆角

  ```
  border-radius
  ```

  ![image-20211217183819179](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217183835.png)

![image-20211217183904452](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217183905.png)

- 盒阴影

  ```
  box-shadow
  ```

  ![image-20211217184123925](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217184125.png)

  box-shadow 第一个值是指向左还是向右，如果为正数则向右，反之则向左，第二个值若为正数则向下，反之向上，第三个值是指虚化度越大越虚

- 边界图片

  ```
  border-image
  ```

## CSS3渐变

- 基本语法

  ```
  /* 从上到下（默认情况下）*/
  background-image: linear-gradient(#e66465, #9198e5);
  
  /* 从左到右 */
  background-image: linear-gradient(to right, red , yellow);
  
  /* 对角 */
  background-image: linear-gradient(to bottom right, red, yellow);
  ```

  ![image-20211217184541236](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217184542.png)

- 使用角度

  ```
  /* 从上到下 */
  background-image: linear-gradient(180deg, red, yellow); 
  /* 从左到右 */
  background-image: linear-gradient(90deg, red, yellow);
  ```

- 多个颜色

  ```
  /* 从上到下 */
  background-image: linear-gradient(red, yellow, green);
  /* 从左到右 */
  background-image: linear-gradient(to right, red, orange, yellow);
  ```

- 透明度

  ```
  background-image: linear-gradient(rgba(255,0,0,0), rgba(255,0,0,1));
  ```

  ![image-20211217184830932](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217211402.png)

- 重复

  ```
  background-image: repeating-linear-gradient(red, yellow 10%, green 20%);
  ```

  ![image-20211217185319867](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217185321.png)

  

## CSS3文本效果

- 文本阴影

  ```
  text-shadow: 5px 5px 5px #FF0000;
  ```

- 文本溢出

  ![image-20211217185652629](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217185653.png)

  ![image-20211217185734974](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217185736.png)

  ![image-20211217185902847](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217185907.png)

  ![image-20211217190100518](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217190102.png)

```
/* 超过1行省略/裁剪 */
overflow: hidden;
white-space: nowrap;
text-overflow: ellipsis | clip; 

/* 超过2行省略/裁剪 */
overflow: hidden;
text-overflow: ellipsis;
display: -webkit-box;
-webkit-box-orient: vertical;
-webkit-line-clamp: 2;
```

- 文本换行

  - 长文本换行

    ```
    word-wrap:break-word;
    ```

  - 单词拆分换行

    ```
    word-break: break-all;
    ```

 # CSS3网格布局（grid）

- 应用场景

  - flex布局、float布局应用于一维布局，网格布局应用于二位布局

  ![image-20211217190447184](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217190448.png)

  ![image-20211217191304293](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217191306.png)

- 父元素属性

```
/* 使用 */
display: grid;
grid-template-columns: 10px 10px 10px;
grid-template-rows: 10px 10px 10px;

/* 百分比使用 */
display: grid;
grid-template-columns: 33.33% 33.33% 33.33%;
grid-template-rows: 33.33% 33.33% 33.33%;

/* repeat()函数简化 */
display: grid;
grid-template-columns: repeat(3, 33.33%);
grid-template-rows: repeat(3, 33.33%);
```

 # 在线教育首页多功能实战

## 真实的项目开发是怎么样的以及页面实战分析

- 真实项目开发流程
  - 需求评审
  - 与UI对接设计图
  - 开发页面
  - 与后端程序员协调接口协议
  - 接入接口数据联调
  - 开发完成，提交测试环境，自测完成然后提测
  - 解决bug
  - 提交预发环境
  - 产品，测试确认，与后端一起提交上正式环境（上线）
- 页面分析

## 项目初始化和阿里巴巴矢量库的使用

![image-20211217192017679](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217192019.png)

![image-20211217192047434](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217192049.png)

- 项目初始化

  - 新建文件夹
  - 初始化css样式：去掉初始的margin值，li标签的点去掉等等

  ![image-20211217192413662](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217192415.png)

- 阿里巴巴矢量库

  - 地址

    - https://www.iconfont.cn/

    ![image-20211217192803754](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217192805.png)

  - 流程

  ```
  - 注册
  - 搜索图片
  - 加入购物车
  - 添加进项目、选择symbol格式
  - 生成在线链接
  ```

- 引入第三方icon 

  ```
  // 根文件html引入  注意：xxxxxxxx是自己项目的在线链接
  <script src="http:xxxxxxxx"></script>
  
  // 使用 注意 xxxx是icon的代码
  <svg class="icon" aria-hidden="true">
      <use xlink:href="#xxxx"></use>
  </svg>
  ```

  ![image-20211217192944166](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217192945.png)

  ![image-20211217192926559](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217192928.png)

## 小滴课堂首页导航栏开发

**首页导航栏的开发**

 ![image-20211217143447072](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217143448.png)

## 小滴课堂首页轮播图开发

![image-20211217200458785](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217200501.png)

![image-20211217200515173](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217200517.png)

![image-20211217143525571](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217143526.png)

## 小滴课堂首页课程列表开发上

![image-20211217143543579](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217143545.png)

![image-20211217203135813](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217203137.png)

![image-20211217203512400](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217203514.png)

## 小滴课堂首页课程列表开发下

![image-20211217143556092](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217211452.png)

## 小滴课堂首页底部开发

![image-20211217143607468](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211217143609.png)

# 接下来

- 项⽬开发过程
  - 先分析⼀个布局，分块细⼀点
  - 写对应的注释
  - 考虑周全⼀点
- 项⽬遇到的问题
  - 布局乱了怎么办
    - 检查其他盒⼦的⾼度是否影响了
  - 换着思路实现问题，⽐如我们的伪类来实现布局
  - 学会iconfont使⽤
  - 未来的规划
    - html + css(最基本的)
    - js(跟⽹⻚交互使⽤的) javascript
      - Vue.js
      - React.js
  - 项⽬的构建
    - git(代码管理⼯具)
    - webpack(打包⼯具，打包资源代码)
  - 后端
    - node.js(多了操作数据库的api)
  - 学习技巧
    - 只有当你的实际练习（写代码）⼤⼤于你的理论学习时间， 你才会有进步
    - 锻炼下⾃⼰独⽴学习能⼒(必须学会看懂编辑器的提示，学会翻译)
    - 多多看mdn⽂档，博⽂只能参考