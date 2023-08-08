---
title: Redis设计与实现之读书笔记
categories: Redis
tags: [Redis]
---

# 第一部分 数据结构与对象

## 第二章 简单动态字符串

![image-20211208133233464](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208133235.png)

![image-20211208133243593](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208133246.png)

![image-20211208133343784](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208133345.png)

![image-20211208133322367](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208133323.png)

![image-20211208133426544](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208133427.png)

![image-20211208133448741](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208133450.png)

![image-20211208133501416](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208133503.png)

![image-20211208133527862](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208133529.png)

Redis 只会使用 C 字符串作为字面量，在大多数情况下，Redis 使用 SDS（Simple Dynamic String，简单动态字符串）作为字符串表示。 

**比起 C 字符串，SDS 具有以下优点：** 

1）常数复杂度获取字符串长度。 2）杜绝缓冲区溢出。 3）减少修改字符串长度时所需的内存重分配次数。 4）二进制安全。 5）兼容部分 C 字符串函数。

## 第三章 链表

列表键的底层实现之一，当一个列表键包含了数量比较多的元素，又或者列表中包含的元素都是比较长的字符串时，Redis就会使用链表来作为列表键的底层实现。

![image-20211208133715387](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208133716.png)

![image-20211208133736524](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208133737.png)

![image-20211208133803987](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208133805.png)

链表被广泛用于实现 Redis 的各种功能，比如列表键、发布与订阅、慢查询、监视 器等。 每个链表节点由一个 listNode 结构来表示，每个节点都有一个指向前置节点和后 置节点的指针，所以 Redis 的链表实现是双端链表。每个链表使用一个 list 结构来表示，这个结构带有表头节点指针、表尾节点指针， 以及链表长度等信息。因为链表表头节点的前置节点和表尾节点的后置节点都指向 NULL，所以 Redis 的链 表实现是无环链表。通过为链表设置不同的类型特定函数，Redis 的链表可以用于保存各种不同类型的值。

## 第四章 字典

除了用来表示数据库之外，字典还是哈希键的底层实现之一，当一个哈希键包含的键值对比较多，又或者键值对中的元素都是比较长的字符串时，redis就会使用字典作为哈希键的底层实现。redis的字典使用哈希表作为底层实现，一个哈希表里面有多个哈希表节点，每个哈希表节点就保存了字典中的一个键值对。

![image-20211208150914165](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208150915.png)

![image-20211208151054413](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208151056.png)

![image-20211208150939376](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208150940.png)

![image-20211208151001052](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208151002.png)

![image-20211208151153461](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208151154.png)

![image-20211208151244494](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208151245.png)

**ht[1] 哈希表只会对ht[0] 哈希表进行rehash 时使用**

![image-20211208153924631](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208153925.png)

![image-20211208154917059](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208154920.png)

![image-20211208155037644](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208155038.png)

**Redis用MurmurHash算法来计算键的哈希值，该算法可以给出一个很好的随机性。**

![image-20211208155558784](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208155600.png)

**程序总是将新节点添加到链表的表头位置**

![image-20211208160110880](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208160113.png)

![image-20211208160237418](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208160238.png)

![image-20211208160408665](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208160409.png)

**//TODO 等我看完操作系统回头来看看这是什么意思**

![image-20211208213829644](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208213838.png)

渐进式rehash

![image-20211208214541860](C:/Users/86158/AppData/Roaming/Typora/typora-user-images/image-20211208214541860.png)

![image-20211208214556018](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208214557.png)

![image-20211208214608613](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208214609.png)

![image-20211208214623220](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208214624.png)

![image-20211208214650133](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208214651.png)

![image-20211208214708196](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208214709.png)

![image-20211208214728758](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208214729.png)

![image-20211208215642393](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208215643.png)

![image-20211208215925322](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208215927.png)

## 第五章 跳跃表

跳跃表支持平均O(logN)，最坏O(N) 复杂度查找，作为有序集合键的底层实现之一，redis只在两个地方用到了跳跃表，一个是实现有序集合键，另一个是在集群节点中用作内部数据结构。

![image-20211208221807338](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208221808.png)

![image-20211208223123554](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208223124.png)

![image-20211209000615498](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209000617.png)

![image-20211208223332753](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208223334.png)

![image-20211208223509756](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208223511.png)

![image-20211208224341759](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208224343.png)

![image-20211208224412303](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208224413.png)

跨度实际上是用来计算排位rank的，将沿途访问过的所有层的跨度累计起来，得到的结果就是目标节点在跳跃表中的排位。

![image-20211208224949435](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208224950.png)

![image-20211208225009825](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208225010.png)

**后退指针每次只能后退至前一个节点。**

![image-20211208225500811](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211208225502.png)

score 是一个double类型的浮点数

obj属性是一个指针，指向一个字符串对象，字符串对象保存着一个SDS值

同一个跳跃表，各个节点保存的成员对象必须是唯一的，多个节点保存的分值可以相同，分值相同的节点将按照成员对象在字典序中的大小来进行排序，成员对象较小的节点排在前面。

![image-20211208232636459](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209154531.png)

![image-20211209001150712](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209001152.png)

**每个跳跃表节点的层高都是1至32之间的随机数**

## 第六章 整数集合

intset是集合键的底层实现之一，当一个集合中只包含整数，并且元素数量不多时，Redis就会使用这个inset作为集合键的底层实现

![image-20211209003212392](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209003213.png)

![image-20211209003228029](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209003229.png)

![image-20211209003243744](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209003245.png)

**升级**

升级首先要做的就是根据新类型的长度，以及集合元素的数量（包括要添加的新元素在内），对底层数组进行空间重分配。

![image-20211209083346846](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209084557.png)

![image-20211209083639526](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209084553.png)

![image-20211209083935824](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209084548.png)

新元素小于现有元素的情况下，新元素会被放置在底层数组的最开头（索引0）；

新元素大于现有元素的情况下，新元素会被放置在底层数组的最末尾（索引 length - 1）；

**每次向整数集合添加新元素都可能会引起升级，而每次升级都需要对底层数组中已经有的所有元素进行类型转换，所以新元素的时间复杂度为O(N)**

![image-20211209085323681](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209085324.png)

## 第七章 压缩列表

压缩列表ziplist是列表键和哈希键的底层实现之一。当一个列表键只包含少量列表项，并且每个列表项要么是小整数值，要么就是长度比较短的字符串，那么Redis就会使用压缩列表来做列表键的底层实现。

![image-20211209091507303](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209091508.png)

![image-20211209092104548](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209092105.png)

![image-20211209092247009](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209092248.png)

![image-20211209092516820](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209092517.png)

![image-20211209092943262](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209092949.png)

![image-20211209093116855](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209093118.png)

![image-20211209093333990](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209093335.png)

**encoding**

一字节、两字节或者五字节长，值的最高位位00、01或者10的是字节数组编码，这种编码表示节点的content属性保存着字节数组，数组的长度由编码除去最高两位之后的其他位记录；

一字节长，值的最高位以11开头的是整数编码，这种编码表示节点的content属性保存着整数值，整数值的类型和长度由编码除去最高两位之后的其他位记录；

![image-20211209094054538](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209094055.png)

![image-20211209095939908](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209095941.png)

![image-20211209100153485](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209100155.png)

**连锁更新**

![image-20211209102431069](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209102432.png)

**ziplistPush等命令的平均复杂度仅为O(N)**

因为ziplistPush ziplistInsert ziplistDelete ziplistDeleteRange 四个函数都有可能引发连锁更新，所以他们的最坏复杂度都是$O(N^2)$，但是这种操作出现的几率不高。

因为连锁更新在最坏情况下需要对压缩列表执行N次空间重分配操作，而每次空间重分配的最坏复杂度为O(N)，所以连锁更新的最坏复杂度为$O(N^2)$

## 第八章 对象

redis基于sds，双端链表，字典，压缩链表，整数集合来创建了一个对象系统，这个系统包含字符串对象，列表对象，哈希对象，集合对象，有序集合对象着五种类型的对象。

redis对象系统实现了基于引用计数技术的内存回收机制，另外，redis通过引用计数技术实现了对象共享机制，这个机制可以在适当的条件下，通过让多个数据库键共享同i对象来节约内存。

在服务器启用了maxmemory功能的情况下，数据库键的空转时长较大的优先被服务器删除，因为redis的对象带有访问时间记录信息。

![image-20211209110109010](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209110110.png)

![image-20211209110125157](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209110126.png)

键总是一个字符串对象，而值可以是字符串对象，列表对象，哈希对象，集合对象或者有序经济和对象的其中一种。

“字符串键”是指这个数据库对应的值为字符串对象，

“列表键”是指这个数据库所对应的值为列表对象。

![image-20211209110903574](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209110905.png)

![image-20211209111027230](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209111028.png)

![image-20211209113435457](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209113436.png)

![image-20211209114737745](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209114739.png)

![image-20211209114745926](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209114747.png)

![image-20211209120248399](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209120249.png)

**字符串值的长度大于32字节**

![image-20211209120428846](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209120430.png)

**字符串值的长度小于32字节**

![image-20211209132140125](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209132141.png)

![image-20211209120645571](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209120646.png)

![image-20211209141016926](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209141018.png)

long double 类型表示的浮点数在redis中也是作为字符串值来保存的。

![image-20211209141558833](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209141600.png)

![image-20211209142129698](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209142131.png)

![image-20211209142316091](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209142317.png)

![image-20211209143100345](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209143102.png)

![image-20211209143022959](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209143025.png)

![image-20211209143413748](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209143415.png)

![image-20211209144148618](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209144150.png)

![image-20211209144254788](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209144256.png)

![image-20211209144554248](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209144555.png)

**哈希对象**

![image-20211209145056236](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209145057.png)

![image-20211209144942565](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209144943.png)

![image-20211209145001143](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209145002.png)

![image-20211209145205256](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209145206.png)

![image-20211209145254387](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209145255.png)

![image-20211209145612080](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209145613.png)

![image-20211209145748195](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209145750.png)

**集合对象**

![image-20211209150153367](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209150154.png)

![image-20211209150704379](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209150705.png)

![image-20211209150908076](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209150909.png)

![image-20211209150925898](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209150927.png)

**有序集合对象**

![image-20211209151411365](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209151413.png)

![image-20211209151426613](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209151428.png)

![image-20211209151500004](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209151501.png)

**skiplist编码的有序集合对象使用zset结构作为底层实现，一个zset结构包含一个字典和一个跳跃表。**

![image-20211209151727732](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209151729.png)

![image-20211209151749487](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209151750.png)

![image-20211209152438709](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209152440.png)

![image-20211209152544089](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209152545.png)

**有序集合每个元素的成员都是一个字符串对象，而每个元素的分值都是一个double类型的浮点数。虽然zset结构同时使用跳跃表和字典来保存有序集合元素，但这两种数据结构都会通过指针来共享相同元素的成员和分支，所以同时使用跳跃表和字典来保存集合元素不会产生任何重复成员或者分值。**

![image-20211209153311597](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209153315.png)

![image-20211209153441259](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209153442.png)

**有序集合键的值为哈希对象，所以用于有序集合键的所有命令都是针对哈希对象来构建的。**

![image-20211209153854025](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209153855.png)

![image-20211209154232928](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209234136.png)

![image-20211209154246812](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209154248.png)

![image-20211209154345945](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209154347.png)

![image-20211209154743800](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209154744.png)

![image-20211209155031195](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209155032.png)

![image-20211209155204185](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209155205.png)

![image-20211209155242715](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209155244.png)

![image-20211209155400261](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209155401.png)

![image-20211209155617230](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209155618.png)

![image-20211209155750060](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209155751.png)

![image-20211209155906203](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209155907.png)

在初始化服务器时，创建一万个字符串对象，这些对象包含了从0到9999的所有整数值，当服务器需要用到值为0到9999字符串对象时，服务器就会使用这些共享对象，而不是新创建对象。

![image-20211209160454356](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209160455.png)

![image-20211209161122176](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209161123.png)

![image-20211209161220808](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209161222.png)

# 第二部分 单机数据库的实现

## 第九章 数据库

![image-20211209162048052](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209162049.png)

![image-20211209162128506](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209162130.png)

切换数据库

select + 数字

![image-20211209162608558](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209162610.png)

![image-20211209162728589](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209162730.png)

![image-20211209163214101](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209163219.png)

![image-20211209163637910](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209163639.png)

![image-20211209164523434](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209164525.png)

数据库的键空间是字典

用于清空整个数据库的FLUSHDB命令，就是通过删除键空间中的所有键值对来实现的，RANDOMKEY命令，就是通过在键空间中随机返回一个键来实现的。

DBSIZE命令返回键空间中包含的键值对的数量来实现的，类似的命令还有EXISTS，RENAME，KEYS等。

![image-20211209170507155](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209170508.png)

![image-20211209170520740](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209170522.png)

![image-20211209171054999](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209171056.png)

![image-20211209171136522](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209171137.png)

![image-20211209171323630](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209171325.png)

![image-20211209171622768](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209171624.png)

![image-20211209171816877](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209171818.png)

![image-20211209171910515](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209171912.png)

![image-20211209172436526](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209172501.png)

![image-20211209172456450](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209172457.png)

![image-20211209174134344](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209174135.png)

![image-20211209174147868](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209174149.png)

![image-20211209174334327](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209174335.png)

![image-20211210085714874](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210085717.png)

![image-20211209174636795](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209174638.png)

![image-20211209175311768](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209175313.png)

**redis实际上使用的是惰性删除和定期删除两种策略**

过期的惰性删除策略由db.c/expireIfNeeded函数实现

**过期的定期删除策略由redis.c/activeExpireCycle函数实现，每当Redis的服务器周期性操作redis.c/serverCron函数执行时，activeExpireCycle函数就会被调用，它在规定的时间内，分多次遍历服务器中的各个数据库，从数据库的expires字典中随机检查一部分键的过期时间，并删除其中的过期键。**

![image-20211209181203225](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209181204.png)

**AOF，RDB和复制功能对过期键的处理**

![image-20211210085844277](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210085845.png)

![image-20211210090144578](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210090146.png)

![image-20211210090213831](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210090215.png)

![image-20211210090359185](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210090402.png)

![image-20211210092127992](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210092129.png)

![image-20211210092425904](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210092427.png)

![image-20211210092540618](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210092541.png)

![image-20211210092630056](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210092631.png)

![image-20211210092745146](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210092746.png)

![image-20211210093112018](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210093113.png)

![image-20211210093501019](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210093502.png)

![image-20211210093623384](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210093624.png)

![image-20211210094117596](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210094119.png)

![image-20211210102919761](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210102921.png)

![image-20211210103115140](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210103117.png)

![image-20211210102745573](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210102748.png)

![image-20211210103347567](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210103349.png)

## 第十章 RDB持久化

![image-20211209182529181](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209182531.png)

Redis是内存数据库，它将自己的数据库状态存储在内存里面。

![image-20211209182649916](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209182651.png)

![image-20211209183033598](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209183035.png)

![image-20211209183051906](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209183053.png)

![image-20211209183329734](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209183331.png)

![image-20211209183607470](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209183609.png)

**save 会阻塞 而bgsave会fork出子进程**

![image-20211209183744887](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209183746.png)

自动间隔性保存——向服务器提供以下配置

save 900 1

save 300 10

save 60 10000

![image-20211209190754684](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209190758.png)

![image-20211209190816248](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209190817.png)

![image-20211209191814227](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209191815.png)

![image-20211209191829720](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209191831.png)

![image-20211209192046006](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209192048.png)

![image-20211209192233830](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209192235.png)

![image-20211209192542380](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209192543.png)

![image-20211209192523845](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209192525.png)

![image-20211209193002367](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209193003.png)

![image-20211209193949321](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209193952.png)

![image-20211209194013021](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209194014.png)

![image-20211209194028129](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209194029.png)



![image-20211209194135503](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209194137.png)

![image-20211209194439900](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209194441.png)

![image-20211209194559651](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209194600.png)

![image-20211209195424223](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209195425.png)

![image-20211209195512019](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209195513.png)



![image-20211209195522617](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209195523.png)

![image-20211209195628034](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209195629.png)

![image-20211209195857951](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209195859.png)

![image-20211209200110921](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209200112.png)

![image-20211209200134361](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209200136.png)

![image-20211209200334804](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209200336.png)

![image-20211209201417378](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209201419.png)

![image-20211209202216197](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209202218.png)

![image-20211209202420306](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209202422.png)

![image-20211209202630267](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209202632.png)

od命令：分析RDB文件

![image-20211209202817141](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209202819.png)

![image-20211209202910020](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209202911.png)

![image-20211209203249849](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209203251.png)

![image-20211209203312340](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209203314.png)

![image-20211209203509457](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209203510.png)

```
SADD LANG "C" "JAVA" " RUBY"
od -c dump.rdb
```

![image-20211209204017415](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209204019.png)

**RDB文件是一个经过压缩的二进制文件，由多个部分组成。**

**对于不同的类型的键值对，RDB文件会使用不同的方式来保存它们。**

## 第十一章 AOF持久化

![image-20211209204905978](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209205145.png)

![image-20211209205131425](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209205133.png)

![image-20211209205344887](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209205346.png)

![image-20211209210948036](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209210949.png)

![image-20211209205538594](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209205540.png)

![image-20211209211052409](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209211054.png)

![image-20211209211226363](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209211227.png)

![image-20211209212543719](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209212545.png)

![image-20211209213043058](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209213045.png)

![image-20211209213243239](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209213244.png)

![image-20211209213418084](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209213419.png)

![image-20211209213539294](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209213540.png)

![image-20211209214151543](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209214153.png)

![image-20211209214445737](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209214447.png)

![image-20211209214758411](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209214759.png)

![image-20211209214856335](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209214858.png)

![image-20211209215045707](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211209215048.png)

![image-20211210084721206](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210084723.png)

**以上就是AOF后台重写，也即是BGREWRITEAOF命令的实现原理**

![image-20211210084854065](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210084855.png)

## 第十二章 事件



![image-20211210104324527](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210104325.png)

![image-20211210104948292](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210104949.png)

![image-20211210110016462](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210110017.png)

![image-20211210110150242](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210110152.png)

![image-20211210110744452](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210110746.png)

![image-20211210110904536](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210110905.png)

![image-20211210111726231](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210111727.png)



## 第十三章 客户端

![image-20211210124521208](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210124523.png)

![image-20211210124639220](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210124649.png)

![image-20211210124740999](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210124744.png)

![image-20211210125015172](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210125016.png)

![image-20211210125027940](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210125029.png)

![image-20211210125503954](C:/Users/86158/AppData/Roaming/Typora/typora-user-images/image-20211210125503954.png)

![image-20211210125644583](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210125646.png)

![image-20211210125707017](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210125708.png)

![image-20211210133142971](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210133144.png)

![image-20211210133735041](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210133736.png)

PUBSUB命令和SCRIPT LOAD命令有副作用，会被写到AOF文件中。

![image-20211210134014593](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210134016.png)

![image-20211210134038957](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210134041.png)

![image-20211210134055357](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210134056.png)

**输入缓冲最大不能大于1GB，否则服务器将关闭这个客户端**

![image-20211210134223161](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210134224.png)

![image-20211210135051564](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210135052.png)

![image-20211210134258443](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210134259.png)

![image-20211210135104211](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210135105.png)

![image-20211210135237387](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210135238.png)

![image-20211210135555716](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210135557.png)

![image-20211210135534034](C:/Users/86158/AppData/Roaming/Typora/typora-user-images/image-20211210135534034.png)

![image-20211210135708633](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210135710.png)

![image-20211210135804653](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210135806.png)

![image-20211210135819155](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210135820.png)

![image-20211210135850076](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210135851.png)

![image-20211210140121091](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210140122.png)

![image-20211210140419548](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210140420.png)

![image-20211210140602303](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210140603.png)

![image-20211210140907904](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210140909.png)

![image-20211210141227584](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210141228.png)

![image-20211210141345883](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210141347.png)



![image-20211210141410341](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210141411.png)

![image-20211210141427133](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210141428.png)

![image-20211210142107605](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210142108.png)

## 第十四章 服务器

![image-20211210144003198](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210144004.png)

![image-20211210144247782](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210144253.png)

![image-20211210144311936](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210144313.png)



![image-20211210150258339](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210150259.png)

![image-20211210150216002](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210150217.png)

![image-20211210150852174](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210150853.png)

![image-20211210151718053](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210151719.png)

![image-20211210151737257](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210151738.png)

![image-20211210152118960](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210152120.png)

**一些特殊情况要注意（一堆预备操作，可怕）**

![image-20211210152537481](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210152538.png)

![image-20211210153418236](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210153420.png)

![image-20211210153602813](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210153604.png)

![image-20211210153817192](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210153818.png)

![image-20211210154033813](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210154035.png)

![image-20211210154121704](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210154122.png)

![image-20211210175301841](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210175303.png)

![image-20211210175919270](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210175920.png)

![image-20211210180048321](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210180049.png)

![image-20211210180100740](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210180102.png)

![image-20211210180435529](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210180438.png)

![image-20211210181424235](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210181425.png)

![image-20211210181523221](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210181524.png)

![image-20211210183434147](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210183435.png)

![image-20211210183546502](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210183547.png)

![image-20211210183808505](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210183809.png)

![image-20211210191305823](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210191307.png)

![image-20211210191428758](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211210191430.png)
