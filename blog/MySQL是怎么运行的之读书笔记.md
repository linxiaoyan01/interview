---
title: MySQL是怎么运行的之读书笔记
categories: MySQL
tags: [MySQL]
---

# 写在前面



nice~，学到蛮多的，搜罗了一堆网课还不如直接看书系统地过一遍知识点呢

# 装作自己是个小白 —— 重新认识MySQL

每一个运行着的程序也被称为一个`进程`。我们的`MySQL`服务器程序和客户端程序本质上都算是计算机上的一个`进程`，这个代表着`MySQL`服务器程序的进程也被称为`MySQL数据库实例`，简称`数据库实例`。

![image-20211204003107231](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211204003108.png)

SHOW ENGINES;

其中的Support列表示该存储引擎是否可⽤，DEFAULT值代表是当 前服务器程序的默认存储引擎。Comment列是对存储引擎的⼀个描 述，。Transactions列代表该存储引擎是否⽀持事务处理。**XA列代表着该存储引擎是否⽀持分布式事 务。Savepoints代表着该列是否⽀持部分事务回滚。**

# MySQL的调控按钮 —— 启动选项和系统变量

如果我 们在多个配置⽂件中设置了相同的启动选项，那以最后⼀个配置⽂件中的为准。

对于启动选项 来说，如果启动选项名由多个单词组成，各个单词之间⽤短划线-或 者下划线_连接起来都可以，但是对应的系统变量之间必须使⽤下划线_连接起来。

对于⼤部分系统变量来说，它们的值可以在服务器程序运⾏过程中进⾏动态修改⽽⽆需停⽌并重启服务器。

通过启动选项设置的系统变量的作⽤范围都是GLOBAL的， 也就是对所有客户端都有效的。

如果在设置系统变量的语句中省略了作 ⽤范围，默认的作⽤范围就是SESSION。

并不是所有系统变量都具有GLOBAL和SESSION的作⽤范围。

有些系统变量是只读的，并不能设置值。

由于状态变量是⽤来显示服务器程序运⾏状况的，所以它们的值只能 由服务器程序⾃⼰来设置，我们程序员是不能设置的。

# 乱码的前世今生 —— 字符集和比较规则

每种字符集对应若⼲种⽐较规则，每种字符集都有⼀种默认的⽐较规则。

MySQL有4个级别的字符集和⽐较规则，分别是：

服务器级别 数据库级别 表级别 列级别。

character_set_database 和 collation_database 这两个系统变 量是只读的，我们不能通过修改这两个变量的值⽽改变当前数据库的 字符集和⽐较规则。

同⼀个表中的不同的列也可以 有不同的字符集和⽐较规则。

只修改字符集，则⽐较规则将变为修改后的字符集默认的⽐较 规则。 只修改⽐较规则，则字符集将变为修改后的⽐较规则对应的字 符集。

不论哪个级别的字符集和⽐较规则，这两条规则都适⽤

![image-20211129210203569](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129210207.png)

从发送请求到返回结果这个过程中伴随着多 次字符集的转换，在这个过程中会⽤到3个系统变量，我们先把它们 写出来看⼀下：

![image-20211129210259097](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129210301.png)

假设你的客户端采⽤的字符集和 character_set_client 不⼀ 样的话，这就会出现意想不到的情况。

假设你的客户端采⽤的字符集和 character_set_results 不⼀ 样的话，这就会出现客户端⽆法解码结果集的情况。

character_set_connection只是服务器在处理请求时使⽤ 的字符集，它是什么其实没多重要，但是⼀定要注意，该字符 集包含的字符范围⼀定涵盖请求以及结果集中的字符，要不然 会出现⽆法将请求中的字符编码 成character_set_connection字符集或者⽆法编码结果集 中的字符。

我们通常都把 character_set_client 、character_set_connection、character_set_results 这三个 系统变量设置成和客户端使⽤的字符集⼀致的情况，这样减少了很多 ⽆谓的字符集转换。

总结

1. 字符集的是某个字符范围的编码规则。

2. ⽐较规则是针对某个字符集中的字符⽐较⼤⼩的⼀种规则。

3. 在MySQL中，⼀个字符集可以有若⼲种⽐较规则，其中有⼀个 默认的⽐较规则，⼀个⽐较规则必须对应⼀个字符集。

4. 查看MySQL中查看⽀持的字符集和⽐较规则的语句如下：

   SHOW (CHARACTER SET|CHARSET) [LIKE 匹配的模式];

   SHOW COLLATION [LIKE 匹配的模式];

   MySQL有四个级别的字符集和⽐较规则

   **服务器级别** character_set_server表示服务器级别的字符集，collation_server表示服务器级别的⽐较规则。

   **数据库级别** 创建和修改数据库时可以指定字符集和⽐较规则：

   CREATE DATABASE 数据库名 [[DEFAULT] CHARACTER SET 字符集名称] [[DEFAULT] COLLATE ⽐较规则名称]; ALTER DATABASE 数据库名 [[DEFAULT] CHARACTER SET 字符集名称] [[DEFAULT] COLLATE ⽐较规则名称]; character_set_database表示当前数据库的字符集，collation_database表示当前默认数据库的⽐较规则，这两个系统变量是只读的，不能修改。如果没有指定当前 默认数据库，则变量与相应的服务器级系统变量具有相同的值。

   **表级别** 创建和修改表的时候指定表的字符集和⽐较规则：

   CREATE TABLE 表名 (列的信息) [[DEFAULT] CHARACTER SET 字符集名称] [COLLATE ⽐较规则名称]]

   ALTER TABLE 表名 [[DEFAULT] CHARACTER SET 字符集名称] [COLLATE ⽐较规则名称]

   **列级别** 创建和修改列定义的时候可以指定该列的字符集和⽐较规则：

   CREATE TABLE 表名( 列名 字符串类型 [CHARACTER SET 字符集名称] [COLLATE ⽐较规则名称], 其他列… ); ALTER TABLE 表名 MODIFY 列名 字符串类型 [CHARACTER SET 字符集名称] [COLLATE ⽐较规则名 称];

   从发送请求到接收结果过程中发⽣的字符集转换： 客户端使⽤操作系统的字符集编码请求字符串 服务器将客户端发送来的字符串的字符集按 照chacharacter_set_client转换 为character_set_connection。 使⽤character_set_connection进⾏服务器操作。 将结果集字符串的字符集从 character_set_connection转 为character_set_results发送到客户端 客户端使⽤操作系统的字符集解析收到的结果集字符串。

   在这个过程中各个系统变量的含义如下： 系统变量描述 character_set_client 服务器解码请求时使⽤的字符集； character_set_connection 服务器运⾏过程中使⽤的字符集； character_set_results 服务器向客户端返回数据时使⽤的字符集。⼀般情况下要使⽤保持这三个变量的值和客户端使⽤的字符集相同。

   ⽐较规则的作⽤通常体现⽐较字符串⼤⼩的表达式以及对某个字符串列进⾏排序中。

# 从一条记录说起—— InnoDB 记录结构

将数据划 分为若⼲个⻚，以⻚作为磁盘和内存之间交互的基本单位，InnoDB 中⻚的⼤⼩⼀般为 16 KB

我们平时是以记录为单位来向表中插⼊数据的，这些记录在磁盘上的 存放⽅式也被称为⾏格式或者记录格式。设计InnoDB存储引擎的⼤ 叔们到现在为⽌设计了4种不同类型的⾏格式，分别 是Compact、Redundant、Dynamic和Compressed⾏格式

![image-20211129212219555](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129212221.png)

服务器为了描述这条记录⽽不得不额外添加的⼀些信 息，这些额外信息分为3类，分别是变⻓字段⻓度列表、NULL值列表 和记录头信息

各变⻓ 字段数据占⽤的字节数按照列的顺序**逆序**存放

```
mysql> CREATE TABLE record_format_demo (
-> c1 VARCHAR(10),
-> c2 VARCHAR(10) NOT NULL,
-> c3 CHAR(10),
-> c4 VARCHAR(10)
-> ) CHARSET=ascii ROW_FORMAT=COMPACT;
Query OK, 0 rows affected (0.03 sec)


mysql> INSERT INTO record_format_demo(c1, c2, c3,
c4) VALUES('aaaa','bbb','cc','d'), ('eeee','fff', NULL, NULL);
Query OK, 2 rows affected (0.02 sec)
Records: 2 Duplicates: 0 Warnings: 0

mysql> SELECT * FROM record_format_demo;
+------+-----+------+------+
| c1 | c2 | c3 | c4 |
+------+-----+------+------+
| aaaa | bbb | cc | d |
| eeee | fff | NULL | NULL |
+------+-----+------+------+
2 rows in set (0.00 sec)
```

![image-20211129212336102](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129212337.png)

![image-20211129212350353](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129212351.png)

InnoDB有它的⼀套 规则，我们⾸先声明⼀下W、M和L的意思

1. 假设某个字符集中表示⼀个字符最多需要使⽤的字节数为W， 也就是使⽤SHOW CHARSET语句的结果中的Maxlen列，⽐⽅ 说utf8字符集中的W就是3，gbk字符集中的W就是2，ascii 字符集中的W就是1。
2. 对于变⻓类型VARCHAR(M)来说，这种类型表示能存储最多M 个字符（注意是字符不是字节），所以这个类型能表示的字符 串最多占⽤的字节数就是M×W。
3. 假设它实际存储的字符串占⽤的字节数是L。

所以确定使⽤1个字节还是2个字节表示真正字符串占⽤的字节数的 规则就是这样

总结⼀下就是说：如果该可变字段允许存储的最⼤字节数（M×W）超 过255字节并且真实存储的字节数（L）超过127字节，则使⽤2个字 节，否则使⽤1个字节。

变⻓字段⻓度列表中只存储值为 ⾮NULL 的列内容占⽤的⻓度，值为 NULL 的列的⻓度是不储存的

如果表中没有允许存储 NULL 的列，则 NULL值列表 也不存在 了，否则将每个允许存储NULL的列对应⼀个⼆进制位，⼆进制 位按照列的顺序逆序排列。

⼆进制位表示的意义如下： ⼆进制位的值为1时，代表该列的值为NULL。 ⼆进制位的值为0时，代表该列的值不为NULL。

![image-20211129213331998](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129213333.png)

MySQL规定NULL值列表必须⽤整数个字节的位表示

![image-20211129213408012](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205141448.png)

第一条记录⽤⼗六进制表示就是：0x00

![image-20211129213451149](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129213453.png)

第二条记录⽤⼗六进制表示就是：0x06

![image-20211129213526793](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129213528.png)

![image-20211129213616100](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129213641.png)

![image-20211129213628078](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129213631.png)

![image-20211129213718802](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130095032.png)

对于record_format_demo表来说，记录的真实数据除了 c1、c2、c3、c4这⼏个我们⾃⼰定义的列的数据以外，MySQL会为 每个记录默认的添加⼀些列（也称为隐藏列），具体的列如下：

![image-20211129213743644](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129213745.png)

InnoDB存储引擎会为每条记录都添加 transaction_id 和 roll_pointer 这两个列，但是 row_id 是可选的（在没有⾃定义主 键以及Unique键的情况下才会添加该列）。

![image-20211129213815126](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129213816.png)

注意第1条记录中c3列的值，它是CHAR(10)类型的，它实际 存储的字符串是：’cc’，⽽ascii字符集中的字节表示 是’0x6363’，虽然表示这个字符串只占⽤了2个字节，但整 个c3列仍然占⽤了10个字节的空间，除真实数据以外的8个字 节的统统都⽤空格字符填充，空格字符在ascii字符集的表示 就是0x20

![image-20211129214046271](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129214047.png)

修改⼀下record_format_demo表的字符集

```
mysql> ALTER TABLE record_format_demo MODIFY
COLUMN c3 CHAR(10) CHARACTER SET utf8;
Query OK, 2 rows affected (0.02 sec)
Records: 2 Duplicates: 0 Warnings: 0
```

![image-20211129214140296](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129214142.png)

对于 CHAR(M) 类型的列来说，当列采⽤的是定⻓字 符集时，该列占⽤的字节数不会被加到变⻓字段⻓度列表，⽽如果采 ⽤变⻓字符集时，该列占⽤的字节数也会被加到变⻓字段⻓度列表

⽐⽅说对于使 ⽤utf8字符集的CHAR(10)的列来说，该列存储的数据字节⻓度的 范围是10～30个字节。即使我们向该列中存储⼀个空字符串也会占 ⽤10个字节，防止碎片

![image-20211129214439826](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129214441.png)

![image-20211129214502068](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129214504.png)

与Compact⾏格式的记录头信息对⽐来看，有两处不同： Redundant⾏格式多了n_field和1byte_offs_flag 这两个属性。 Redundant⾏格式没有record_type这个属性。

如果该存储NULL值的字段是CHAR(M)数据类型的，则将 占⽤记录的真实数据部分，并把该字段对应的数据使 ⽤0x00字节填充。

在Redundant⾏格 式中⼗分⼲脆，不管该列使⽤的字符集是啥，只要是使⽤CHAR(M) 类型，占⽤的真实数据空间就是该字符集表示⼀个字符最多需要的字节数和M的乘积。⽐⽅说使⽤utf8字符集的CHAM(10)类型的列占⽤的真实数据空间始终为30个字节，使⽤gbk字符集的CHAM(10)类型 的列占⽤的真实数据空间始终为20个字节。由此可以看出来，使 ⽤Redundant⾏格式的CHAR(M)类型的列是不会产⽣碎⽚的。

我们为了存储⼀ 个VARCHAR(M)类型的列，其实需要占⽤3部分存储空间：

1. 真实数据
2. 真实数据占⽤字节的⻓度
3. NULL值标识，如果该列有NOT NULL属性则可以没有这部分存 储空间

在列的值允许为NULL的情况下，gbk字符集表示⼀ 个字符最多需要2个字符，那在该字符集下，M的最⼤取值就 是32766（也就是：65532/2），也就是说最多能存储32766个字 符；utf8字符集表示⼀个字符最多需要3个字符，那在该字符集 下，M的最⼤取值就是21844，就是说最多能存储21844（也就是： 65532/3）个字符。⼀定要记住⼀个⾏中的所有列（不 包括隐藏列和记录头信息）占⽤的字节⻓度加起来不能超过65535 个字节

**记录中的数据太多产⽣的溢出**

![image-20211129215445992](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129215447.png)

![image-20211129215623861](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129215625.png)

MySQL中规定⼀个⻚中⾄少存放两⾏记录

**Dynamic和Compressed⾏格式**

把所有的字节都存储到其他⻚⾯中，只在记录的真实数据处 存储其他⻚⾯的地址

![image-20211129215819101](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129215822.png)

Compressed⾏格式和Dynamic不同的⼀点是，Compressed⾏格 式会采⽤压缩算法对⻚⾯进⾏压缩，以节省空间。

CHAR(M)中的M值过⼤的情况：最⼤字节⻓度超过768字节，那么不论我们使⽤的是上述4种的哪种⾏格式，InnoDB都会把该列当 成变⻓字段看待。⽐⽅说采⽤utf8mb4的CHAR(255)类型的列将会 被当作变⻓字段看待，因为4×255 > 768。

总结

1. ⻚是MySQL中磁盘和内存交互的基本单位，也是MySQL是管理 存储空间的基本单位。

2. 指定和修改⾏格式的语法如下：

   CREATE TABLE 表名 (列的信息) ROW_FORMAT=⾏格式名称 ALTER TABLE 表名 ROW_FORMAT=⾏格式名称

3. ⼀个⻚⼀般是16KB，当记录中的数据太多，当前⻚放不下的时 候，会把多余的数据存储到其他⻚中，这种现象称为⾏溢出

# 盛放记录的大盒子 —— InnoDB 数据页结构

![image-20211129174450680](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129174603.png)

![image-20211129174624487](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129174627.png)

```
mysql> CREATE TABLE page_demo(
-> c1 INT,
-> c2 INT,
-> c3 VARCHAR(10000),
-> PRIMARY KEY (c1)
-> ) CHARSET=ascii ROW_FORMAT=Compact;
Query OK, 0 rows affected (0.03 sec)
```

![image-20211129174703087](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129174812.png)

![image-20211129174855647](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129174919.png)

![image-20211129174930127](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129174932.png)

![image-20211129174943101](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129174945.png)

![image-20211129175023590](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129175026.png)

![image-20211129175045823](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129175048.png)

![image-20211129180611137](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129180623.png)

![image-20211129181121849](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129181339.png)

![image-20211129181414188](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129181416.png)

**Page Header（⻚⾯头部）**

![image-20211129201308854](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129201444.png)

![image-20211129201419191](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129201446.png)

**File Header（⽂件头部）**

![image-20211129201809205](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129201832.png)

![image-20211129201829056](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129201837.png)

**页的类型**

![image-20211129202031292](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129202033.png)

**我们存放记录的数据⻚的类型其实是FIL_PAGE_INDEX，也就是所谓的索引⻚。**并不是所有类型的⻚都有上⼀个和下⼀个⻚的属性

![image-20211129202756365](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129202759.png)

**File Trailer：⽤于检验⻚是否完整的部分，占⽤固 定的8个字节。前4个字节代表⻚的校验和，后4个字节代表⻚⾯被最后修改时对应的⽇志序列位置（LSN）**

**总结**

1. InnoDB为了不同的⽬的⽽设计了不同类型的⻚，我们把⽤于存 放记录的⻚叫做数据⻚。

2. ⼀个数据⻚可以被⼤致划分为7个部分，分别是 File Header，表示⻚的⼀些通⽤信息，占固定的38字 节。

   Page Header，表示数⻚专有的⼀些信息，占固定的 56个字节。

   Infimum + Supremum，两个虚拟的伪记录，分别表示 ⻚中的最⼩和最⼤记录，占固定的26个字节。

   User Records：真实存储我们插⼊的记录的部分，⼤⼩不固定。 Free Space：⻚中尚未使⽤的部分，⼤⼩不确定。

   Page Directory：⻚中的某些记录相对位置，也就是 各个槽在⻚⾯中的地址偏移量，⼤⼩不固定，插⼊的记录越多，这个部分占⽤的空间越多。

   File Trailer：⽤于检验⻚是否完整的部分，占⽤固 定的8个字节。

3. 每个记录的头信息中都有⼀个next_record属性，从⽽使⻚中的所有记录串联成⼀个单链表。

4. InnoDB会为把⻚中的记录划分为若⼲个组，每个组的最后⼀ 个记录的地址偏移量作为⼀个槽，存放在Page Directory 中，所以在⼀个⻚中根据主键查找记录是⾮常快的，分为两 步： 通过⼆分法确定该记录所在的槽。 通过记录的next_record属性遍历该槽所在的组中的各个 记录。

5. 每个数据⻚的File Header部分都有上⼀个和下⼀个⻚的编 号，所以所有的数据⻚会组成⼀个双链表。

6. 为保证从内存中同步到磁盘的⻚的完整性，在⻚的⾸部和尾部 都会存储⻚中数据的校验和和⻚⾯最后修改时对应的LSN（log sequence number）值， 如果⾸部和尾部的校验和和LSN值校验不成功的话，就说明同步过程出现了问题

# 快速查询的秘籍 —— B+ 树索引

![image-20211129223229698](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129223231.png)

**在⼀个⻚中的查找**

假设⽬前表中的记录⽐较少，所有的记录都可以被存放到⼀个⻚中， 在查找记录的时候可以根据搜索条件的不同分为两种情况：

1. 以主键为搜索条件 这个查找过程我们已经很熟悉了，可以在⻚⽬录中使⽤⼆分法 快速定位到对应的槽，然后再遍历该槽对应分组中的记录即可 快速找到指定的记录。
2. 以其他列作为搜索条件 对⾮主键列的查找的过程可就不这么幸运了，因为在数据⻚中 并没有对⾮主键列建⽴所谓的⻚⽬录，所以我们⽆法通过⼆分 法快速定位相应的槽。这种情况下只能从最⼩记录开始依次遍 历单链表中的每条记录，然后对⽐每条记录是不是符合搜索条 件。很显然，这种查找的效率是⾮常低的。

**在很多⻚中查找**

⼤部分情况下我们表中存放的记录都是⾮常多的，需要好多的数据⻚来存储这些记录。在很多⻚中查找记录的话可以分为两个步骤： 1. 定位到记录所在的⻚。2. 从所在的⻚内中查找相应的记录。

在没有索引的情况下，不论是根据主键列或者其他列的值进⾏查找， 由于我们并不能快速的定位到记录所在的⻚，所以只能从第⼀个⻚沿着双向链表⼀直往下找，在每⼀个⻚中根据我们刚刚唠叨过的查找⽅式去查找指定的记录。

```
mysql> CREATE TABLE index_demo(
-> c1 INT,
-> c2 INT,
-> c3 CHAR(1),
-> PRIMARY KEY(c1)
-> ) ROW_FORMAT = Compact;
Query OK, 0 rows affected (0.03 sec)
```

![image-20211129224059476](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129224101.png)

![image-20211129224248812](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129224250.png)

![image-20211130001551672](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130081343.png)

因为各个⻚中的记录并没有规律，我们并不知道我们的搜索条件匹配哪些⻚中的记录，所以 不得不依次遍历所有的数据 ⻚

我们也可以想办法为快速定位记录所在的数据⻚⽽建 ⽴⼀个别的⽬录，建这个⽬录必须完成下边这些事⼉：

**下⼀个数据⻚中⽤户记录的主键值必须⼤于上⼀个⻚中⽤户记 录的主键值。**

![image-20211130003306391](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130081340.png)

新分配的数据⻚编号可能并不是连续的，也就是说我们使⽤的 这些⻚在存储空间⾥可能并不挨着

不符合下⼀个数据⻚中⽤户记录的主键值 必须⼤于上⼀个⻚中⽤户记录的主键值的要求，所以在插⼊主 键值为4的记录的时候需要伴随着⼀次记录移动，也就是把主 键值为5的记录移动到⻚28中，然后再把主键值为4的记录插⼊ 到⻚10中。这个过程我们也可以称为⻚分裂

![image-20211130003819180](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130081335.png)

**给所有的⻚建⽴⼀个⽬录项。**

数据⻚的编号可能并不是连续的

![image-20211130004329748](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130081333.png)

我们需要 给它们做个⽬录，每个⻚对应⼀个⽬录项，每个⽬录项包括下边两个部分。这个⽬录 有⼀个别名，称为索引。

⻚的⽤户记录中最⼩的主键值，我们⽤key来表示。 ⻚号，我们⽤page_no表示。

![image-20211130004416487](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130081326.png)

我们只需要把⼏个⽬录项在物理存储器上连续存储，⽐如把他们放到⼀个数组⾥，就可以实现根据主键值快速查找某条记录的功能了。

我们想找主键值为20的记录，具体查找过程分两步： 1. 先从⽬录项中根据⼆分法快速确定出主键值为20的记录 在⽬录项3中（因为 12 < 20 < 209），它对应的⻚是 ⻚9。 2. 再根据前边说的在⻚中查找记录的⽅式去⻚9中定位具体 的记录。

忽然发现这些⽬录项其实⻓得跟我们的⽤户记录 差不多，只不过⽬录项中的两个列是主键和⻚号⽽已，所以他们复⽤ 了之前存储⽤户记录的数据⻚来存储⽬录项，为了和⽤户记录做⼀下 区分，我们把这些⽤来表示⽬录项的记录称为⽬录项记录

record_type属性

0：普通的⽤户记录 1：⽬录项记录 2：最⼩记录 3：最⼤记录

把前边 使⽤到的⽬录项放到数据⻚中的样⼦：

![image-20211130005427867](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130081330.png)

⽬录项记录只有主键值和⻚的编号两个列，⽽普通的⽤户记录 的列是⽤户⾃⼰定义的，可能包含很多列，另外还有InnoDB ⾃⼰添加的隐藏列。

还记得我们之前在唠叨记录头信息的时候说过⼀个叫 min_rec_mask的属性么，只有在存储⽬录项记录的⻚中的主 键值最⼩的⽬录项记录的min_rec_mask值为1，其他别的记 录的min_rec_mask值都是0

![image-20211130081309564](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130081921.png)

1. 确定⽬录项记录⻚

   我们现在的存储⽬录项记录的⻚有两个，即⻚30和⻚32，⼜因 为⻚30表示的⽬录项的主键值的范围是[1, 320)，⻚32表示 的⽬录项的主键值不⼩于320，所以主键值为20的记录对应的 ⽬录项记录在⻚30中

2. 通过⽬录项记录⻚确定⽤户记录真实所在的⻚

3. 在真实存储⽤户记录的⻚中定位到具体的记录。

![image-20211130081915167](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130081924.png)

不论是存放⽤户记录的数据⻚，还是存放⽬录项记录的数据⻚，我们 都把它们存放到B+树这个数据结构中了，所以我们也称这些数据⻚ 为节点。实际⽤户记录其实都存放在 B+树的最底层的节点上。存放我 们⽤户记录的那层为第0层，之后依次往上加。

你的表⾥能存放100000000000条记录么？所以⼀般情况下，我们 ⽤到的B+树都不会超过4层，那我们通过主键值去查找某条记录最多 只需要做4个⻚⾯内的查找（查找3个⽬录项⻚和⼀个⽤户记录 ⻚），⼜因为在每个⻚⾯内有所谓的Page Directory（⻚⽬ 录），所以在⻚⾯内也可以通过⼆分法实现快速定位记录

⻚内的记录是按照主键的⼤⼩顺序排成⼀个单向链表。 各个存放⽤户记录的⻚也是根据⻚中⽤户记录的主键⼤⼩ 顺序排成⼀个双向链表。 存放⽬录项记录的⻚分为不同的层次，在同⼀层次中的⻚也是根据⻚中⽬录项记录的主键⼤⼩顺序排成⼀个双向链表。B+树的叶⼦节点存储的是完整的⽤户记录，指这个记录中存储了所有列的值 （包括隐藏列）。

**我们把具有这两种特性（上面那段话）的B+树称为聚簇索引，聚簇索引就是数据的存储⽅式，**（所有的⽤户记录都存储在了叶⼦节点），也就是所谓的索引即数据，数据即索引。

**二级索引**

⽤c2列的⼤⼩作为数据⻚、⻚中记录的排序规则，再建⼀棵B+树

![image-20211130083221852](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130083301.png)

B+树的叶⼦节点存储的并不是完整的⽤户记录，⽽只是c2列 +主键这两个列的值。

⽬录项记录中不再是主键+⻚号的搭配，⽽变成了c2列+⻚号的 搭配。

1. 确定⽬录项记录⻚

   根据根⻚⾯，也就是⻚44，可以快速定位到⽬录项记录所在的 ⻚为⻚42（因为2 < 4 < 9）。

2. 通过⽬录项记录⻚确定⽤户记录真实所在的⻚。

   在⻚42中可以快速定位到实际存储⽤户记录的⻚，但是由于c2 列并没有唯⼀性约束，所以c2列值为4的记录可能分布在多个 数据⻚中，⼜因为2 < 4 ≤ 4，所以确定实际存储⽤户记录的 ⻚在⻚34和⻚35中。

3. 在真实存储⽤户记录的⻚中定位到具体的记录。

   到⻚34和⻚35中定位到具体的记录。

4. 但是这个B+树的叶⼦节点中的记录只存储了c2和c1（也就是 主键）两个列，所以我们必须再根据主键值去聚簇索引中再查 找⼀遍完整的⽤户记录。

到聚簇索引中再查⼀遍， 这个过程也被称为回表。也就是根据c2列的值查询⼀条完整的⽤户 记录需要使⽤到2棵B+树！！！

因为 这种按照⾮主键列建⽴的B+树需要⼀次回表操作才可以定位到完整 的⽤户记录，所以这种B+树也被称为⼆级索引（英⽂名secondary index），或者辅助索引。由于我们使⽤的是c2列的⼤⼩作为B+树 的排序规则，所以我们也称这个B+树为为c2列建⽴的索引。

**联合索引**

![image-20211130084642819](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130095146.png)

每条⽬录项记录都由c2、c3、⻚号这三个部分组成，各条记录 先按照c2列的值进⾏排序，如果记录的c2列相同，则按照c3 列的值进⾏排序

B+树叶⼦节点处的⽤户记录由c2、c3和主键c1列组成，以c2和c3列的⼤⼩为排序规则建⽴的B+树称为联 合索引，它的意思与分别为c2和c3列分别建⽴索引的表述是不同的，建⽴联合索引只会建⽴如上图⼀样的1棵B+树，为c2和c3列分别建⽴索引会分别以c2和c3列的⼤⼩为排序规 则建⽴2棵B+树

⼀个B+树索引的根节点⾃诞⽣之 ⽇起，便不会再移动

这个存储某个索引的根节点在哪个⻚⾯中的信息 就是传说中的数据字典中的⼀项信息

根节点的⻚号便会被记录到某个地⽅，凡是InnoDB存储引擎 需要⽤到这个索引的时候，都会从那个固定的地⽅取出根节点的⻚ 号，从⽽来访问这个索引

![image-20211130085414402](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130085417.png)

如果⼆级索引中⽬录项记录的内容只是索引列 + ⻚号的搭配的话， 那么为c2列建⽴索引后的B+树应该⻓这样：

![image-20211130085431304](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130090328.png)

也就是我们把主键值也添加到⼆级索引内节点中的⽬录项记录了，这 样就能保证B+树每⼀层节点中各条⽬录项记录除⻚号这个字段外是 唯⼀的，所以我们为c2列建⽴⼆级索引后的示意图实际上应该是这 样⼦的

![image-20211130090034781](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130090324.png)

为了让新插⼊记录能找到⾃⼰在那个⻚⾥，我们需要保证在B+树的 同⼀层内节点的⽬录项记录除⻚号这个字段以外是唯⼀的。所以对于 ⼆级索引的内节点的⽬录项记录的内容实际上是由三个部分构成的： 索引列的值、主键值、⻚号。

插⼊记录(9, 1, ‘c’)时，如果c2列的值相同的 话，可以接着⽐较主键值

⼀个B+树只需要很少的层级就可以轻松存储数亿条记录

⼀个⻚⾯最少存储2条记录

MyISAM的索引⽅案虽然也使⽤树形结 构，但是却将索引和数据分开存储：

将表中的记录按照记录的插⼊顺序单独存储在⼀个⽂件中，称 之为数据⽂件。

这个⽂件并不划分为若⼲个数据⻚，有多少记 录就往这个⽂件中塞多少记录就成了。我们可以通过⾏号⽽快 速访问到⼀条记录。

![image-20211130090914706](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130094640.png)

使⽤MyISAM存储引擎的表会把索引信息另外存储到⼀个称为 索引⽂件的另⼀个⽂件中，在索引的叶⼦节点中存储的不是完整的⽤户记 录，⽽是主键值 + ⾏号的组合

⽽在MyISAM中却需要进⾏⼀次回表操作，意味 着MyISAM中建⽴的索引相当于全部都是⼆级索引。对其它的列分别建⽴索引或者建 ⽴联合索引。在叶⼦节 点处存储的是相应的列 + ⾏号。这些索引也全部都是⼆级索 引。

MyISAM的⾏格式有定⻓记录格式（Static）、变⻓记录格式 （Dynamic）、压缩记录格式（Compressed）。index_demo表采⽤定⻓记录格式，也就是⼀条记录占⽤存储空间 的⼤⼩是固定的，这样就可以轻松算出某条记录在数据⽂件中的地 址偏移量。但是变⻓记录格式就不⾏了，MyISAM会直接在索引叶⼦ 节点处存储该条记录在数据⽂件中的地址偏移量。反观InnoDB是通过获取主键之后再去聚簇索引 ⾥边⼉找记录，虽然说也不慢，但还是⽐不上直接⽤地址去访问

为啥不⾃动为每个列都建⽴个索引呢？别忘了，每建 ⽴⼀个索引都会建⽴⼀棵B+树，每插⼊⼀条记录都要维护各个记 录、数据⻚的排序关系，这是很费性能和存储空间的。

总结

1. 对于InnoDB存储引擎来说，在单个⻚中查找某条记录分为两 种情况： 以主键为搜索条件，可以使⽤Page Directory通过⼆ 分法快速定位相应的⽤户记录。 以其他列为搜索条件，需要按照记录组成的单链表依次遍 历各条记录。
2. 没有索引的情况下，不论是以主键还是其他列作为搜索条件， 只能沿着⻚的双链表从左到右依次遍历各个⻚。
3. InnoDB存储引擎的索引是⼀棵B+树，完整的⽤户记录都存储 在B+树第0层的叶⼦节点，其他层次的节点都属于内节点，内 节点⾥存储的是⽬录项记录。InnoDB的索引分为两⼤种： 聚簇索引 以主键值的⼤⼩为⻚和记录的排序规则，在叶⼦节点处存 储的记录包含了表中所有的列。 ⼆级索引 以⾃定义的列的⼤⼩为⻚和记录的排序规则，在叶⼦节点 处存储的记录内容是列 + 主键。
4. MyISAM存储引擎的数据和索引分开存储，这种存储引擎的索 引全部都是⼆级索引，在叶⼦节点处存储的是列 + ⻚号

# 好东西也得先学会怎么用 —— B+ 树索引的使用

InnoDB存储引擎会⾃动为主键（如果没有它会⾃动帮我们添 加）建⽴聚簇索引，聚簇索引的叶⼦节点包含完整的⽤户记 录。

如果想通过⼆ 级索引来查找完整的⽤户记录的话，需要通过回表操作，也就 是在通过⼆级索引找到主键值之后再到聚簇索引中查找完整的 ⽤户记录

⼀个⻚默认会占 ⽤16KB的存储空间，⼀棵很⼤的B+树由许多数据⻚组成，那 可是很⼤的⼀⽚存储空间呢。

时间上的代价：每次对表中的数据进⾏增、删、改操作时，都需要去修改各 个B+树索引。⽽且我们讲过，B+树每层节点都是按照索引列的 值从⼩到⼤的顺序排序⽽组成了双向链表。不论是叶⼦节点中 的记录，还是内节点中的记录（也就是不论是⽤户记录还是⽬ 录项记录）都是按照索引列的值从⼩到⼤的顺序⽽形成了⼀个 单向链表。⽽增、删、改操作可能会对节点和记录的排序造成 破坏，所以存储引擎需要额外的时间进⾏⼀些记录移位，⻚⾯ 分裂、⻚⾯回收啥的操作来维护好节点和记录的排序。如果我 们建了许多索引，每个索引对应的B+树都要进⾏相关的维护操 作，这还能不给性能拖后腿么？

并不是所有的查询语句都能⽤ 到我们建⽴的索引

表中的主键是id列，它存储⼀个⾃动递增的整数。所以 InnoDB存储引擎会⾃动为id列建⽴聚簇索引。

idx_name_birthday_phone_number的示意图

其中内节点中⽬录项记录的⻚号信息我 们⽤箭头来代替

![image-20211130101207349](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130101210.png)

先按照name列的值进⾏排序。 如果name列的值相同，则按照birthday列的值进⾏排序。 如果birthday列的值也相同，则按照phone_number的值进 ⾏排序。

全值匹配

```
SELECT * FROM person_info WHERE birthday = '1990-
09-27' AND phone_number = '15123983239' AND name
= 'Ashburn';
```

调换顺序没啥影响，因为有查询优化器

```
SELECT * FROM person_info WHERE name = 'Ashburn'
AND phone_number = '15123983239';
```

这样只能⽤到name列的索引，birthday和phone_number的索引 就⽤不上了，因为name值相同的记录先按照birthday的值进⾏排 序，birthday值相同的记录才按照phone_number值进⾏排序。

name列：比较字符串大小就用到了该列的字符集和比较规则

查询以com为后缀的url：逆序 where url like ‘moc%’

![image-20211130104044619](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130104046.png)

![image-20211130104133081](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130171411.png)

记录之间⽤单链 表，数据⻚之间⽤双链表

如果对多个列同时进 ⾏范围查找的话，只有对索引最左边的那个列进⾏范围查找的时候才 能⽤到B+树索引

```
SELECT * FROM person_info WHERE name > 'Asa' AND
name < 'Barlow' AND birthday > '1980-01-01';
```

上边这个查询可以分成两个部分： 1. 通过条件name > ‘Asa’ AND name < ‘Barlow’来对 name进⾏范围，查找的结果可能有多条name值不同的记录， 2. 对这些name值不同的记录继续通过birthday > ‘1980-01- 01’条件继续过滤。

因为只 有name值相同的情况下才能⽤birthday列的值进⾏排序，⽽这个 查询中通过name进⾏范围查找的记录中可能并不是按照birthday 列进⾏排序的，所以在搜索条件中继续以birthday列进⾏查找时是 ⽤不到这个B+树索引的。

对多个列都进⾏范围查找时只能⽤到 最左边那个索引列，但是如果左边的列是精确查找，则右边的列可以 进⾏范围查找

```
SELECT * FROM person_info WHERE name = 'Ashburn'
AND birthday > '1980-01-01' AND birthday < '2000-
12-31' AND phone_number > '15100000000';
```

1. name = ‘Ashburn’，对name列进⾏精确查找，当然可以使 ⽤B+树索引了。
2. birthday > ‘1980-01-01’ AND birthday < ‘2000- 12-31’，由于name列是精确查找，所以通过name = ‘Ashburn’条件查找后得到的结果的name值都是相同的，它 们会再按照birthday的值进⾏排序。所以此时对birthday 列进⾏范围查找是可以⽤到B+树索引的。
3. phone_number > ‘15100000000’，通过birthday的范 围查找的记录的birthday的值可能不同，所以这个条件⽆法 再利⽤B+树索引了，只能遍历上⼀步查询得到的记录。

在MySQL中，把这种在内存中或者磁盘上进⾏排序的⽅式统称为⽂件 排序（英⽂名：filesort）

但是如果ORDER BY⼦句⾥使⽤到了我们的索引列，就有 可能省去在内存或⽂件中排序的步骤，⽐如下边这个简单的查询语句，因为这 个B+树索引本身就是排好序的，然后进⾏回表操作取出该索引中不包含的列就好了

**对于联合索引有个问题需要注意，ORDER BY的⼦句后边的列的顺序 也必须按照索引列的顺序给出，如果给出ORDER BY phone_number, birthday, name的顺序，那也是⽤不了B+树索 引，这种颠倒顺序就不能使⽤索引的原因我们上边详细说过了，这就 不赘述了**

ORDER BY name、ORDER BY name, birthday这种匹配 索引左边的列的形式可以使⽤部分的B+树索引。当联合索引左边列 的值为常量

**不可以使⽤索引进⾏排序的⼏种情况**

对于使⽤联合索引进⾏排序的场景，我们要求各个排序列的排序顺序 是⼀致的，也就是要么各个列都是ASC规则排序，要么都是DESC规 则排序。默认升序

ORDER BY name DESC, birthday DESC LIMIT 10， 这种情况直接从索引的最右边开始往左读10⾏记录就可以了

规定使⽤联合索引的各个排序列的排序顺序必须是⼀ 致的。

**如果WHERE⼦句中出现了⾮排序使⽤到的索引列，那么排序依然是 使⽤不到索引的**

**有时候⽤来排序的多个列不是⼀个索引⾥的，这种情况也不能使⽤索引进⾏排序**

**排序列使⽤了复杂的表达式**

SELECT * FROM person_info ORDER BY UPPER(name) LIMIT 10;

使⽤了UPPER函数修饰过的列就不是单独的列啦，这样就⽆法使⽤索 引进⾏排序啦。

**用于分组**

```
SELECT name, birthday, phone_number, COUNT(*)
FROM person_info GROUP BY name, birthday,
phone_number
```

针对那些⼩⼩分组进⾏统计，⽐如在我们这个查询语句中就是统 计每个⼩⼩分组包含的记录条数。

和使⽤B+树索引进⾏排序是⼀个道理，分组列的顺序也需要和索引 列的顺序⼀致，也可以只使⽤索引列中左边的列进⾏分组。

**顺序I/O**

随机I/O

在聚簇索引中记录是根据id（也就是主键）的顺序 排列的，所以根据这些并不连续的id值到聚簇索引中访问完整的⽤ 户记录可能分布在不同的数据⻚中，这样读取完整的⽤户记录可能要 访问更多的数据⻚，这种读取⽅式我们也可以称为随机I/O

所以这个使⽤索引idx_name_birthday_phone_number的查询有这么两个特点： 会使⽤到两个B+树索引，⼀个⼆级索引，⼀个聚簇索引。 访问⼆级索引使⽤顺序I/O，访问聚簇索引使⽤随机I/O。

需要回表的记录越多，使⽤⼆级索引的性能就越低，甚⾄让某些查询 宁愿使⽤全表扫描也不使⽤⼆级索引。⽐⽅说name值在Asa ～Barlow之间的⽤户记录数量占全部记录数量90%以上，那么如果 使⽤idx_name_birthday_phone_number索引的话，有90%多的 id值需要回表，这不是吃⼒不讨好么，还不如直接去扫描聚簇索引 （也就是全表扫描）

查询优化器：需要回表的记录数越多，就越倾向于使⽤全表扫描，反之倾向于使⽤⼆级索引 + 回表 的⽅式

限制查询获取较少的记录数会让优化器更 倾向于选择使⽤⼆级索引 + 回表的⽅式进⾏查询，因为回表的记录 越少，性能提升就越⾼

```
SELECT * FROM person_info ORDER BY name,
birthday, phone_number;
```

由于查询列表是*，所以如果使⽤⼆级索引进⾏排序的话，需要把排 序完的⼆级索引记录全部进⾏回表操作，这样操作的成本还不如直接 遍历聚簇索引然后再进⾏⽂件排序（filesort）低，所以优化器会 倾向于使⽤全表扫描的⽅式执⾏查询。

**最好在查询列表 ⾥只包含索引列**

所以在通过idx_name_birthday_phone_number索引得到结果后就不必到聚簇索引中再查找记录的剩余列，也就是country列 的值了，这样就省去了回表操作带来的性能损耗。我们把这种只需要⽤到索引的查询⽅式称为索引覆盖

```
SELECT name, birthday, phone_number FROM
person_info ORDER BY name, birthday,
phone_number;
```

这个查询中没有LIMIT⼦句，但是采⽤了覆盖索引，所以查询优 化器就会直接使⽤idx_name_birthday_phone_number索引进⾏ 排序⽽不需要回表操作了。

我们只需要为出现在WHERE⼦句中的name列创建索引就可以了

在记录⾏数⼀定的情况下，列的基数越⼤，该 列中的值越分散，列的基数越⼩，该列中的值越集中

假设某个列的 基数为1，也就是所有记录在该列中的值都⼀样，那为该列建⽴索引 是没有⽤的

如果某个建⽴了⼆级索引的列的重复值特别多，那么使⽤这个⼆ 级索引查出的记录还可能要做回表操作，这样性能损耗就更⼤了。所 以结论就是：最好为那些列的基数⼤的列建⽴索引，为基数太⼩列的 建⽴索引效果可能不好。

在表示的整数范围允许的情况下，尽量 让索引列使⽤较⼩的类型，⽐如我们能使⽤INT就不要使 ⽤BIGINT，能使⽤MEDIUMINT就不要使⽤INT

数据类型越⼩，在查询时进⾏的⽐较操作越快（这是CPU层次 的东东） 数据类型越⼩，索引占⽤的存储空间就越少，在⼀个数据⻚内 就可以放下更多的记录，从⽽减少磁盘I/O带来的性能损耗， 也就意味着可以把更多的数据⻚缓存在内存中，从⽽加快读写 效率。

这个建议对于表的主键来说更加适⽤，因为不仅是聚簇索引中会存储 主键值，其他所有的⼆级索引的节点处都会存储⼀份记录的主键值， 如果主键适⽤更⼩的数据类型，也就意味着节省更多的存储空间和更 ⾼效的I/O

B+树索引中的记录需要把该列的完整字符串存储起来，⽽且字 符串越⻓，在索引中占⽤的存储空间越⼤。 如果B+树索引中索引列存储的字符串很⻓，那在做字符串⽐较 时会占⽤更多的时间。

**只对字符串的前⼏个字符进⾏索引也就是 说在⼆级索引的记录中只保留字符串前⼏个字符**

这样只在B+树中存储字符串的前⼏个字符的编码，既 节约空间，⼜减少了字符串的⽐较时间，

这样只在B+树中存储字符串的前⼏个字符的编码，既 节约空间，⼜减少了字符串的⽐较时间，还⼤概能解决排序的问题

**在建表语句中只对name列的前10个字符进 ⾏索引**

```
CREATE TABLE person_info(
    name VARCHAR(100) NOT NULL,
    birthday DATE NOT NULL,
    phone_number CHAR(11) NOT NULL,
    country varchar(100) NOT NULL,
    KEY idx_name_birthday_phone_number (name(10),
                                        birthday, phone_number)
);
```

索引列前缀对排序的影响：使⽤索引列前缀的⽅ 式⽆法⽀持使⽤索引排序，只好乖乖的⽤⽂件排序喽

```
SELECT * FROM person_info ORDER BY name LIMIT 10;
```

如果索引列在⽐较表达式中不是以单独列的形式出 现，⽽是以某个表达式，或者函数调⽤形式出现的话，是⽤不到索引 的。比如

```
WHERE my_col * 2 < 4 # 不可以使用B+树索引
WHERE my_col < 4/2 #可以使用B+树索引
```

⻚⾯ 分裂和记录移位意味着什么？意味着：性能损耗！所以如果我们想尽 量避免这样⽆谓的性能损耗，最好让插⼊的记录的主键值依次递增， 这样就不会发⽣这样的性能损耗了。

让主键具 有AUTO_INCREMENT，让存储引擎⾃⼰为表⽣成主键，⽽不是我们 ⼿动插⼊

我们⾃定义的主键列id拥有AUTO_INCREMENT属性，在插⼊记录时 存储引擎会⾃动为我们填⼊⾃增的主键值。

维护冗余索引只会增加维护的成本，并不会对搜索有 什么好处。

**唯一索引**

如果该表包含具有重复键值的行，那么索引创建过程会失败。为表定义了唯一索引之后，每当在该索引内添加或更改键时就会强制执行唯一性。此强制执行包括插入、更新、装入、导入和设置完整性以命名一些键。除了强制数据值的唯一性以外，唯一索引还可用来提高查询处理期间检索数据的性能。

```
CREATE TABLE repeat_index_demo (
    c1 INT PRIMARY KEY,
    c2 INT,
    UNIQUE uidx_c1 (c1),
    INDEX idx_c1 (c1)
);
```

我们看到，c1既是主键、⼜给它定义为⼀个唯⼀索引，还给它定义 了⼀个普通索引，可是主键本身就会⽣成聚簇索引，所以定义的唯⼀ 索引和普通索引是重复的，这种情况要避免。

B+树索引适⽤于下边这些情况： 全值匹配 匹配左边的列 匹配范围值 精确匹配某⼀列并范围匹配另外⼀列 ⽤于排序 ⽤于分组

在使⽤索引时需要注意下边这些事项：

```
1. 只为⽤于搜索、排序或分组的列创建索引 
1. 为列的基数⼤的列创建索引 
1. 索引列的类型尽量⼩ 
1. 可以只对字符串值的前缀建⽴索引 
1. 只有索引列在⽐较表达式中单独出现才可以适⽤索引 
1. 为了尽可能少的让聚簇索引发⽣⻚⾯分裂和记录移位的情 况，建议让主键拥有AUTO_INCREMENT属性。 
1. 定位并删除表中的重复和冗余索引 尽量适⽤覆盖索引进⾏查询，避免回表带来的性能损耗
```

# 数据的家 —— MySQL 的数据目录

像 InnoDB 、 MyISAM 这样的存储引 擎都是把表存储在⽂件系统上的

**查看数据⽬录位置的两个⽅式：**

服务器未启动时（类Linux操作系统）：

```
mysqld --verbose --help | grep datadir
```

MySQL服务器程序在启动时会到⽂件系统的某个⽬录下加载⼀些⽂ 件，之后在运⾏过程中产⽣的数据也都会存储到这个⽬录下的某些⽂件中，这个⽬录就称为数据⽬录

```
SHOW VARIABLES LIKE 'datadir';
```

新建⼀个数据库时，MySQL会帮我们做这两件事⼉：

1. 在数据⽬录下创建⼀个和数据库名同名的⼦⽬录（或者说是⽂ 件夹）。
2. 在该与数据库名同名的⼦⽬录下创建⼀个名为db.opt的⽂ 件，这个⽂件中包含了该数据库的各种属性，⽐⽅说该数据库 的字符集和⽐较规则是个啥。

```
mysql> SHOW DATABASES;
+--------------------+
| Database |
+--------------------+
| information_schema |
| charset_demo_db |
| dahaizi |
| mysql |
| performance_schema |
| sys |
| xiaohaizi |
+--------------------+
7 rows in set (0.00 sec)


.
├── auto.cnf
├── ca-key.pem
├── ca.pem
├── charset_demo_db
├── client-cert.pem
├── client-key.pem
├── dahaizi
├── ib_buffer_pool
├── ib_logfile0
├── ib_logfile1
├── ibdata1
├── ibtmp1
├── mysql
├── performance_schema
├── private_key.pem
├── public_key.pem
├── server-cert.pem
├── server-key.pem
├── sys
├── xiaohaizideMacBook-Pro.local.err
├── xiaohaizideMacBook-Pro.local.pid
└── xiaohaizi
6 directories, 16 files
```

除了information_schema这个系统数据库外，其他的数据库 在数据⽬录下都有对应的⼦⽬录。这个information_schema⽐较 特殊，设计MySQL的⼤叔们对它的实现进⾏了特殊对待，没有使⽤ 相应的数据库⽬录，我们忽略它的存在就好了哈。

我们的数据其实都是以记录的形式插⼊到表中的，每个表的信息其实 可以分为两种： **1. 表结构的定义 2. 表中的数据**

**表结构**就是该表的名称是啥，表⾥边有多少列，每个列的数据类型是 啥，有啥约束条件和索引，⽤的是啥字符集和⽐较规则吧啦吧啦的各 种信息，这些信息都体现在了我们的建表语句中了。为了保存这些信 息InnoDB和MyISAM这两种存储引擎都在数据⽬录下对应的数据 库⼦⽬录下创建了⼀个专⻔⽤于描述表结构的⽂件，⽂件名是这样：表名.frm，⼆进 制格式存储的

![image-20211130134331665](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130134334.png)

**表空间**

为了管理页用的

每⼀个表空间可以 被划分为很多很多很多个⻚，我们的表数据就存放在某个表空间下的 某些⻚⾥。

**系统表空间 system tablespace**

**这个所谓的系统表空间可以对应⽂件系统上⼀个或多个实际的⽂件， 默认情况下，InnoDB会在数据⽬录下创建⼀个名为ibdata1（在你 的数据⽬录下找找看有⽊有）、⼤⼩为12M的⽂件，这个⽂件就是对 应的系统表空间在⽂件系统上的表示。**

系统表空间只有⼀ 份

独立表空间 file-per-table tablespace

在MySQL5.6.6以及之后的版本中，InnoDB并不会默认的把各个表 的数据存储到系统表空间中，⽽是为每⼀个表建⽴⼀个独⽴表空间， 也就是说我们创建了多少个表，就有多少个独⽴表空间。使⽤独⽴表 空间来存储表数据的话，会在该表所属数据库对应的⼦⽬录下创建⼀ 个表示该独⽴表空间的⽂件，⽂件名和表名相同，只不过添加了⼀ 个.ibd的扩展名⽽已，所以完整的⽂件名称⻓这样：表名.ibd

xiaohaizi数据库下的 test表

在该表所在数据库对应的xiaohaizi⽬录下会 为test表创建这两个⽂件： test.frm 和 test.ibd

test.ibd⽂件就⽤来存储test表中的数据和索引。

⽐如说我们想刻意 将表数据都存储到系统表空间时，可以在启动MySQL服务器的时候这 样配置：

```
[server]
innodb_file_per_table=0
```

当innodb_file_per_table的值为0时，代表使⽤系统表空间； 当innodb_file_per_table的值为1时，代表使⽤独⽴表空间。 不过innodb_file_per_table参数只对新建的表起作⽤，对于已 经分配了表空间的表并不起作⽤。

⽐⽅说我们想把test表从独⽴表空 间移动到系统表空间，可以这么写：

```
ALTER TABLE test TABLESPACE innodb_system;
```

**其他类型的表空间**

⽐如通⽤表空间（general tablespace）、undo表空间（undo tablespace）、临时表空间 （temporary tablespace）吧啦吧啦的

**MyISAM是如何存储表数据的**

在MyISAM中的索引全部都是⼆级索引，该存储引擎的数据和索引是 分开存放的。

**MyISAM并没有什么所谓的 表空间⼀说，表数据都存放到对应的数据库⼦⽬录下，表数据都存放到对应的数据库⼦⽬录下**

在数据库对应的xiaohaizi ⽬录下会为test表创建这三个⽂件：

```
test.frm
test.MYD
test.MYI
```

其中test.MYD代表表的数据⽂件，也就是我们插⼊的⽤户记 录；test.MYI代表表的索引⽂件，我们为该表创建的索引都会放到 这个⽂件中。

在存储视图的时候是不需要存储真实的数据的，只 需要把它的结构存储起来就⾏了。和表⼀样，描述视图结构的⽂件也 会被存储到所属数据库对应的⼦⽬录下边，只会存储⼀个视图 名.frm的⽂件

数据目录下还包括为了更好运⾏程序的⼀些额外⽂件：

**服务器进程⽂件**

**服务器⽇志⽂件**

**默认/⾃动⽣成的SSL和RSA证书和密钥⽂件**：主要是为了客户端和服务器安全通信⽽创建的⼀些⽂件， ⼤家 看不懂可以忽略～

**⽂件系统对数据库的影响**

1. 数据库名称和表名称不得超过⽂件系统所允许的最⼤⻓度。

2. 特殊字符的问题

   为了避免因为数据库名和表名出现某些特殊字符⽽造成⽂件系 统不⽀持的情况，MySQL会把数据库名和表名中所有除数字和 拉丁字⺟以外的所有字符在⽂件名⾥都映射成 @+编码值的形式 作为⽂件名。⽐⽅说我们创建的表的名称为’test?’，由于? 不属于数字或者拉丁字⺟，所以会被映射成编码值，所以这个 表对应的.frm⽂件的名称就变成了test@003f.frm。

3. ⽂件⻓度受⽂件系统最⼤⻓度限制

   对于InnoDB的独⽴表空间来说，每个表的数据都会被存储到 ⼀个与表名同名的.ibd⽂件中；对于MyISAM存储引擎来说， 数据和索引会分别存放到与表同名的.MYD和.MYI⽂件中。这 些⽂件会随着表中记录的增加⽽增⼤，它们的⼤⼩受限于⽂件 系统⽀持的最⼤⽂件⼤⼩。

4. 如果同时访问的表的数量⾮常多，可能会受到⽂件系统的 ⽂件描述符有限的影响

# 存放页面的大池子 —— InnoDB 的表空间

这块的知识量很大。

**⼤家 可以把表空间想象成被切分为许许多多个⻚的池⼦，当我们想为某个 表插⼊⼀条记录的时候，就从池⼦中捞出⼀个对应的⻚来把数据写进 去。**

⻚⾯类型

![image-20211130143449661](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130143452.png)

⻚⾯通⽤部分 我们前边说过数据⻚，也就是INDEX类型的⻚由7个部分组成，其中 的两个部分是所有类型的⻚⾯都通⽤的。当然我不能寄希望于你把我 说的话都记住，所以在这⾥重新强调⼀遍，任何类型的⻚⾯都有下边 这种通⽤的结构

![image-20211130143955888](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130143957.png)

File Trailer：校验⻚是否完整，保证从内存到磁盘刷新时 内容的⼀致性。

File Header：记录⻚⾯的⼀些通⽤信息

![image-20211130144120094](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130144122.png)

表空间中的每⼀个⻚都对应着⼀个⻚号，也就 是FIL_PAGE_OFFSET，这个⻚号由4个字节组成，也就是32 个⽐特位，所以⼀个表空间最多可以拥有2³²个⻚，如果按照⻚ 的默认⼤⼩16KB来算，⼀个表空间最多⽀持64TB的数据。表 空间的第⼀个⻚的⻚号为0，之后的⻚号分别是1，2，3…依此 类推

某些类型的⻚可以组成链表，链表中的⻚可以不按照物理顺序存储，⽽是根据FIL_PAGE_PREV和FIL_PAGE_NEXT来存储 上⼀个⻚和下⼀个⻚的⻚号。需要注意的是，这两个字段主要 是为了INDEX类型的⻚，也就是我们之前⼀直说的数据⻚建⽴ B+树后，为每层节点建⽴双向链表⽤的，⼀般类型的⻚是不使 ⽤这两个字段的。

每个⻚的类型由FIL_PAGE_TYPE表示，⽐如像数据⻚的该字 段的值就是0x45BF，我们后边会介绍各种不同类型的⻚，不 同类型的⻚在该字段上的值是不同的。

**独⽴表空间结构**

**区（extent）的概念**

对于16KB 的⻚来说，连续的64个⻚就是⼀个区，也就是说⼀个区默认占⽤ 1MB空间⼤⼩

![image-20211130145020329](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130145023.png)

![image-20211130145201413](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130145204.png)

独立表空间是由多个区组成，64个页组成一个区，256个区被划分为一组，**每个组的最开始的⼏个⻚⾯类型是固定 的就好了。**

**第⼀个组最开始的由3个⻚⾯的类型是固定的，也就是说 extent 0这个区最开始的3个⻚⾯的类型是固定的，分别是：**

1. FSP_HDR类型：这个类型的⻚⾯是⽤来登记整个表空间 的⼀些整体属性以及本组所有的区，也就是extent 0 ~ extent 255这256个区的属性，整个表空间只有⼀个FSP_HDR类型的⻚⾯。
2. IBUF_BITMAP类型：这个类型的⻚⾯是存储本组所有的区的所有⻚⾯关于INSERT BUFFER的信息。
3. INODE类型：这个类型的⻚⾯存储了许多称为INODE的数 据结构

**其余各组最开始的2个⻚⾯的类型是固定的，也就是说extent 256、extent 512这些区最开始的2个⻚⾯的类型是固定的， 分别是：**

XDES类型：全称是extent descriptor，⽤来登记本 组256个区的属性，也就是说对于在extent 256区中的 该类型⻚⾯存储的就是extent 256 ~ extent 511这 些区的属性，对于在extent 512区中的该类型⻚⾯存储 的就是extent 512 ~ extent 767这些区的属性。上 边介绍的FSP_HDR类型的⻚⾯其实和XDES类型的⻚⾯的 作⽤类似，只不过FSP_HDR类型的⻚⾯还会额外存储⼀ 些表空间的属性。

IBUF_BITMAP类型：这个类型的⻚⾯是存储本组所有的区的所有⻚⾯关于INSERT BUFFER的信息。

**段（segment）的概念**

我们每向表中插⼊⼀条记录，本质上就是向该表的聚簇索引以 及所有⼆级索引代表的B+树的节点中插⼊数据。⽽B+树的每⼀ 层中的⻚都会形成⼀个双向链表，如果是以⻚为单位来分配存 储空间的话，双向链表相邻的两个⻚之间的物理位置可能离得 ⾮常远。我们介绍B+树索引的适⽤场景的时候特别提到范围查 询只需要定位到最左边的记录和最右边的记录，然后沿着双向 链表⼀直扫描就可以了，⽽如果链表中相邻的两个⻚物理位置 离得⾮常远，就是所谓的随机I/O。再⼀次强调，磁盘的速度 和内存的速度差了好⼏个数量级，随机I/O是⾮常慢的，所以 我们应该尽量让链表中相邻的⻚的物理位置也相邻，这样进⾏ 范围查询的时候才可以使⽤所谓的顺序I/O。所以才引⼊了区（extent）的概念，⼀个区就是在物 理位置上连续的64个⻚。

**我们提到的范围查询，其实是对 B+树叶⼦节点中的记录进⾏顺序扫描，⽽如果不区分叶⼦节点和⾮ 叶⼦节点，统统把节点代表的⻚⾯放到申请到的区中的话，进⾏范围 扫描的效果就⼤打折扣了。所以设计InnoDB的⼤叔们对B+树的叶⼦ 节点和⾮叶⼦节点进⾏了区别对待，也就是说叶⼦节点有⾃⼰独有的 区，⾮叶⼦节点也有⾃⼰独有的区。存放叶⼦节点的区的集合就算是 ⼀个段（segment），存放⾮叶⼦节点的区的集合也算是⼀个段。 也就是说⼀个索引会⽣成2个段，⼀个叶⼦节点段，⼀个⾮叶⼦节点 段。**

**但是但是**⼀个 索引会⽣成2个段，⽽段是以区为单位申请存储空间的，⼀个区默认 占⽤1M存储空间，所以默认情况下⼀个只存了⼏条记录的⼩表也需 要2M的存储空间么？

**其实段是 ⼀些零散的⻚⾯以及⼀些完整的区的集合**

设计InnoDB的⼤叔们提出 了⼀个**碎⽚（fragment）区**的概念，也就是在⼀个碎⽚区中，并不 是所有的⻚都是为了存储同⼀个段的数据⽽存在的，⽽是碎⽚区中的 ⻚可以⽤于不同的⽬的，⽐如有些⻚⽤于段A，有些⻚⽤于段B，有 些⻚甚⾄哪个段都不属于。碎⽚区直属于表空间，并不属于任何⼀个 段。

某个段分配存储空间的策略是这样的： 在刚开始向表中插⼊数据的时候，段是从某个碎⽚区以单个⻚ ⾯为单位来分配存储空间的。 当某个段已经占⽤了32个碎⽚区⻚⾯之后，就会以完整的区为 单位来分配存储空间。

**表空间的是由若⼲个区组成的，这些 区⼤体上可以分为4种类型：**

- 空闲的区：现在还没有⽤到这个区中的任何⻚⾯。
- 有剩余空间的碎⽚区：表示碎⽚区中还有可⽤的⻚⾯。
- 没有剩余空间的碎⽚区：表示碎⽚区中的所有⻚⾯都被使⽤， 没有空闲⻚⾯。
- 附属于某个段的区。每⼀个索引都可以分为叶⼦节点段和⾮叶 ⼦节点段，除此之外 InnoDB 还会另外定义⼀些特殊作⽤的 段，在这些段中的数据量很⼤时将使⽤区来作为基本的分配单位。

**这4种类型的区也可以被称为区的4种状态（State）：**

```
状态名 含义
FREE 空闲的区
FREE_FRAG 有剩余空间的碎⽚区
FULL_FRAG没有剩余空间的碎⽚区
FSEG 附属于某个段的区
```

**处于FREE、FREE_FRAG以及FULL_FRAG 这三种状态的区都是独⽴的，算是直属于表空间；⽽处于FSEG状态 的区是附属于某个段的。**

为了⽅便管理这些区，设计InnoDB的⼤叔设计了⼀个称为XDES Entry的结构（全称就是Extent Descriptor Entry），每⼀个区都对应着⼀个XDES Entry结构，这个结构记录了对应的区的⼀些属 性。我们先看图来对这个结构有个⼤致的了解：

![image-20211130163831749](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130163833.png)

XDES Entry是⼀个40个字节的结构

1. 此处的Segment ID字段表示就是该区所在的段，当然前提是该区已经被分配给 某个段了，不然的话该字段的值没啥意义。

2. List Node : 这个部分可以将若⼲个XDES Entry结构串联成⼀个链表，⼤ 家看⼀下这个List Node的结构：

   如果我们想定位表空间内的某⼀个位置的话，只需指定⻚号以 及该位置在指定⻚号中的⻚内偏移量即可。所以： Pre Node Page Number和Pre Node Offset的组合 就是指向前⼀个XDES Entry的指针。 Next Node Page Number和Next Node Offset的 组合就是指向后⼀个XDES Entry的指针。

3. State: 这个字段表明区的状态：FREE、FREE_FRAG、FULL_FRAG和FSEG

4. Page State Bitmap（16字节） 这个部分共占⽤16个字节，也就是128个⽐特位

   ⼀个 区默认有64个⻚，这128个⽐特位被划分为64个部分，每个部 分2个⽐特位，对应区中的⼀个⻚。这两个⽐特位的第⼀个位表示对应的⻚是否是空闲 的，第⼆个⽐特位还没有⽤。

**我发现看着看着就有点迷糊了。**

这一块就这样吧，我截图好了，资料我回去再看一遍。。。。。。

![image-20211130204307935](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130204310.png)

![image-20211130204403546](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130204405.png)

![image-20211130205247223](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130205249.png)

File Space Header 这个部分是⽤来存储表空间的⼀些整体属性的

![image-20211130205512195](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130205514.png)

每个段对应的INODE Entry结构会集中存放到⼀个类型 位INODE的⻚中，如果表空间中的段特别多，则会有多 个INODE Entry结构，可能⼀个⻚放不下，这些INODE类型 的⻚会组成两种列表： SEG_INODES_FULL链表，该链表中的INODE类型的⻚⾯ 都已经被INODE Entry结构填充满了，没空闲空间存放 额外的INODE Entry了。 SEG_INODES_FULL链表，该链表中的INODE类型的⻚⾯ 都已经仍有空闲空间来存放INODE Entry结构

![image-20211130210858131](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130210859.png)

![image-20211130211043396](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130211050.png)

**SEG_INODES_FULL链表和SEG_INODES_FREE链表**

这两个链表的基节 点就存储在File Space Header⾥边，也就是说这两个链表的基 节点的位置是固定的，所以我们可以很轻松的访问到这两个链表。以 后每当我们新创建⼀个段（创建索引时就会创建段）时，都会创建⼀ 个INODE Entry结构与之对应，存储INODE Entry的⼤致过程就 是这样的： 先看看SEG_INODES_FREE链表是否为空，如果不为空，直接 从该链表中获取⼀个节点，也就相当于获取到⼀个仍有空闲空 间的INODE类型的⻚⾯，然后把该INODE Entry结构防到该 ⻚⾯中。当该⻚⾯中⽆剩余空间时，就把该⻚放 到SEG_INODES_FULL链表中。

如果SEG_INODES_FREE链表为空，则需要从表空间的 FREE_FRAG链表中申请⼀个⻚⾯，修改该⻚⾯的类型 为INODE，把该⻚⾯放到SEG_INODES_FREE链表中，与此同 时把该INODE Entry结构放⼊该⻚⾯。

![image-20211129174450680](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211129174603.png)

**Page Header部分**

**PAGE_BTR_SEG_LEAF 10字节 B+树叶⼦段的头部信息，仅在 B+树的根⻚定义**

**PAGE_BTR_SEG_TOP 10字节 B+树⾮叶⼦段的头部信息，仅在 B+树的根⻚定义**

其中的PAGE_BTR_SEG_LEAF和PAGE_BTR_SEG_TOP都占⽤10个字 节，它们其实对应⼀个叫Segment Header的结构，该结构图示如 下：

![image-20211130212315261](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130212317.png)

**这样⼦就很清晰了，PAGE_BTR_SEG_LEAF记录着叶⼦节点段对应 的INODE Entry结构的地址是哪个表空间的哪个⻚⾯的哪个偏移 量，PAGE_BTR_SEG_TOP记录着⾮叶⼦节点段对应的INODE Entry结构的地址是哪个表空间的哪个⻚⾯的哪个偏移量。这样⼦索 引和其对应的段的关系就建⽴起来了。不过需要注意的⼀点是，因为 ⼀个索引只对应两个段，所以只需要在索引的根⻚⾯中记录这两个结 构即可。**

![image-20211130212828043](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130212829.png)

系统表空间和独⽴表空间的前三个⻚⾯（⻚号分别 为0、1、2，类型分别是FSP_HDR、IBUF_BITMAP、INODE）的类 型是⼀致的，只是⻚号为3～7的⻚⾯是系统表空间特有的，我们来 看⼀下这些多出来的⻚⾯都是⼲啥使的

![image-20211130213633335](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130213635.png)

除了这⼏个记录系统属性的⻚⾯之外，系统表空间的extent 1和 extent 2这两个区，也就是⻚号从64~127这128个⻚⾯被称 为Doublewrite buffer，也就是双写缓冲区。不过上述的⼤部分 知识都涉及到了事务和多版本控制的问题

**先说说下面这个这个页面类型为SYS的页面**

**7 SYS Data Dictionary Header 数据字典头部信息**

**MySQL除了保存着我们插⼊的⽤ 户数据之外，还需要保存许多额外的信息：**

某个表属于哪个表空间，表⾥边有多少列

表对应的每⼀个列的类型是什么

该表有多少索引，每个索引对应哪⼏个字段

该索引对应的根 ⻚⾯在哪个表空间的哪个⻚⾯

该表有哪些外键，外键对应哪个表的哪些列

某个表空间对应⽂件系统上⽂件路径是什么 balabala … 还有好多，不⼀⼀列举了

**InnoDB存储引擎特意定义了⼀些列的内部 系统表（internal system table）来记录这些这些元数据**

![image-20211130214212746](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130214214.png)

**这些系统表也被称为数据字典，它们都是以B+树的形式保存在系统 表空间的某些⻚⾯中，其中 SYS_TABLES、SYS_COLUMNS、SYS_INDEXES、SYS_FIELDS这 四个表尤其重要，称之为基本系统表（basic system tables）**

![image-20211130231238857](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130231240.png)

这4个表是表中之表，那这4个表的元数据去哪⾥获取呢？ 没法搞了，只能把这4个表的元数据，就是它们有哪些列、哪些索引 等信息硬编码到代码中，然后设计InnoDB的⼤叔⼜拿出⼀个固定的 ⻚⾯来记录这4个表的聚簇索引和⼆级索引对应的B+树位置，这个⻚ ⾯就是⻚号为7的⻚⾯，类型为SYS，记录了Data Dictionary Header，也就是数据字典的头部信息。除了这4个表的5个索引的根 ⻚⾯信息外，这个⻚号为7的⻚⾯还记录了整个InnoDB存储引擎的 ⼀些全局属性

![image-20211130233435653](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211130233437.png)

可以看到这个⻚⾯⾥竟然有Segment Header部分，意味着设计 InnoDB的⼤叔把这些有关数据字典的信息当成⼀个段来分配存储空 间，我们就姑且称之为数据字典段吧。

Data Dictionary Header部分的 各个字段：

Max Row ID：我们说过如果我们不显式的为表定义主键，⽽ 且表中也没有UNIQUE索引，那么InnoDB存储引擎会默认为我 们⽣成⼀个名为row_id的列作为主键。因为它是主键，所以 每条记录的row_id列的值不能重复。原则上只要⼀个表中的 row_id列不重复就可以了，也就是说表a和表b拥有⼀样的 row_id列也没啥关系，不过设计InnoDB的⼤叔只提供了这 个Max Row ID字段，不论哪个拥有row_id列的表插⼊⼀条 记录时，该记录的row_id列的值就是Max Row ID对应的 值，然后再把Max Row ID对应的值加1，也就是说这个Max Row ID是全局共享的。

Max Table ID：InnoDB存储引擎中的所有的表都对应⼀个 唯⼀的ID，每次新建⼀个表时，就会把本字段的值作为该表的 ID，然后⾃增本字段的值。

Max Index ID：InnoDB存储引擎中的所有的索引都对应⼀ 个唯⼀的ID，每次新建⼀个索引时，就会把本字段的值作为该 索引的ID，然后⾃增本字段的值。

Max Space ID：InnoDB存储引擎中的所有的表空间都对应 ⼀个唯⼀的ID，每次新建⼀个表空间时，就会把本字段的值作 为该表空间的ID，然后⾃增本字段的值。

Mix ID Low(Unused)：这个字段没啥⽤，跳过。

Root of SYS_TABLES clust index：本字段代 表SYS_TABLES表聚簇索引的根⻚⾯的⻚号。

Root of SYS_TABLE_IDS sec index：本字段代 表SYS_TABLES表为ID列建⽴的⼆级索引的根⻚⾯的⻚号。

Root of SYS_COLUMNS clust index：本字段代 表SYS_COLUMNS表聚簇索引的根⻚⾯的⻚号。

Root of SYS_INDEXES clust： index本字段代 表SYS_INDEXES表聚簇索引的根⻚⾯的⻚号。

Root of SYS_FIELDS clust index：本字段代 表SYS_FIELDS表聚簇索引的根⻚⾯的⻚号。

Unused：这4个字节没⽤，跳过。

**information_schema系统数据库**

需要注意⼀点的是，⽤户是不能直接访问InnoDB的这些内部系统表 的，除⾮你直接去解析系统表空间对应⽂件系统上的⽂件。不过设计 InnoDB的⼤叔考虑到查看这些表的内容可能有助于⼤家分析问题， 所以在系统数据库information_schema中提供了⼀些以 innodb_sys开头的表：

```
mysql> USE information_schema;
Database changed
mysql> SHOW TABLES LIKE 'innodb_sys%';
+--------------------------------------------+
| Tables_in_information_schema (innodb_sys%) |
+--------------------------------------------+
| INNODB_SYS_DATAFILES |
| INNODB_SYS_VIRTUAL |
| INNODB_SYS_INDEXES |
| INNODB_SYS_TABLES |
| INNODB_SYS_FIELDS |
| INNODB_SYS_TABLESPACES |
| INNODB_SYS_FOREIGN_COLS |
| INNODB_SYS_COLUMNS |
| INNODB_SYS_FOREIGN |
| INNODB_SYS_TABLESTATS |
+--------------------------------------------+
10 rows in set (0.00 sec)
```

在information_schema数据库中的这些以INNODB_SYS开头的表 并不是真正的内部系统表（内部系统表就是我们上边唠叨的以SYS开 头的那些表），⽽是在存储引擎启动时读取这些以SYS开头的系统 表，然后填充到这些以INNODB_SYS开头的表中。以INNODB_SYS开 头的表和以SYS开头的表中的字段并不完全⼀样。

# 条条大路通罗马 —— 单表访问方法

⼀条查询语句进⾏语法解析 之后就会被交给查询优化器来进⾏优化，优化的结果就是⽣成⼀个所 谓的执⾏计划，这个执⾏计划表明了应该使⽤哪些索引进⾏查询，表 之间的连接顺序是啥样的，最后会按照执⾏计划中的步骤调⽤存储引 擎提供的⽅法来真正的执⾏查询，并将查询结果返回给⽤户。

查询的执⾏⽅式⼤致分为下边两种：

1. 使⽤全表扫描进⾏查询

   这种执⾏⽅式很好理解，就是把表的每⼀⾏记录都扫⼀遍嘛， 把符合搜索条件的记录加⼊到结果集就完了。不管是啥查询都 可以使⽤这种⽅式执⾏，当然，这种也是最笨的执⾏⽅式。

2. 使⽤索引进⾏查询

   因为直接使⽤全表扫描的⽅式执⾏查询要遍历好多记录，所以 代价可能太⼤了。如果查询语句中的搜索条件可以使⽤到某个 索引，那直接使⽤索引来执⾏查询可能会加快查询执⾏的时 间。使⽤索引来执⾏查询的⽅式五花⼋⻔，⼜可以细分为许多 种类：

   - 针对主键或唯⼀⼆级索引的等值查询
   - 针对普通⼆级索引的等值查询
   - 针对索引列的范围查询
   - 直接扫描整个索引

MySQL执⾏查询语句的⽅式称之为访问⽅法或者访问类型。

同⼀个查询语句可能可以使⽤多种不同的访问⽅法来执⾏。

**通过主键列来定位⼀条记录，MySQL会直接利⽤主键值在聚簇索引中定位对应的⽤户记录。**

![image-20211201000137731](C:/Users/86158/AppData/Roaming/Typora/typora-user-images/image-20211201000137731.png)

B+树本来就是⼀个矮矮的⼤胖⼦，所以这样根据主键值定位⼀ 条记录的速度贼快。根据唯⼀⼆级索引列来定位⼀条记 录的速度也是贼快的。

**const**

```
SELECT * FROM single_table WHERE key2 = 3841;
```

![image-20211201000408269](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205141626.png)

设计MySQL的⼤叔认为通过主键或者唯⼀⼆级索引列与常数的等值⽐ 较来定位⼀条记录是像坐⽕箭⼀样快的，所以他们**把这种通过主键或 者唯⼀⼆级索引列来定位⼀条记录的访问⽅法定义为：const，**意思 是常数级别的，代价是可以忽略不计的。不过这种const访问⽅法只 能在主键列或者唯⼀⼆级索引列和⼀个常数进⾏等值⽐较时才有效， 如果主键或者唯⼀⼆级索引是由多个列构成的话，索引中的每⼀个列 都需要与常数进⾏等值⽐较，这个const访问⽅法才有效（这是因为 只有该索引中全部列都采⽤等值⽐较才可以定位唯⼀的⼀条记录）。

**ref**

**对于唯⼀⼆级索引来说，查询该列为NULL值的情况⽐较特殊**

```
SELECT * FROM single_table WHERE key2 IS NULL;
```

**唯⼀⼆级索引列并不限制NULL值的数量，所以上述语句可能访 问到多条记录，也就是说上边这个语句不可以使⽤const访问⽅法来 执⾏。**

**普通⼆级索引并不限 制索引列值的唯⼀性，所以可能找到多条对应的记录，也就是说使⽤ ⼆级索引来执⾏查询的代价取决于等值匹配到的⼆级索引记录条数。采⽤⼆级索引来执⾏查询的访问⽅法称为：ref。**

![image-20211201085237092](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211201085300.png)

**⼆级索引列值为NULL的情况**

**不论是普通的⼆级索引，还是唯⼀⼆级索引，它们的索引列对 包含NULL值的数量并不限制，所以我们采⽤key IS NULL这 种形式的搜索条件最多只能使⽤ref的访问⽅法，⽽不 是const的访问⽅法。**

但是如果最左边的连续索引列并**不全部是等值⽐较**的话，它的 访问⽅法就**不能称为ref**了，⽐⽅说这样：

```
SELECT * FROM single_table WHERE key_part1 =
'god like' AND key_part2 > 'legendary';
```

**ref_or_null**

```
SELECT * FROM single_demo WHERE key1 = 'abc' OR
key1 IS NULL;
```

![image-20211201090014080](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211201090016.png)

**range**

```
SELECT * FROM single_table WHERE key2 IN (1438,
6328) OR (key2 >= 38 AND key2 <= 79);
```

![image-20211201090645411](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211201090646.png)

我们可以把那种索引列等值匹配的情况称之为单点区间，上边所说的 范围1和范围2都可以被称为单点区间，像范围3这种的我们可以称为 连续范围区间。

**index**

这种采⽤遍历⼆级索引记录的执⾏⽅式称之 为：index

```
SELECT key_part1, key_part2, key_part3 FROM
single_table WHERE key_part2 = 'abc';
```

它的查询列表只有3个列：key_part1, key_part2, key_part3，⽽索引idx_key_part⼜包含这三个列。 搜索条件中只有key_part2列。这个列也包含在索 引idx_key_part中。

也就是说我们可以直接通过遍历idx_key_part索引的叶⼦节点的 记录来⽐较key_part2 = ‘abc’这个条件是否成⽴，把匹配成功 的⼆级索引记录的key_part1, key_part2, key_part3列的值直 接加到结果集中就⾏了。由于⼆级索引记录⽐聚簇索记录⼩的多（聚 簇索引记录要存储所有⽤户定义的列以及所谓的隐藏列，⽽⼆级索引 记录只需要存放索引列和主键），⽽且这个过程也不⽤进⾏回表操 作，所以直接遍历⼆级索引⽐直接遍历聚簇索引的成本要⼩很多。

**all**

最直接的查询执⾏⽅式就是我们已经提了⽆数遍的全表扫描，对于 InnoDB表来说也就是**直接扫描聚簇索引**，设计MySQL的⼤叔把这种 使⽤全表扫描执⾏查询的⽅式称之为：all。

**注意 **

**二级索引+回表**

ref的访问⽅ 法⼀般⽐range好，但这也不总是⼀定的，也可能采⽤ref访问⽅法 的那个索引列的值为特定值的⾏数特别多

```
SELECT * FROM single_table WHERE key1 = 'abc' AND
key2 > 1000;
```

假设优化器决 定使⽤idx_key1索引进⾏查询，那么整个查询过程可以分为两个步 骤：

步骤1：使⽤⼆级索引定位记录的阶段，也就是根据条件key1 = ‘abc’从idx_key1索引代表的B+树中找到对应的⼆级索引 记录。

步骤2：回表阶段，也就是根据上⼀步骤中找到的记录的主键 值进⾏回表操作，也就是到聚簇索引中找到对应的完整的⽤户 记录，再根据条件key2 > 1000到完整的⽤户记录继续过 滤。将最终符合过滤条件的记录返回给⽤户。

**因为⼆级索引的节点中的记录只包 含索引列和主键，所以在步骤1中使⽤idx_key1索引进⾏查询时只 会⽤到与key1列有关的搜索条件，其余条件，⽐如key2 > 1000这 个条件在步骤1中是⽤不到的，只有在步骤2完成回表操作后才能继 续针对完整的⽤户记录中继续过滤。**

**明确range访问⽅法使⽤的范围区间**

其实对于B+树索引来说，只要索引列和常数使⽤=、<=>、IN、NOT IN、IS NULL、IS NOT NULL、>、<、>=、<=、BETWEEN、!=（不等于也可以写成<>）或 者LIKE操作符连接起来，就可以产⽣⼀个所谓的区间

IN操作符的效果和若⼲个等值匹配操作符`=`之间⽤`OR`连接起来 是⼀样的，也就是说会产⽣多个单点区间，⽐如下边这两个语句的 效果是⼀样的：

```
SELECT * FROM single_table WHERE key2 IN (1438, 6328); 

SELECT * FROM single_table WHERE key2 = 1438 OR key2 = 6328
```

**有的搜索条件⽆法使⽤索引的情况**

```
SELECT * FROM single_table WHERE key2 > 100 AND
common_field = 'abc';
```

请注意，这个查询语句中能利⽤的索引只有idx_key2⼀个， ⽽idx_key2这个⼆级索引的记录中⼜不包含common_field这个字 段，所以在使⽤⼆级索引idx_key2定位定位记录的阶段⽤不 到common_field = ‘abc’这个条件，这个条件是在回表获取了完 整的⽤户记录后才使⽤的，⽽范围区间是为了到索引中取记录中提出 的概念，所以在确定范围区间的时候不需要考虑common_field = ‘abc’这个条件，我们在为某个索引确定范围区间的时候只需要把⽤ 不到相关索引的搜索条件替换为TRUE就好了。

```
SELECT * FROM single_table WHERE key2 > 100 OR
common_field = 'abc';
=
SELECT * FROM single_table WHERE key2 > 100 OR
TRUE;
=
SELECT * FROM single_table WHERE TRUE;
```

这也就说说明如果我们强制使⽤idx_key2执⾏查询的话，对应 的范围区间就是(-∞, +∞)，也就是需要将全部⼆级索引的记录进⾏ 回表，这个代价肯定⽐直接全表扫描都⼤了。也就是说⼀个使⽤到索 引的搜索条件和没有使⽤该索引的搜索条件使⽤OR连接起来后是⽆ 法使⽤该索引的。

**分析复杂查询**

```
SELECT * FROM single_table WHERE
(key1 > 'xyz' AND key2 = 748 ) OR
(key1 < 'abc' AND key1 > 'lmn') OR
(key1 LIKE '%suf' AND key1 > 'zzz' AND
(key2 < 8000 OR common_field = 'abc')) ;
```

这个查询的搜索条件涉及到了key1、key2、common_field 这3个列，然后key1列有普通的⼆级索引idx_key1，key2列 有唯⼀⼆级索引idx_key2。**把那些⽤不到该索引的搜索条件暂时移除 掉，移除⽅法也简单，直接把它们替换为TRUE就好 了**。

**假设我们使⽤idx_key1执⾏查询**

```
(key1 > 'xyz' AND TRUE ) OR
(key1 < 'abc' AND key1 > 'lmn') OR
(TRUE AND key1 > 'zzz' AND (TRUE OR
TRUE))
= 
(key1 > 'xyz') OR
(key1 < 'abc' AND key1 > 'lmn') OR
(key1 > 'zzz')

# 因为符合key1 < 'abc' AND key1 > 'lmn'永远为FALSE，所以上边的搜索条件可以被写成这样：

(key1 > 'xyz') OR (key1 > 'zzz')
=
key1 > xyz
```

上边那个有⼀坨搜索条件的查询语句如果使⽤ idx_key1 索引执⾏查询的话，需要把满⾜key1 > xyz的⼆级索引记录都取出来，然后拿着这些记录 的id再进⾏回表，得到完整的⽤户记录之后再使⽤ 其他的搜索条件进⾏过滤。

**假设我们使⽤idx_key2执⾏查询**

```
(TRUE AND key2 = 748 ) OR
(TRUE AND TRUE) OR
(TRUE AND TRUE AND (key2 < 8000 OR
TRUE))
=
TRUE
```

**这个结果也就意味着如果我们要使⽤idx_key2索 引执⾏查询语句的话，需要扫描idx_key2⼆级索 引的所有记录，然后再回表，这不是得不偿失么， 所以这种情况下不会使⽤idx_key2索引的。**

**索引合并**

在⼀个 查询中使⽤到多个⼆级索引，设计MySQL的⼤叔把这种使⽤到多个索 引来完成⼀次查询的执⾏⽅法称之为：index merge，具体的索引 合并算法有下边三种。

Intersection合并 Intersection翻译过来的意思是交集。这⾥是说某个查询可以使 ⽤多个⼆级索引，将从多个⼆级索引中查询到的结果取交集，⽐⽅说 下边这个查询：

从idx_key1⼆级索引对应的B+树中取出key1 = ‘a’的相关 记录。 从idx_key3⼆级索引对应的B+树中取出key3 = ‘b’的相关 记录。 ⼆级索引的记录都是由索引列 + 主键构成的，所以我们可以 计算出这两个结果集中id值的交集。 按照上⼀步⽣成的id值列表进⾏回表操作，也就是从聚簇索引 中把指定id值的完整⽤户记录取出来，返回给⽤户。

虽然读取多个⼆级索引⽐读取⼀个⼆级索引消耗性能，但是读取⼆级 索引的操作是顺序I/O，⽽回表操作是随机I/O，所以如果只读取⼀ 个⼆级索引时需要回表的记录数特别多，⽽读取多个⼆级索引之后取 交集的记录数⾮常少，当节省的因为回表⽽造成的性能损耗⽐访问多 个⼆级索引带来的性能损耗更⾼时，读取多个⼆级索引后取交集⽐只 读取⼀个⼆级索引的成本更低。

**MySQL在某些特定的情况下才可能会使⽤到Intersection索引合 并：**

**情况⼀：⼆级索引列是等值匹配的情况，对于联合索引来说， 在联合索引中的每个列都必须等值匹配，不能出现只出现匹配 部分列的情况。**

⽽下边这两个查询就不能进⾏Intersection索引合并：

```
SELECT * FROM single_table WHERE key1 > 'a' AND key_part1 = 'a' AND key_part2 = 'b' AND key_part3 = 'c'; 

SELECT * FROM single_table WHERE key1 = 'a' AND key_part1 = 'a';
```

第⼀个查询是因为对key1进⾏了范围匹配，第⼆个查询是因为联合索引idx_key_part中的key_part2列并没有出现在搜索条件中，所以这两个查询不能进⾏Intersection索引合并。

**情况⼆：主键列可以是范围匹配**

⽐⽅说下边这个查询可能⽤到主键和idx_key_part进 ⾏Intersection索引合并的操作： SELECT * FROM single_table WHERE id > 100 AND key1 = ‘a’;

**之所以在⼆级索引列都是等值匹配的情况下才可能使 ⽤Intersection索引合并，是因为只有在这种情况下根据⼆级索 引查询出的结果集是按照主键值排序的。求交集的过程就是这样：逐个取出这两个结果集中最⼩的主键 值，如果两个值相等，则加⼊最后的交集结果中，否则丢弃当前较⼩ 的主键值，再取该丢弃的主键值所在结果集的后⼀个主键值来⽐较， 直到某个结果集中的主键值⽤完了。如果从各个⼆级索引中查询出的结果集并不是按照主键排序的话， 那就要先把结果集中的主键值排序完再来做上边的那个过程，就⽐较 耗时了。按照有序的主键值去回表取记录有个专有名词⼉，叫：Rowid Ordered Retrieval，简称ROR，以后⼤家在某些地⽅⻅到这个 名词⼉就眼熟了。**

上边说的情况⼀和情况⼆只是发⽣Intersection索引合并 的必要条件，不是充分条件。也就是说即使情况⼀、情况⼆成⽴，也 不⼀定发⽣Intersection索引合并，这得看优化器的⼼情。优化 器在下边两个条件满⾜的情况下才趋向于使⽤Intersection索引 合并： **单独根据搜索条件从某个⼆级索引中获取的记录数太多，导致 回表开销太⼤ 。通过Intersection索引合并后需要回表的记录数⼤⼤减少**

Intersection是交集的意思，这适⽤于使⽤不同索引的搜索条件 之间使⽤AND连接起来的情况；Union是并集的意思，适⽤于使⽤不 同索引的搜索条件之间使⽤OR连接起来的情况。

**Union合并**

**情况⼀：⼆级索引列是等值匹配的情况，对于联合索引来说， 在联合索引中的每个列都必须等值匹配，不能出现只出现匹配 部分列的情况。**

⽽下边这两个查询就不能进⾏Union索引合并：

```
SELECT * FROM single_table WHERE key1 > 'a' OR (key_part1 = 'a' AND key_part2 = 'b' AND key_part3 = 'c'); 
SELECT * FROM single_table WHERE key1 = 'a' OR key_part1 = 'a';
```

**情况⼆：主键列可以是范围匹配**

**情况三：使⽤Intersection索引合并的搜索条件**

```
SELECT * FROM single_table WHERE key_part1 = 'a' AND key_part2 = 'b' AND key_part3 = 'c' OR (key1 = 'a' AND key3 = 'b');
```

优化器可能采⽤这样的⽅式来执⾏这个查询： 先按照搜索条件key1 = ‘a’ AND key3 = ‘b’从索 引idx_key1和idx_key3中使⽤Intersection索引合 并的⽅式得到⼀个主键集合。 再按照搜索条件key_part1 = ‘a’ AND key_part2 = ‘b’ AND key_part3 = ‘c’从联合索 引idx_key_part中得到另⼀个主键集合。 采⽤Union索引合并的⽅式把上述两个主键集合取并集， 然后进⾏回表操作，将结果返回给⽤户。

优化器在下边两个条件满⾜的情况下才趋向于 使⽤Union索引合并： 单独根据搜索条件从某个⼆级索引中获取的记录数⽐较少。 通过Intersection索引合并后需要回表的记录数⼤⼤减少

**Sort-Union合并**

Union索引合并的使⽤条件太苛刻，必须保证各个⼆级索引列在进⾏ 等值匹配的条件下才可能被⽤到，⽐⽅说下边这个查询就⽆法使⽤ 到Union索引合并：

```
SELECT * FROM single_table WHERE key1 < 'a' OR key3 > 'z'
```

这是因为根据key1 < ‘a’从idx_key1索引中获取的⼆级索引记录 的主键值不是排好序的，根据key3 > ‘z’从idx_key3索引中获取 的⼆级索引记录的主键值也不是排好序的，但是key1 < ‘a’和 key3 > ‘z’这两个条件⼜特别让我们动⼼，所以我们可以这样：

先根据key1 < ‘a’条件从idx_key1⼆级索引总获取记录， 并按照记录的主键值进⾏排序

再根据key3 > ‘z’条件从idx_key3⼆级索引总获取记录， 并按照记录的主键值进⾏排序

因为上述的两个⼆级索引主键值都是排好序的，剩下的操作和 Union索引合并⽅式就⼀样了。

**我们把上述这种先按照⼆级索引记录的主键值进⾏排序，之后按 照Union索引合并⽅式执⾏的⽅式称之为Sort-Union索引合并，很 显然，这种Sort-Union索引合并⽐单纯的Union索引合并多了⼀步 对⼆级索引记录的主键值排序的过程。**

**没有Sort-Intersection索引 合并么？是的，的确没有Sort-Intersection索引合并这么⼀ 说，**

**Sort-Union的适⽤场景是单独根据搜索条件从某个⼆级索引中获 取的记录数⽐较少，这样即使对这些⼆级索引记录按照主键值进⾏ 排序的成本也不会太⾼**

⽽Intersection索引合并的适⽤场景是单独根据搜索条件从某个 ⼆级索引中获取的记录数太多，导致回表开销太⼤，合并后可以明 显降低回表开销，但是如果加⼊Sort-Intersection后，就需要 为⼤量的⼆级索引记录按照主键值进⾏排序，这个成本可能⽐回表 查询都⾼了，所以也就没有引⼊Sort-Intersection这个玩意 ⼉。

**联合索引替代Intersection索引合并**

```
ALTER TABLE single_table drop index idx_key1,
idx_key3, add index idx_key1_key3(key1, key3);
```

这样就不⽤多读⼀棵B+树，也不⽤合并结果，何乐⽽不为？

# 两个表的亲密接触 —— 连接的原理

```
mysql> SELECT * FROM t1;
+------+------+
| m1 | n1 |
+------+------+
| 1 | a |
| 2 | b |
| 3 | c |
+------+------+
3 rows in set (0.00 sec)
mysql> SELECT * FROM t2;
+------+------+
| m2 | n2 |
+------+------+
| 2 | b |
| 3 | c |
| 4 | d |
+------+------+
3 rows in set (0.00 sec)
```

![image-20211201104403171](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211201104424.png)

像这样的结果集就可以称之为笛卡尔积

在MySQL中，连接查询的语法也很随意，只要 在FROM语句后边跟多个表名就好了，⽐如我们把t1表和t2表连接起 来的查询语句可以写成这样：

```
mysql> SELECT * FROM t1, t2;
+------+------+------+------+
| m1 | n1 | m2 | n2 |
+------+------+------+------+
| 1 | a | 2 | b |
| 2 | b | 2 | b |
| 3 | c | 2 | b |
| 1 | a | 3 | c |
| 2 | b | 3 | c |
| 3 | c | 3 | c |
| 1 | a | 4 | d |
| 2 | b | 4 | d |
| 3 | c | 4 | d |
+------+------+------+------+
9 rows in set (0.00 sec)
```

**在连接的时候过滤掉特定 记录组合是有必要的，在连接查询中的过滤条件可以分成两种：涉及单表的条件和涉及两表的条件**

```
SELECT * FROM t1, t2 WHERE t1.m1 > 1 AND t1.m1 = t2.m2 AND t2.n2 < 'd';
```

在这个查询中我们指明了这三个过滤条件：

t1.m1 > 1

t1.m1 = t2.m2

t2.n2 < ‘d’

**那么这个连接查询的⼤致执⾏过程如下：**

1. ⾸先确定第⼀个需要查询的表，这个表称之为驱动表。怎样在 单表中执⾏查询语句我们在前⼀章都唠叨过了，只需要选取代 价最⼩的那种访问⽅法去执⾏单表查询语句就好了（就是说从 const、ref、ref_or_null、range、index、all这些执⾏⽅法 中选取代价最⼩的去执⾏查询）。此处假设使⽤t1作为驱动 表，那么就需要到t1表中找满⾜t1.m1 > 1的记录，因为表 中的数据太少，我们也没在表上建⽴⼆级索引，所以此处查询 t1表的访问⽅法就设定为all吧，也就是采⽤全表扫描的⽅式 执⾏单表查询。

![image-20211201105008995](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211201105010.png)

1. ![image-20211201105223782](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211201105225.png)

   也就是说整个连接查询最后的结果只有两条符合过滤条件的记录：

```
+------+------+------+------+
| m1 | n1 | m2 | n2 |
+------+------+------+------+
| 2 | b | 2 | b |
| 3 | c | 3 | c |
+------+------+------+------+
```

这个两表连接查询共需 要查询1次t1表，2次t2表。当然这是在特定的过滤条件下的结果， 如果我们把t1.m1 > 1这个条件去掉，那么从t1表中查出的记录就 有3条，就需要查询3次t3表了。也就是说在两表连接查询中，驱动 表只需要访问⼀次，被驱动表可能被访问多次。

**内连接和外连接**

内连接例子：

```
mysql> SELECT s1.number, s1.name, s2.subject,
s2.score FROM student AS s1, score AS s2 WHERE
s1.number = s2.number;
```

**需求：驱动表中的记录即使在被驱动表中没有匹配的记录，也仍然需要加⼊到结果集。**

为了 解决这个问题，就有了**内连接和外连接**的概念：

**对于内连接的两个表，驱动表中的记录在被驱动表中找不到匹配的记录，该记录不会加⼊到最后的结果集，我们上边提到的连接是所谓的内连接。**

**对于外连接的两个表，驱动表中的记录即使在被驱动表中没有 匹配的记录，也仍然需要加⼊到结果集。**

在MySQL中，根据选取驱动表的不同，外连接仍然可以细分为 2种：

- 左外连接 选取左侧的表为驱动表。
- 右外连接 选取右侧的表为驱动表。

**放 在不同地⽅的过滤条件是有不同语义的：**

**WHERE⼦句中的过滤条件**

WHERE⼦句中的过滤条件就是我们平时⻅的那种，不论是内连接还是外连接，凡是不符合WHERE⼦句中的过滤条件的记录都不会被加⼊最后的结果集。

**ON⼦句中的过滤条件**

对于外连接的驱动表的记录来说，如果⽆法在被驱动表中找到匹配ON⼦句中的过滤条件的记录，那么该记录仍然会被加⼊到 结果集中，对应的被驱动表记录的各个字段使⽤NULL值填充。

**需要注意的是，这个ON⼦句是专⻔为外连接驱动表中的记录在被驱动表找不到匹配记录时应不应该把该记录加⼊结果集这个 场景下提出的，所以如果把ON⼦句放到内连接中，MySQL会把 它和WHERE⼦句⼀样对待，也就是说：内连接中的WHERE⼦句 和ON⼦句是等价的。**

⼀般情况下，我们都把只涉及单表的过滤条件放到WHERE⼦句中，**把 涉及两表的过滤条件都放到ON⼦句中，我们也⼀般把放到ON⼦句中 的过滤条件也称之为连接条件。**

**左（外）连接的语法还是挺简单的，⽐如我们要把t1表和t2表进⾏ 左外连接查询可以这么写：**

```
SELECT * FROM t1 LEFT [OUTER] JOIN t2 ON 连接条件
[WHERE 普通过滤条件];
```

其中中括号⾥的OUTER单词是可以省略的。对于LEFT JOIN类型的 连接来说，我们把放在左边的表称之为外表或者驱动表，右边的表称 之为内表或者被驱动表。所以上述例⼦中t1就是外表或者驱动 表，t2就是内表或者被驱动表。需要注意的是，**对于左（外）连接 和右（外）连接来说，必须使⽤ON⼦句来指出连接条件。**了解了左 （外）连接的基本语法之后，再次回到我们上边那个现实问题中来， 看看怎样写查询语句才能把所有的学⽣的成绩信息都查询出来，即使 是缺考的考⽣也应该被放到结果集中：

```
mysql> SELECT s1.number, s1.name, s2.subject,
s2.score FROM student AS s1 LEFT JOIN score AS s2
ON s1.number = s2.number;
```

**右（外）连接的语法 右（外）连接和左（外）连接的原理是⼀样⼀样的，语法也只是把 LEFT换成RIGHT⽽已**

```
SELECT * FROM t1 RIGHT [OUTER] JOIN t2 ON 连接条件
[WHERE 普通过滤条件];
```

**内连接的语法**

内连接和外连接的根本区别就是在驱动表中的记录不符合ON⼦句中 的连接条件时不会把该记录加⼊到最后的结果集。

最简单的 内连接语法，就是直接把需要连接的多个表都放到FROM⼦句后边。 其实针对内连接，MySQL提供了好多不同的语法，我们以t1和t2表 为例瞅瞅：

```
mysql> SELECT s1.number, s1.name, s2.subject,
s2.score FROM student AS s1, score AS s2 WHERE
s1.number = s2.number;

SELECT * FROM t1 [INNER | CROSS] JOIN t2 [ON 连接
条件] [WHERE 普通过滤条件];

# 也就是说在MySQL中，下边这⼏种内连接的写法都是等价的：
SELECT * FROM t1 JOIN t2;
SELECT * FROM t1 INNER JOIN t2;
SELECT * FROM t1 CROSS JOIN t2;
#上边的这些写法和直接把需要连接的表名放到FROM语句之后，⽤逗号,分隔开的写法是等价的：
SELECT * FROM t1, t2;
#我们推荐INNER JOIN的形式书写内连接，由于在内连接中ON⼦句和WHERE⼦句是等价的，所以内连接中不要求强制写明ON⼦句
```

对于内连接来说，驱动表和被驱动表是可以互换的，并不会影响 最后的查询结果。但是对于外连接来说，由于驱动表中即使在被驱动表中找不到符合ON⼦句连接条件的记录仍然会被加⼊到 结果集中，所以此时驱动表和 被驱动表的关系就很重要了，也就是说左外连接和右外连接的驱动表 和被驱动表不能轻易互换。

```
mysql> SELECT * FROM t1 INNER JOIN t2 ON t1.m1 =
t2.m2;
+------+------+------+------+
| m1 | n1 | m2 | n2 |
+------+------+------+------+
| 2 | b | 2 | b |
| 3 | c | 3 | c |
+------+------+------+------+
2 rows in set (0.00 sec)
mysql> SELECT * FROM t1 LEFT JOIN t2 ON t1.m1 =
t2.m2;
+------+------+------+------+
| m1 | n1 | m2 | n2 |
+------+------+------+------+
| 2 | b | 2 | b |
| 3 | c | 3 | c |
| 1 | a | NULL | NULL |
+------+------+------+------+
3 rows in set (0.00 sec)
mysql> SELECT * FROM t1 RIGHT JOIN t2 ON t1.m1 =
t2.m2;
+------+------+------+------+
| m1 | n1 | m2 | n2 |
+------+------+------+------+
| 2 | b | 2 | b |
| 3 | c | 3 | c |
| NULL | NULL | 4 | d |
+------+------+------+------+
3 rows in set (0.00 sec)
```

t1表和t2表执⾏内连接查询的⼤致过程：

步骤1：选取驱动表，使⽤与驱动表相关的过滤条件，选取代价最低的单表访问⽅法来执⾏对驱动表的单表查询。

**嵌套循环连接（Nested-Loop Join）**

步骤2：对上⼀步骤中查询驱动表得到的结果集中每⼀条记 录，都分别到被驱动表中查找匹配的记录。

![image-20211201115045460](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211201115046.png)

如果有3个表进⾏连接的话，那么步骤2中得到的结果集就像是新的 驱动表，然后第三个表就成为了被驱动表，重复上边过程，也就是步 骤2中得到的结果集中的每⼀条记录都需要到t3表中找⼀找有没有匹 配的记录，⽤伪代码表示⼀下这个过程就是这样：

```
for each row in t1 { #此处表示遍历满⾜对t1单表查询结果集中的每⼀条记录
    for each row in t2 { #此处表示对于某条t1表的记录来说，遍历满⾜对t2单表查询结果集中的每⼀条记录
        for each row in t3 { #此处表示对于某条t1和t2表的记录组合来说，对t3表进⾏单表查询
            if row satisfies join conditions,
                            send to client
        }
	}
}
```

**所以这种驱动表只访问⼀次，但被驱动表却可能被多次访问，访问次数取决于对驱动表执⾏单表查询后 的结果集中的记录条数的连接执⾏⽅式称之为嵌套循环连接 （Nested-Loop Join），这是最简单，也是最笨拙的⼀种连接查询算法。**

**使⽤索引加快连接速度**

查询t2表其实就相当于⼀次单表扫描，我们 可以利⽤索引来加快查询速度哦

```
SELECT * FROM t1, t2 WHERE t1.m1 > 1 AND t1.m1 =
t2.m2 AND t2.n2 < 'd';
```

在m2列上建⽴索引，因为对m2列的条件是等值查找，⽐如 t2.m2 = 2、t2.m2 = 3等，所以可能使⽤到ref的访问⽅ 法，假设使⽤ref的访问⽅法去执⾏对t2表的查询的话，需要回表之后再判断t2.n2 < d这个条件是否成⽴。

这⾥有⼀个⽐较特殊的情况，就是假设m2列是t2表的主键或者 唯⼀⼆级索引列，那么使⽤t2.m2 = 常数值这样的条件从t2 表中查找记录的过程的代价就是常数级别的。我们知道在单表 中使⽤主键值或者唯⼀⼆级索引列的值进⾏等值查找的⽅式称 之为const，⽽设计MySQL的⼤叔把在连接查询中对被驱动表 使⽤主键值或者唯⼀⼆级索引列的值进⾏等值查找的查询执⾏ ⽅式称之为：**eq_ref**。

在n2列上建⽴索引，涉及到的条件是t2.n2 < ‘d’，可能⽤ 到range的访问⽅法，假设使⽤range的访问⽅法对t2表的查 询的话，需要回表之后再判断在m2列上的条件是否成⽴

假设m2和n2列上都存在索引的话，那么就需要从这两个⾥边⼉挑⼀ 个代价更低的去执⾏对t2表的查询。当然，建⽴了索引不⼀定使⽤ 索引，只有在⼆级索引 + 回表的代价⽐全表扫描的代价更低时才会 使⽤索引。

**另外，有时候连接查询的查询列表和过滤条件中可能只涉及被驱动表 的部分列，⽽这些列都是某个索引的⼀部分，这种情况下即使不能使 ⽤eq_ref、ref、ref_or_null或者range这些访问⽅法执⾏对被 驱动表的查询的话，也可以使⽤索引扫描，也就是index的访问⽅法 来查询被驱动表。**

**基于块的嵌套循环连接（Block Nested-Loop Join）**

扫描⼀个表的过程其实是先把这个表从磁盘上加载到内存中，然后从 内存中⽐较匹配条件是否满⾜。

采⽤嵌套循环连接算法的两表连接过程 中，被驱动表可是要被访问好多次的，如果这个被驱动表中的数据特 别多⽽且不能使⽤索引进⾏访问，那就相当于要从磁盘上读好⼏次这个表，这个I/O代价就⾮常⼤了，所以我们得想办法：**尽量减少访问 被驱动表的次数。**

我们可不可以在把被驱动表的记录加载到内 存的时候，**⼀次性和多条驱动表中的记录做匹配**，这样就可以⼤⼤减 少重复从磁盘上加载被驱动表的代价了。

join buffer就是执⾏连接查询前 申请的⼀块固定⼤⼩的内存，先把若⼲条驱动表结果集中的记录装在 这个join buffer中，然后开始扫描被驱动表，每⼀条被驱动表的 记录⼀次性和join buffer中的多条驱动表记录做匹配，因为匹配 的过程都是在内存中完成的，所以这样可以显著减少被驱动表的I/O 代价。

![image-20211201132142850](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211201132144.png)

**这种加⼊了join buffer的嵌套循环连接算法称之 为基于块的嵌套连接（Block Nested-Loop Join）算法。这个join buffer的⼤⼩是可以通过启动参数或者系统变量 join_buffer_size进⾏配置，默认⼤⼩为262144字节（也就 是256KB），最⼩可以设置为128字节。**

另外需要注意的是，驱动表的记录并不是所有列都会被放到join buffer中，只有查询列表中的列和过滤条件中的列才会被放到join buffer中，所以再次提醒我们，最好不要把*作为查询列表，只需 要把我们关⼼的列放到查询列表就好了，这样还可以在join buffer中放置更多的记录。

# 谁最便宜就选谁 —— MySQL 基于成本的优化

在MySQL中⼀条查询语句 的执⾏成本是由下边这两个⽅⾯组成的：

**I/O成本**

我们的表经常使⽤的MyISAM、InnoDB存储引擎都是将数据和 索引都存储到磁盘上的，当我们想查询表中的记录时，需要先 把数据或者索引加载到内存中然后再操作。这个从磁盘到内存 这个加载的过程损耗的时间称之为I/O成本。

**CPU成本**

读取以及检测记录是否满⾜对应的搜索条件、对结果集进⾏排 序等这些操作损耗的时间称之为CPU成本。

对于InnoDB存储引擎来说，⻚是磁盘和内存之间交互的基本单位

**基于成本的优化步骤**

1. 根据搜索条件，找出所有可能使⽤的索引
2. 计算全表扫描的代价
3. 计算使⽤不同索引执⾏查询的代价
4. 对⽐各种执⾏⽅案的代价，找出成本最低的那⼀个

```
CREATE TABLE single_table (
id INT NOT NULL AUTO_INCREMENT,
key1 VARCHAR(100),
key2 INT,
key3 VARCHAR(100),
key_part1 VARCHAR(100),
key_part2 VARCHAR(100),
key_part3 VARCHAR(100),
common_field VARCHAR(100),
PRIMARY KEY (id),
KEY idx_key1 (key1),
UNIQUE KEY idx_key2 (key2),
KEY idx_key3 (key3),
KEY idx_key_part(key_part1, key_part2,
key_part3)
) Engine=InnoDB CHARSET=utf8;


SELECT * FROM single_table WHERE
key1 IN ('a','b','c') AND
key2 > 10 AND key2 < 1000 AND
key3 > key2 AND
key_part1 LIKE '%hello%' AND
common_field = '123';
```

我们前边说过，对于B+树索引来说，只要索引列和常数使 ⽤=、<=>、IN、NOT IN、IS NULL、IS NOT NULL、>、<、>=、<=、BETWEEN、!=（不等于也可以写成<>）或 者LIKE操作符连接起来，就可以产⽣⼀个所谓的范围区间（LIKE匹 配字符串前缀也⾏），也就是说这些搜索条件都可能使⽤到索引，设计MySQL的⼤叔把⼀个查询中可能使⽤到的索引称之为possible keys。

```
key1 IN ('a','b','c')，这个搜索条件可以使⽤⼆级索引idx_key1。
key2 > 10 AND key2 < 1000，这个搜索条件可以使⽤⼆级索引idx_key2。
key3 > key2，这个搜索条件的索引列由于没有和常数⽐较，所以并不能使⽤到索引。
key_part1 LIKE '%hello%'，key_part1通过LIKE操作符和以通配符开头的字符串做⽐较，不可以适⽤索引。
common_field = '123'，由于该列上压根⼉没有索引，所以不会⽤到索引。
```

**综上所述，上边的查询语句可能⽤到的索引，也就是possible keys只有idx_key1和idx_key2。**

**计算全表扫描的代价**

全表扫描的意思就是把聚簇索引中的记 录都依次和给定的搜索条件做⼀下⽐较，把符合搜索条件的记录加⼊ 到结果集，所以需要将聚簇索引对应的⻚⾯加载到内存中，然后再检 测记录是否符合搜索条件。由于查询成本=I/O成本+CPU成本，所 以计算全表扫描的代价需要两个信息： **聚簇索引占⽤的⻚⾯数、该表中的记录数**

```
mysql> USE xiaohaizi;
Database changed
mysql> SHOW TABLE STATUS LIKE 'single_table'\G
*************************** 1. row
***************************
Name: single_table
Engine: InnoDB
Version: 10
Row_format: Dynamic
Rows: 9693
Avg_row_length: 163
Data_length: 1589248
Max_data_length: 0
Index_length: 2752512
Data_free: 4194304
Auto_increment: 10001
Create_time: 2018-12-10 13:37:23
Update_time: 2018-12-10 13:38:03
Check_time: NULL
Collation: utf8_general_ci
Checksum: NULL
Create_options:
Comment:
1 row in set (0.01 sec)
```

本选项表示表中的记录条数。对于使⽤MyISAM存储引擎的表 来说，该值是准确的，对于使⽤InnoDB存储引擎的表来说， 该值是⼀个估计值。Rows值只有9693条记录，实 际上表中有10000条记录。

Data_length = 聚簇索引的⻚⾯数量 x 每个⻚⾯的⼤⼩

我们的single_table使⽤默认16KB的⻚⾯⼤⼩，⽽上边查 询结果显示Data_length的值是1589248，所以我们可以反 向来推导出聚簇索引的⻚⾯数量：

聚簇索引的⻚⾯数量 = 1589248 ÷ 16 ÷ 1024 = 97

**计算使⽤不同索引执⾏查询的代价**

MySQL查询优化器先分析使⽤唯⼀⼆级索引的成本，再分析使 ⽤普通索引的成本，所以我们也先分析idx_key2的成本，然后再看 使⽤idx_key1的成本

![image-20211201140757448](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211201140758.png)

对于使⽤⼆级索引 + 回表⽅式的查询，设计MySQL的⼤叔计算这种 查询的成本依赖两个⽅⾯的数据：

**范围区间数量**

不论某个范围区间的⼆级索引到底占⽤了多少⻚⾯，**查询优化 器粗暴的认为读取索引的⼀个范围区间的I/O成本和读取⼀个⻚⾯是相同的。**本例中使⽤idx_key2的范围区间只有⼀ 个：(10, 1000)，所以相当于**访问这个范围区间的⼆级索引 付出的I/O成本就是：**

***1 x 1.0 = 1.0\***

**需要回表的记录数**

优化器需要计算⼆级索引的某个范围区间到底包含多少条记 录，对于本例来说就是要计算idx_key2在(10, 1000)这个 范围区间中包含多少⼆级索引记录

如果区间最左记录和区间最右记录相隔不太远 （在MySQL 5.7.21这个版本⾥，只要相隔不⼤于10个 ⻚⾯即可），那就可以精确统计出满⾜key2 > 10 AND key2 < 1000条件的⼆级索引记录条数。否则只沿着区 间最左记录向右读10个⻚⾯，计算平均每个⻚⾯中包含 多少记录，然后⽤这个平均值乘以区间最左记录和区间最 右记录之间的⻚⾯数量就可以了。**那么问题⼜来了，怎么 估计区间最左记录和区间最右记录之间有多少个⻚⾯呢？ 解决这个问题还得回到B+树索引的结构中来：**

![image-20211201141444348](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211201141446.png)

**我们想计算区间最左记录和区间最右记录之 间的⻚⾯数量就相当于计算⻚b和⻚c之间有多少⻚⾯， ⽽每⼀条⽬录项记录都对应⼀个数据⻚，所以计算⻚b和 ⻚c之间有多少⻚⾯就相当于计算它们⽗节点（也就是⻚ a）中对应的⽬录项记录之间隔着⼏条记录。**

**读取这95条⼆级索引记录需要付 出的CPU成本就是： \*95 x 0.2 + 0.01 = 19.01\***

**他 们认为每次回表操作都相当于访问⼀个⻚⾯，也就是说⼆ 级索引范围区间有多少记录，就需要进⾏多少次回表操 作，也就是需要进⾏多少次⻚⾯I/O。我们上边统计了使 ⽤idx_key2⼆级索引执⾏查询时，预计有95条⼆级索引 记录需要进⾏回表操作，所以回表操作带来的I/O成本就 是： \*95 x 1.0 = 95.0\***

回表操作后得到的完整⽤户记录，然后再检测其他搜索条 件是否成⽴

再检测除key2 > 10 AND key2 < 1000这个搜索条件以外的搜索条件是否成 ⽴。因为我们通过范围区间获取到⼆级索引记录共95 条，也就对应着聚簇索引中95条完整的⽤户记录，读取 并检测这些完整的⽤户记录是否符合其余的搜索条件的 CPU成本如下

***95 x 0.2 = 19.0* **

其中`95`是待检测记录的条数，`0.2`是检测⼀条记录是否符 合给定的搜索条件的成本常数。

**是否有可能使⽤索引合并（Index Merge） 本例中有关key1和key2的搜索条件是使⽤AND连接起来的，⽽对于 idx_key1和idx_key2都是范围查询，也就是说查找到的⼆级索引 记录并不是按照主键值进⾏排序的，并不满⾜使⽤Intersection 索引合并的条件，所以并不会使⽤索引合并。**

**⼀个单点区间对应的⼆级索引 记录的条数有多少，需要我们去计算。计算⽅式我们上边已经介绍过 了，就是先获取索引对应的B+树的区间最左记录和区间最右记录， 然后再计算这两条记录之间有多少记录（记录条数少的时候可以做到 精确计算，多的时候只能估算）。设计MySQL的⼤叔把这种通过直接 访问索引对应的B+树来计算某个范围区间对应的索引记录条数的⽅ 式称之为index dive。**

index dive就是直接利⽤索引对应的B+树来计算某个范围区间对应的记录 条数。

**系统变量 eq_range_index_dive_limit**

```
mysql> SHOW VARIABLES LIKE '%dive%';
+---------------------------+-------+
| Variable_name | Value |
+---------------------------+-------+
| eq_range_index_dive_limit | 200 |
+---------------------------+-------+
1 row in set (0.08 sec)
```

也就是说如果我们的IN语句中的参数个数⼩于200个的话，将使 ⽤index dive的⽅式计算各个单点区间对应的记录条数，如果⼤于 或等于200个的话，可就不能使⽤index dive了，要使⽤所谓的索 引统计数据来进⾏估算。怎么个估算法？继续往下看。

查看某个表中索引的统计数据可以使⽤SHOW INDEX FROM 表名的语法，⽐如我们查看⼀下single_table的各 个索引的统计数据可以这么写：

```
mysql> SHOW INDEX FROM single_table;
+--------------+------------+--------------+--------------+-------------+-----------+-------------
+----------+--------+------+------------+---------+---------------+
| Table | Non_unique | Key_name | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment |
+--------------+------------+--------------+--------------+-------------+-----------+-------------
+----------+--------+------+------------+---------+---------------+
| single_table | 0 | PRIMARY | 1 | id | A | 9693 | NULL | NULL | | BTREE | | | 
| single_table | 0 | idx_key2 | 1 | key2 | A | 9693 | NULL | NULL | YES | BTREE | | |
| single_table | 1 | idx_key1| 1 |key1| A | 968  | NULL | NULL |YES| BTREE | | | 
|single_table| 1 | idx_key3 | 1 | key3 | A | 799 | NULL | NULL | YES | BTREE | | |
| single_table | 1 | idx_key_part | 1 | key_part1 | A | 9673 | NULL | NULL | YES | BTREE | | |
| single_table | 1 | idx_key_part | 2 | key_part2 | A | 9999 | NULL | NULL | YES | BTREE | | |
| single_table | 1 | idx_key_part | 3 | key_part3 | A | 10000 | NULL | NULL | YES | BTREE | | |
+--------------+------------+--------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
7 rows in set (0.01 sec)
```

![image-20211201161020009](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211201161021.png)

**Cardinality直译过来就 是基数的意思，表示索引列中不重复值的个数。⽐如对于⼀个⼀万⾏ 记录的表来说，某个索引列的Cardinality属性是10000，那意味 着该列中没有重复的值，如果Cardinality属性是1的话，就意味 着该列的值全部是重复的。不过需要注意的是，对于InnoDB存储引 擎来说，使⽤SHOW INDEX语句展示出来的某个索引列的 Cardinality属性是⼀个估计值，并不是精确的。**

当IN语句中的参数个数⼤于或等于系统变量 eq_range_index_dive_limit的值的话，就不会使⽤index dive的⽅式计算各个单点区间对应的索引记录条数，⽽是使⽤索引 统计数据，这⾥所指的索引统计数据指的是这两个值：

使⽤SHOW TABLE STATUS展示出的Rows值，也就是⼀个表 中有多少条记录。 这个统计数据我们在前边唠叨全表扫描成本的时候说过很多遍 了，就不赘述了。

使⽤SHOW INDEX语句展示出的Cardinality属性。 结合上⼀个Rows统计数据，我们可以针对索引列，计算出平均 ⼀个值重复多少次。

**⼀个值的重复次数 ≈ Rows ÷ Cardinality** **它的致命弱点就是：不精确！**

![image-20211201161156659](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211201161158.png)

**连接查询的成本**

Condition filtering介绍

把对驱动表进⾏查询后得到的记录条数称之为驱动表的扇出（英 ⽂名：fanout）

对被驱动表的查询次数也就越少，连接查询的总成本也就越低。当查询优化器想计算整个连接查询所使⽤的成本时，就需要计算出驱动表的扇出值。

> 如果使⽤的是全表扫描的⽅式执⾏的单表查询，那么计算驱动 表扇出时需要猜满⾜搜索条件的记录到底有多少条。

> 如果使⽤的是索引执⾏的单表扫描，那么计算驱动表扇出的时 候需要猜满⾜除使⽤到对应索引的搜索条件外的其他搜索条件 的记录有多少条。

**这里的 condition filtering 作者解释得不是很严谨**

在MySQL 5.7之前的版本中，查询优化器在计算驱动表扇出时，如 果是使⽤全表扫描的话，就直接使⽤表中记录的数量作为扇出值， 如果使⽤索引的话，就直接使⽤满⾜范围条件的索引记录条数作为 扇出值。在MySQL 5.7中，设计MySQL的⼤叔引⼊了这个 condition filtering的功能，就是还要猜⼀猜剩余的那些搜索 条件能把驱动表中的记录再过滤多少条，其实本质上就是为了让成 本估算更精确。 我们所说的纯粹瞎猜其实是很不严谨的，设计MySQL的⼤叔们称之 为启发式规则（heuristic），⼤家有兴趣的可以再深⼊了解⼀下 哈。

**两表连接的成本分析**

**多表连接的成本分析**

这两块内容还蛮复杂的

连接查询总成本 = 单次访问驱动表的成本 + 驱动表扇出数 x 单 次访问被驱动表的成本

**调节成本常数**

⼀条语句的执⾏其实是分为两层的：

server层 存储引擎层

**mysql.server_cost表**

```
mysql> SELECT * FROM mysql.server_cost;
+------------------------------+------------+---------------------+---------+
| cost_name | cost_value | last_update | comment |
+------------------------------+------------+---------------------+---------+
| disk_temptable_create_cost | NULL | 2018-01-20 12:03:21 | NULL |
| disk_temptable_row_cost | NULL | 2018-01-20 12:03:21 | NULL |
| key_compare_cost | NULL | 2018-01-20 12:03:21 | NULL |
| memory_temptable_create_cost | NULL | 2018-01-20 12:03:21 | NULL |
| memory_temptable_row_cost | NULL | 2018-01-20 12:03:21 | NULL |
| row_evaluate_cost | NULL | 2018-01-20 12:03:21 | NULL |
+------------------------------+------------+---------------------+---------+
6 rows in set (0.05 sec)
```

MySQL在执⾏诸如DISTINCT查询、分组查询、Union查询以及某些 特殊条件下的排序查询都可能在内部先创建⼀个临时表，使⽤这个 临时表来辅助完成查询（⽐如对于DISTINCT查询可以建⼀个带 有UNIQUE索引的临时表，直接把需要去重的记录插⼊到这个临时表 中，插⼊完成之后的记录就是结果集了）。在数据量⼤的情况下可 能创建基于磁盘的临时表，也就是为该临时表使⽤MyISAM、 InnoDB等存储引擎，在数据量不⼤时可能创建基于内存的临时表， 也就是使⽤Memory存储引擎。创建临时表 和对这个临时表进⾏写⼊和读取的操作代价还是很⾼的。

```
UPDATE mysql.server_cost
SET cost_value = 0.4
WHERE cost_name = 'row_evaluate_cost';

FLUSH OPTIMIZER_COSTS;
```

**mysql.engine_cost表**

engine_cost表表中在存储引擎层进⾏的⼀些操作对应的成本常 数，具体内容如下：

```
mysql> SELECT * FROM mysql.engine_cost;
+-------------+-------------+------------------------+------------+---------------------+---------+
| engine_name | device_type | cost_name| cost_value | last_update | comment |
+-------------+-------------+------------------------+------------+---------------------+---------+
| default | 0 | io_block_read_cost| NULL | 2018-01-20 12:03:21 | NULL |
| default | 0 | memory_block_read_cost | NULL | 2018-01-2012:03:21 | NULL |
+-------------+-------------+------------------------+------------+--------------------+---------+
2 rows in set (0.05 sec)
```

# 兵马未动，粮草先行 —— InnoDB 统计数据是如何收集的

**两种不同的统计数据存储⽅式**

InnoDB提供了两种存储统计数据的⽅式：

永久性的统计数据 这种统计数据存储在磁盘上，也就是服务器重启之后这些统计 数据还在。

⾮永久性的统计数据 这种统计数据存储在内存中，当服务器关闭时这些这些统计数 据就都被清除掉了，等到服务器重启之后，在某些适当的场景 下才会重新收集这些统计数据。

系统变量 innodb_stats_persistent来控制到底采⽤哪种⽅式去存储统计 数据。在MySQL 5.6.6之前，innodb_stats_persistent的值 默认是OFF，也就是说InnoDB的统计数据默认是存储到内存的，之 后的版本中innodb_stats_persistent的值默认是ON，也就是统 计数据默认被存储到磁盘中。

**指定STATS_PERSISTENT属性来指明 该表的统计数据存储⽅式**

```
CREATE TABLE 表名 (...) Engine=InnoDB,
STATS_PERSISTENT = (1|0);

ALTER TABLE 表名 Engine=InnoDB, STATS_PERSISTENT =
(1|0);
```

当STATS_PERSISTENT=1时，表明我们想把该表的统计数据永久的 存储到磁盘上，当STATS_PERSISTENT=0时，表明我们想把该表的 统计数据临时的存储到内存中。如果我们在创建表时未指定 STATS_PERSISTENT属性，那默认采⽤系统变量 innodb_stats_persistent的值作为该属性的值。

**基于磁盘的永久性统计数据**

```
mysql> SHOW TABLES FROM mysql LIKE 'innodb%';
+---------------------------+
| Tables_in_mysql (innodb%) |
+---------------------------+
| innodb_index_stats |
| innodb_table_stats |
+---------------------------+
2 rows in set (0.01 sec)
```

这个innodb_table_stats表中的各个列

```
字段名 描述
database_name 数据库名
table_name 表名
last_update 本条记录最后更新时间
n_rows 表中记录的条数
clustered_index_size 表的聚簇索引占⽤的⻚⾯数量
sum_of_other_index_sizes表的其他索引占⽤的⻚⾯数量
```

主键是(database_name,table_name) 每条记录代表着⼀个表的统计信息

```
mysql> SELECT * FROM mysql.innodb_table_stats;
+---------------+---------------+---------------------+--------+----------------------+--------------------------+
|database_name|table_name|last_update|n_rows|clustered_index_size| sum_of_other_index_sizes |
+---------------+---------------+---------------------+--------+----------------------+-----------
---------------+
| mysql | gtid_executed | 2018-07-10 23:51:36 | 0 | 1 | 0 | 
| sys | sys_config | 2018-07-10 23:51:38 | 5 | 1 | 0 |
| xiaohaizi | single_table | 2018-12-10 17:03:13 | 9693 | 97 | 175 |
+---------------+---------------+---------------------+--------+----------------------+-----------
---------------+
3 rows in set (0.01 sec)
```

single_table表的统计信息就对应着mysql.innodb_table_stats的第三条记录。

⼏个重要统计信 息项的值如下：

n_rows的值是9693，表明single_table表中⼤约有9693 条记录，注意这个数据是估计值。

clustered_index_size的值是97，表明single_table表 的聚簇索引占⽤97个⻚⾯，这个值是也是⼀个估计值。

sum_of_other_index_sizes的值是175，表 明single_table表的其他索引⼀共占⽤175个⻚⾯，这个值 是也是⼀个估计值。

**按照⼀定算法（并不是纯粹随机的）选取⼏个叶⼦节点⻚⾯， 计算每个⻚⾯中主键值记录数量，然后计算平均⼀个⻚⾯中主 键值的记录数量乘以全部叶⼦节点的数量就算是该表的 n_rows值。**

**innodb_stats_persistent_sample_pages的系统变 量来控制使⽤永久性的统计数据时，计算统计数据时采样的⻚ ⾯数量。**

该值设置的越⼤，统计出的n_rows值越精确，但是 统计耗时也就最久；该值设置的越⼩，统计出的n_rows值越 不精确，但是统计耗时特别少。所以在实际使⽤是需要我们去 权衡利弊，该系统变量的默认值是20

```
CREATE TABLE 表名 (...) Engine=InnoDB,
STATS_SAMPLE_PAGES = 具体的采样⻚⾯数量;
ALTER TABLE 表名 Engine=InnoDB,
STATS_SAMPLE_PAGES = 具体的采样⻚⾯数量;
```

如果我们在创建表的语句中并没有指定 STATS_SAMPLE_PAGES属性的话，将默认使⽤系统变量 innodb_stats_persistent_sample_pages的值作为该 属性的值。

**clustered_index_size和sum_of_other_index_sizes统计项的 收集**

这两个统计项的收集过程如下：

从数据字典⾥找到表的各个索引对应的根⻚⾯位置。

系统表SYS_INDEXES⾥存储了各个索引对应的根⻚⾯信息。

从根⻚⾯的Page Header⾥找到叶⼦节点段和⾮叶⼦节点段对应的Segment Header。在每个索引的根⻚⾯的Page Header部分都有两个字段： PAGE_BTR_SEG_LEAF：表示B+树叶⼦段的Segment Header信息。 PAGE_BTR_SEG_TOP：表示B+树⾮叶⼦段的Segment Header信息。

从叶⼦节点段和⾮叶⼦节点段的Segment Header中找到这两个段对应的INODE Entry结构。 这个是Segment Header结构：

![image-20211202094920213](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211202094928.png)

从对应的INODE Entry结构中可以找到该段对应所有零散的 ⻚⾯地址以及FREE、NOT_FULL、FULL链表的基节点。

这个是INODE Entry结构：

![image-20211202095000922](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211202095003.png)

直接统计零散的⻚⾯有多少个，然后从那三个链表的List Length字段中读出该段占⽤的区的⼤⼩，每个区占⽤64个 ⻚，所以就可以统计出整个段占⽤的⻚⾯。

![image-20211202095056673](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211202095058.png)

分别计算聚簇索引的叶⼦结点段和⾮叶⼦节点段占⽤的⻚⾯ 数，它们的和就是clustered_index_size的值，按照同样 的套路把其余索引占⽤的⻚⾯数都算出来，加起来之后就 是sum_of_other_index_sizes的值。

这⾥需要⼤家注意⼀个问题，我们说⼀个段的数据在⾮常多时（超过 32个⻚⾯），会以区为单位来申请空间，这⾥头的问题是以区为单 位申请空间中有⼀些⻚可能并没有使⽤，但是在统计 clustered_index_size和sum_of_other_index_sizes时都 把它们算进去了，所以说聚簇索引和其他的索引占⽤的⻚⾯数可能⽐ 这两个值要⼩⼀些。

**innodb_index_stats**

```
字段名 描述
database_name 数据库名
table_name 表名
index_name 索引名
last_update 本条记录最后更新时间
stat_name 统计项的名称
stat_value 对应的统计项的值
sample_size 为⽣成统计数据⽽采样的⻚⾯数量
stat_description对应的统计项的描述
```

主键 是(database_name,table_name,index_name,stat_name)，innodb_index_stats表的每条记录代表着⼀个索引的⼀个统计项

index_name列，这个列说明该记录是哪个索引的统计 信息，从结果中我们可以看出来，PRIMARY索引（也就是主 键）占了3条记录，idx_key_part索引占了6条记录。

具体看⼀下⼀个索引都有哪些统计项：

- n_leaf_pages：表示该索引的叶⼦节点占⽤多少⻚⾯。

- size：表示该索引共占⽤多少⻚⾯。

- n_diff_pfxNN：表示对应的索引列不重复的值有多少。

  其中的NN⻓得有点⼉怪呀，啥意思呢？ 其实NN可以被替换为01、02、03… 这样的数字。⽐如对 于idx_key_part来说：

  - n_diff_pfx01表示的是统计key_part1这单单 ⼀个列不重复的值有多少
  - n_diff_pfx02表示的是统计key_part1、 key_part2这两个列组合起来不重复的值有多少。
  - n_diff_pfx03表示的是统计key_part1、 key_part2、key_part3这三个列组合起来不重 复的值有多少。
  - n_diff_pfx04表示的是统计key_part1、 key_part2、key_part3、id这四个列组合起来 不重复的值有多少。

对于普通的⼆级索引，并不能保证 它的索引列值是唯⼀的，⽐如对于idx_key1来说， key1列就可能有很多值重复的记录。此时只有在索引列 上加上主键值才可以区分两条索引列值都⼀样的⼆级索 引记录。

在计算某些索引列中包含多少不重复值时，需要对⼀些叶⼦节点⻚⾯进⾏采样，size列就表明了采样的⻚⾯数量是多少。

**对于有多个列的联合索引来说，采样的⻚⾯数量是： innodb_stats_persistent_sample_pages × 索引列的 个数。当需要采样的⻚⾯数量⼤于该索引的叶⼦节点数量的 话，就直接采⽤全表扫描来统计索引列的不重复值数量了。所 以⼤家可以在查询结果中看到不同索引对应的size列的值可能 是不同的。**

**定期更新统计数据**

随着我们不断的对表进⾏增删改操作，表中的数据也⼀直在变 化，innodb_table_stats和innodb_index_stats表⾥的统计数据是不是也应该跟着变⼀变了？

**开启innodb_stats_auto_recalc**

不过 ⾃动重新计算统计数据的过程是异步发⽣的，也就是即使表中 变动的记录数超过了10%，⾃动重新计算统计数据也不会⽴即 发⽣，可能会延迟⼏秒才会进⾏计算。

```
CREATE TABLE 表名 (...) Engine=InnoDB,
STATS_AUTO_RECALC = (1|0);
ALTER TABLE 表名 Engine=InnoDB,
STATS_AUTO_RECALC = (1|0);
```

如果我们在创建表时未指定 STATS_AUTO_RECALC属性，那默认采⽤系统变量 innodb_stats_auto_recalc的值作为该属性的值。

**⼿动调⽤ANALYZE TABLE语句来更新统计信息**

```
mysql> ANALYZE TABLE single_table;
+------------------------+---------+----------+----------+
| Table | Op | Msg_type | Msg_text |
+------------------------+---------+----------+----------+
| xiaohaizi.single_table | analyze | status | OK |
+------------------------+---------+----------+----------+
1 row in set (0.08 sec)
```

ANALYZE TABLE语句会⽴即重新计算统计数 据，也就是这个过程是同步的，在表中索引多或者采样⻚⾯特 别多时这个过程可能会特别慢，请不要没事⼉就运⾏⼀ 下ANALYZE TABLE语句，最好在业务不是很繁忙的时候再运 ⾏。

**⼿动更新innodb_table_stats和 innodb_index_stats表**

```
UPDATE innodb_table_stats
SET n_rows = 1
WHERE table_name = 'single_table';

FLUSH TABLE single_table;
```

**基于内存的⾮永久性统计数据**

与永久性的统计数据不同，⾮永久性的统计数据采样的⻚⾯数量是 由innodb_stats_transient_sample_pages控制的，这个系统 变量的默认值是8。

由于⾮永久性的统计数据经常更新，所以导致MySQL查询优化 器计算查询成本的时候依赖的是经常变化的统计数据，也就会⽣成经 常变化的执⾏计划。

**innodb_stats_method的使⽤**

我们知道索引列不重复的值的数量这个统计数据对于MySQL查询优化 器⼗分重要，因为通过它可以计算出在索引列中平均⼀个值重复多少 ⾏，**它的应⽤场景主要有两个：**

**单表查询中单点区间太多**，⽐⽅说这样： SELECT * FROM tbl_name WHERE key IN (‘xx1’ , ‘xx2’ , …, ‘xxn’); 当IN⾥的参数数量过多时，采⽤index dive的⽅式直接访问 B+树索引去统计每个单点区间对应的记录的数量就太耗费性能 了，所以直接依赖统计数据中的平均⼀个值重复多少⾏来计算 单点区间对应的记录数量。

**连接查询时，如果有涉及两个表的等值匹配连接条件，该连接 条件对应的被驱动表中的列⼜拥有索引时，则可以使⽤ref访 问⽅法来对被驱动表进⾏查询，⽐⽅说这样**

SELECT * FROM t1 JOIN t2 ON t1.column = t2.key WHERE …；

**在真正执⾏对t2表的查询前，t1.comumn的值是不确定的， 所以我们也不能通过index dive的⽅式直接访问B+树索引去 统计每个单点区间对应的记录的数量，所以也只能依赖统计数 据中的平均⼀个值重复多少⾏来计算单点区间对应的记录数 量。**

在统计索引列不重复的值的数量时，有⼀个⽐较烦的问题就是索引列 中出现NULL值怎么办

每⼀个`NULL`值都是独⼀⽆⼆的，也就是说统计索引列不重复 的值的数量时，应该把`NULL`值当作⼀个独⽴的值，所以`col`列 的不重复的值的数量就是：`4`（分别是1、2、NULL、NULL这四个 值）。

**innodb_stats_method系统变量**

相当于在计算某个索引列 不重复值的数量时如何对待NULL值这个锅甩给了⽤户，这个系统变 量有三个候选值：

- nulls_equal：认为所有NULL值都是相等的。这个值也是innodb_stats_method的默认值。 如果某个索引列中NULL值特别多的话，这种统计⽅式会让优化 器认为某个列中平均⼀个值重复次数特别多，所以倾向于不使 ⽤索引进⾏访问。
- nulls_unequal：认为所有NULL值都是不想等的。 如果某个索引列中NULL值特别多的话，这种统计⽅式会让优化 器认为某个列中平均⼀个值重复次数特别少，所以倾向于使⽤ 索引进⾏访问。
- nulls_ignored：直接把NULL值忽略掉。

最好不在索引列中 存放NULL值才是正解。

**总结**

InnoDB以表为单位来收集统计数据，这些统计数据可以是基 于磁盘的永久性统计数据，也可以是基于内存的⾮永久性统计 数据。

innodb_stats_persistent控制着使⽤永久性统计数据还 是⾮永久性统计数 据；innodb_stats_persistent_sample_pages控制着 永久性统计数据的采样⻚⾯数 量；innodb_stats_transient_sample_pages控制着⾮ 永久性统计数据的采样⻚⾯数 量；innodb_stats_auto_recalc控制着是否⾃动重新计算 统计数据。

我们可以针对某个具体的表，在创建和修改表时通过指定 STATS_PERSISTENT、STATS_AUTO_RECALC、STATS_SAMPLE_PAGES 的值来控制相关统计数据属性。

innodb_stats_method决定着在统计某个索引列不重复值的 数量时如何对待NULL值

# 不好看就要多整容 —— MySQL 基于规则的优化（内含关于子查询优化二三事儿）

这一部分的内容很简单

# 查询优化的百科全书 —— Explain 详解（上）（下）

都在解释各个字段的意思。

⼀条查询语句在经过MySQL查询优化器的各种基于成本和规则的优化 会后⽣成⼀个所谓的执⾏计划，这个执⾏计划展示了接下来具体执⾏ 查询的⽅式，⽐如多表连接的顺序是什么，对于每个表采⽤什么访问 ⽅法来具体执⾏查询等等。

```
列名 描述
id 在⼀个⼤的查询语句中每个SELECT关键字都对应⼀个唯⼀的id
select_type SELECT关键字对应的那个查询的类型
table 表名
partitions 匹配的分区信息
type 针对单表的访问⽅法
possible_keys可能⽤到的索引
key 实际上使⽤的索引
key_len 实际使⽤到的索引⻓度
ref 当使⽤索引列等值查询时，与索引列进⾏等值匹配的对象信息
rows 预估的需要读取的记录条数
filtered 某个表经过搜索条件过滤后剩余记录条数的百分⽐
Extra ⼀些额外的信息
```

了解到索引下推这个新概念

```
SELECT * FROM s1 WHERE key1 > 'z' AND key1
LIKE '%a';
```

但是虽然key1 LIKE ‘%a’不能组成范围区间参与range访问 ⽅法的执⾏，但这个条件毕竟只涉及到了key1列，所以设计 MySQL的⼤叔把上边的步骤改进了⼀下：

先根据key1 > ‘z’这个条件，定位到⼆级索 引idx_key1中对应的⼆级索引记录。

对于指定的⼆级索引记录，先不着急回表，⽽是先检测⼀ 下该记录是否满⾜key1 LIKE ‘%a’这个条件，如果这 个条件不满⾜，则该⼆级索引记录压根⼉就没必要回表。

对于满⾜key1 LIKE ‘%a’这个条件的⼆级索引记录执 ⾏回表操作。

我们说回表操作其实是⼀个随机IO，⽐较耗时，所以上述修改 虽然只改进了⼀点点，但是可以省去好多回表操作的成本。设 计MySQL的⼤叔们把他们的这个改进称之为索引条件下推（英 ⽂名：Index Condition Pushdown）。

如果在查询语句的执⾏过程中将要使⽤索引条件下推这个特 性，在Extra列中将会显示Using index condition

**Json格式的执⾏计划**

# 神兵利器 —— optimizer trace 的神器功效

optimizer trace的功能，这个功能可以让我 们⽅便的查看优化器⽣成执⾏计划的整个过程，这个功能的开启与关 闭由系统变量optimizer_trace决定，我们看⼀下：

```
mysql> SHOW VARIABLES LIKE 'optimizer_trace';
+-----------------+--------------------------+
| Variable_name | Value |
+-----------------+--------------------------+
| optimizer_trace | enabled=off,one_line=off


mysql> SET optimizer_trace="enabled=on";
Query OK, 0 rows affected (0.00 sec)
```

然后我们就可以输⼊我们想要查看优化过程的查询语句，当该查询语 句执⾏完成后，就可以到information_schema数据库下的 OPTIMIZER_TRACE表中查看完整的优化过程。这 个OPTIMIZER_TRACE表有4个列，分别是：

QUERY：表示我们的查询语句。

TRACE：表示优化过程的JSON格式⽂本。

MISSING_BYTES_BEYOND_MAX_MEM_SIZE：由于优化过程 可能会输出很多，如果超过某个限制时，多余的⽂本将不会被 显示，这个字段展示了被忽略的⽂本字节数。

INSUFFICIENT_PRIVILEGES：表示是否没有权限查看优化 过程，默认值是0，只有某些特殊情况下才会是1，我们暂时不 关⼼这个字段的值。

# 调节磁盘和CPU的矛盾 —— InnoDB 的 Buffer Pool

![image-20211202203551656](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211202203553.png)

![image-20211202203615020](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211202203616.png)

![image-20211202211130971](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211202211132.png)

innodb_old_blocks_time的默认值是1000，它的单位是毫 秒，也就意味着对于从磁盘上被加载到LRU链表的old区域的某个⻚ 来说，如果第⼀次和最后⼀次访问该⻚⾯的时间间隔⼩于1s（很明 显在⼀次全表扫描的过程中，多次访问⼀个⻚⾯中的时间不会超过 1s），那么该⻚是不会被加⼊到young区域的

综上所述，正是因为将LRU链表划分为young和old区域这两个部 分，⼜添加了innodb_old_blocks_time这个系统变量，才使得 预读机制和全表扫描造成的缓存命中率降低的问题得到了遏制，因为 ⽤不到的预读⻚⾯以及全表扫描的⻚⾯都只会被放到old区域，⽽不 影响young区域中的缓存⻚。

只有被访问的缓存⻚位于young区域的1/4的后边，才 会被移动到LRU链表头部，这样就可以降低调整LRU链表的频率，从 ⽽提升性能（也就是说如果某个缓存⻚对应的节点在young区域的 1/4中，再次访问该缓存⻚时也不会将其移动到LRU链表头部）

我们之前介绍随机预读的时候曾说，如果Buffer Pool中有某个区 的13个连续⻚⾯就会触发随机预读，这其实是不严谨的（不幸的是 MySQL⽂档就是这么说的[摊⼿]），其实还要求这13个⻚⾯是⾮常 热的⻚⾯，所谓的⾮常热，指的是这些⻚⾯在整个young区域的 头1/4处。

**其他的⼀些链表**

为了更好的管理Buffer Pool中的缓存⻚，除了我们上边提到的⼀ 些措施，设计InnoDB的⼤叔们还引进了其他的⼀些链表，⽐如 unzip LRU链表⽤于管理解压⻚，zip clean链表⽤于管理没有被 解压的压缩⻚，zip free数组中每⼀个元素都代表⼀个链表，它们 组成所谓的伙伴系统来为压缩⻚提供内存空间等等，反正是为了更好 的管理这个Buffer Pool引⼊了各种链表或其他数据结构

从LRU链表的冷数据中刷新⼀部分⻚⾯到磁盘。 后台线程会定时从LRU链表尾部开始扫描⼀些⻚⾯，扫描的⻚ ⾯数量可以通过系统变量innodb_lru_scan_depth来指 定，如果从⾥边⼉发现脏⻚，会把它们刷新到磁盘。这种刷新 ⻚⾯的⽅式被称之为BUF_FLUSH_LRU。

从flush链表中刷新⼀部分⻚⾯到磁盘。 后台线程也会定时从flush链表中刷新⼀部分⻚⾯到磁盘，刷 新的速率取决于当时系统是不是很繁忙。这种刷新⻚⾯的⽅式 被称之为BUF_FLUSH_LIST。

有时候后台线程刷新脏⻚的进度⽐较慢，导致⽤户线程在准备加载⼀ 个磁盘⻚到Buffer Pool时没有可⽤的缓存⻚，这时就会尝试看看 LRU链表尾部有没有可以直接释放掉的未修改⻚⾯，如果没有的话会 不得不将LRU链表尾部的⼀个脏⻚同步刷新到磁盘（和磁盘交互是很 慢的，这会降低处理⽤户请求的速度）。这种刷新单个⻚⾯到磁盘中 的刷新⽅式被称之为BUF_FLUSH_SINGLE_PAGE。 当然，有时候系统特别繁忙时，也可能出现⽤户线程批量的从flush 链表中刷新脏⻚的情况，很显然在处理⽤户请求过程中去刷新脏⻚是 ⼀种严重降低处理速度的⾏为

在多线程环境下，访问Buffer Pool中的各种 链表都需要加锁处理啥的，在Buffer Pool特别⼤⽽且多线程并发 访问特别⾼的情况下，单⼀的Buffer Pool可能会影响请求的处理 速度。所以在Buffer Pool特别⼤的时候，我们可以把它们**拆分成 若⼲个⼩的Buffer Pool**，每个Buffer Pool都称为⼀个实例， 它们都是独⽴的，独⽴的去申请内存空间，独⽴的管理各种链表，独 ⽴的吧啦吧啦，所以在多线程并发访问时并不会相互影响，从⽽提⾼ 并发处理能⼒。

```
[server]
innodb_buffer_pool_instances = 2
```

![image-20211202212026462](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211202212028.png)

这些链表的节点其实就是每个缓存⻚对应的控制块！

**当innodb_buffer_pool_size的值⼩于1G的时候设置多个实例 是⽆效的，InnoDB会默认把innodb_buffer_pool_instances 的值 修改为1。⽽我们⿎励在Buffer Pool⼤⼩或等于1G的时候设置多 个Buffer Pool实例。**

**⼀ 个Buffer Pool实例其实是由若⼲个chunk组成的，⼀个chunk就 代表⼀⽚连续的内存空间，⾥边⼉包含了若⼲缓存⻚与其对应的控制 块，画个图表示就是这样：**

![image-20211202212405119](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211202212406.png)

上图代表的Buffer Pool就是由2个实例组成的，每个实例中⼜包 含2个chunk。

正是因为发明了这个chunk的概念，我们在服务器运⾏期间调 整Buffer Pool的⼤⼩时就是以chunk为单位增加或者删除内存空 间，⽽不需要重新向操作系统申请⼀⽚⼤的内存，然后进⾏缓存⻚的 复制。这个所谓的chunk的⼤⼩是我们在启动操作MySQL服务器时通 过innodb_buffer_pool_chunk_size启动参数指定的，它的默 认值是134217728，也就是128M。不过需要注意的是， innodb_buffer_pool_chunk_size的值只能在服务器启动时指定， 在服务器运⾏过程中是不可以修改的。

另外，这个innodb_buffer_pool_chunk_size的值并不包含缓 存⻚对应的控制块的内存空间⼤⼩，所以实际上InnoDB向操作系统 申请连续内存空间时，每个chunk的⼤⼩要⽐ innodb_buffer_pool_chunk_size的值⼤⼀些，约5%。

配置Buffer Pool时的注意事项 innodb_buffer_pool_size必须 是innodb_buffer_pool_chunk_size × innodb_buffer_pool_instances的倍数（这主要是想保 证每⼀个Buffer Pool实例中包含的chunk数量相同）。

假设我们指定的innodb_buffer_pool_chunk_size的值 是128M，innodb_buffer_pool_instances的值是16，那 么这两个值的乘积就是2G，也就是说 innodb_buffer_pool_size的值必须是2G或者2G的整数 倍。

⽐⽅说我们在启动MySQL服务器是这样指定启动参数的：

```
mysqld --innodb-buffer-pool-size=8G --innodb-buffer-pool-instances=16
```

**如果我们指定的innodb_buffer_pool_size⼤于2G并且不 是2G的整数倍，那么服务器会⾃动的把 innodb_buffer_pool_size的值调整为2G的整数倍。**

如果在服务器启动时，innodb_buffer_pool_chunk_size × innodb_buffer_pool_instances的值已经⼤于 innodb_buffer_pool_size的值，那 么innodb_buffer_pool_chunk_size的值会被服务器⾃动 设置 为innodb_buffer_pool_size/innodb_buffer_pool_instances 的值。

**Buffer Pool的缓存⻚除了⽤来缓存磁盘上的⻚⾯以外，还可以存 储锁信息、⾃适应哈希索引等信息。**

```
mysql> SHOW ENGINE INNODB STATUS\G
```

Total memory allocated：代表Buffer Pool向操作系 统申请的连续内存空间⼤⼩，包括全部控制块、缓存⻚、以及 碎⽚的⼤⼩。

Dictionary memory allocated：为数据字典信息分配的 内存空间⼤⼩，注意这个内存空间和Buffer Pool没啥关 系，不包括在Total memory allocated中。

Buffer pool size：代表该Buffer Pool可以容纳多少缓 存⻚，注意，单位是⻚！

Free buffers：代表当前Buffer Pool还有多少空闲缓存 ⻚，也就是free链表中还有多少个节点。

Database pages：代表LRU链表中的⻚的数量，包含young 和old两个区域的节点数量。

Old database pages：代表LRU链表old区域的节点数量。

Modified db pages：代表脏⻚数量，也就是flush链表中 节点的数量。

Pending reads：正在等待从磁盘上加载到Buffer Pool中 的⻚⾯数量。 当准备从磁盘中加载某个⻚⾯时，会先为这个⻚⾯在Buffer Pool中分配⼀个缓存⻚以及它对应的控制块，然后把这个控制 块添加到LRU的old区域的头部，但是这个时候真正的磁盘⻚ 并没有被加载进来，Pending reads的值会跟着加1。

Pending writes LRU：即将从LRU链表中刷新到磁盘中的 ⻚⾯数量。

Pending writes flush list：即将从flush链表中刷新 到磁盘中的⻚⾯数量。

Pending writes single page：即将以单个⻚⾯的形式 刷新到磁盘中的⻚⾯数量。

Pages made young：代表LRU链表中曾经从old区域移动 到young区域头部的节点数量。 这⾥需要注意，⼀个节点每次只有从old区域移动到young区 域头部时才会将Pages made young的值加1，也就是说如果 该节点本来就在young区域，由于它符合在young区域1/4后 边的要求，下⼀次访问这个⻚⾯时也会将它移动到young区域 头部，但这个过程并不会导致Pages made young的值加1。

Page made not young：在 将innodb_old_blocks_time设置的值⼤于0时，⾸次访问 或者后续访问某个处在old区域的节点时由于不符合时间间隔 的限制⽽不能将其移动到young区域头部时，Page made not young的值会加1。 这⾥需要注意，对于处在young区域的节点，如果由于它 在young区域的1/4处⽽导致它没有被移动到young区域头 部，这样的访问并不会将Page made not young的值加1。

youngs/s：代表每秒从old区域被移动到young区域头部的 节点数量。

non-youngs/s：代表每秒由于不满⾜时间限制⽽不能从old 区域移动到young区域头部的节点数量。

Pages read、created、written：代表读取，创建，写⼊ 了多少⻚。后边跟着读取、创建、写⼊的速率。

Buffer pool hit rate：表示在过去某段时间，平均访问 1000次⻚⾯，有多少次该⻚⾯已经被缓存到Buffer Pool 了。

young-making rate：表示在过去某段时间，平均访问 1000次⻚⾯，有多少次访问使⻚⾯移动到young区域的头部 了。 需要⼤家注意的⼀点是，这⾥统计的将⻚⾯移动到young区域 的头部次数不仅仅包含从old区域移动到young区域头部的次 数，还包括从young区域移动到young区域头部的次数（访问 某个young区域的节点，只要该节点在young区域的1/4处往 后，就会把它移动到young区域的头部）。

not (young-making rate)：表示在过去某段时间，平均 访问1000次⻚⾯，有多少次访问没有使⻚⾯移动到young区域 的头部。 需要⼤家注意的⼀点是，这⾥统计的没有将⻚⾯移动到young 区域的头部次数不仅仅包含因为设置了 innodb_old_blocks_time系统变量⽽导致访问了old区域 中的节点但没把它们移动到young区域的次数，还包含因为该 节点在young区域的前1/4处⽽没有被移动到young区域头部 的次数。

LRU len：代表LRU链表中节点的数量。

unzip_LRU：代表unzip_LRU链表中节点的数量（由于我们 没有具体唠叨过这个链表，现在可以忽略它的值）。

I/O sum：最近50s读取磁盘⻚的总数。

I/O cur：现在正在读取的磁盘⻚数量。

I/O unzip sum：最近50s解压的⻚⾯数量。

I/O unzip cur：正在解压的⻚⾯数量。

总结

1. 磁盘太慢，⽤内存作为缓存很有必要。
2. Buffer Pool本质上是InnoDB向操作系统申请的⼀段连续的 内存空间，可以通过innodb_buffer_pool_size来调整它 的⼤⼩。
3. Buffer Pool向操作系统申请的连续内存由控制块和缓存⻚ 组成，每个控制块和缓存⻚都是⼀⼀对应的，在填充⾜够多的 控制块和缓存⻚的组合后，Buffer Pool剩余的空间可能产 ⽣不够填充⼀组控制块和缓存⻚，这部分空间不能被使⽤，也 被称为碎⽚。
4. InnoDB使⽤了许多链表来管理Buffer Pool。
5. free链表中每⼀个节点都代表⼀个空闲的缓存⻚，在将磁盘中 的⻚加载到Buffer Pool时，会从free链表中寻找空闲的缓 存⻚。
6. 为了快速定位某个⻚是否被加载到Buffer Pool，使⽤表空 间号 + ⻚号作为key，缓存⻚作为value，建⽴哈希表。
7. 在Buffer Pool中被修改的⻚称为脏⻚，脏⻚并不是⽴即刷 新，⽽是被加⼊到flush链表中，待之后的某个时刻同步到磁 盘上。
8. LRU链表分为young和old两个区域，可以通过 innodb_old_blocks_pct来调节old区域所占的⽐例。⾸次 从磁盘上加载到Buffer Pool的⻚会被放到old区域的头部， 在innodb_old_blocks_time间隔时间内访问该⻚不会把它 移动到young区域头部。在Buffer Pool没有可⽤的空闲缓 存⻚时，会⾸先淘汰掉old区域的⼀些⻚。
9. 我们可以通过指定innodb_buffer_pool_instances来控 制Buffer Pool实例的个数，每个Buffer Pool实例中都有 各⾃独⽴的链表，互不⼲扰。
10. ⾃MySQL 5.7.5版本之后，可以在服务器运⾏过程中调 整Buffer Pool⼤⼩。每个Buffer Pool实例由若⼲ 个chunk组成，每个chunk的⼤⼩可以在服务器启动时通过启 动参数调整。
11. 可以⽤下边的命令查看Buffer Pool的状态信息：

# 从猫爷被杀说起 —— 事务简介

我们现在知道事务是⼀个抽象的概念，它其实对应着⼀个或多个数据 库操作，设计数据库的⼤叔根据这些操作所执⾏的不同阶段把事务⼤ 致上划分成了这么⼏个状态：

活动的（active） 事务对应的数据库操作正在执⾏过程中时，我们就说该事务处 在活动的状态。

部分提交的（partially committed） 当事务中的最后⼀个操作执⾏完成，但由于操作都在内存中执 ⾏，所造成的影响并没有刷新到磁盘时，我们就说该事务处在 部分提交的状态。

失败的（failed） 当事务处在活动的或者部分提交的状态时，可能遇到了某些错 误（数据库⾃身的错误、操作系统错误或者直接断电等）⽽⽆ 法继续执⾏，或者⼈为的停⽌当前事务的执⾏，我们就说该事 务处在失败的状态。

中⽌的（aborted） 如果事务执⾏了半截⽽变为失败的状态，⽐如我们前边唠叨的 狗哥向猫爷转账的事务，当狗哥账户的钱被扣除，但是猫爷账 户的钱没有增加时遇到了错误，从⽽当前事务处在了失败的状 态，那么就需要把已经修改的狗哥账户余额调整为未转账之前 的⾦额，换句话说，就是要撤销失败事务对当前数据库造成的 影响。书⾯⼀点的话，我们把这个撤销的过程称之为回滚。当 回滚操作执⾏完毕时，也就是数据库恢复到了执⾏事务之前的 状态，我们就说该事务处在了中⽌的状态。

提交的（committed） 当⼀个处在部分提交的状态的事务将修改过的数据都同步到磁 盘上之后，我们就可以说该事务处在了提交的状态。

随着事务对应的数据库操作执⾏到不同阶段，事务的状态也在不断变 化，⼀个基本的状态转换图如下所示：

![image-20211203085445387](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203085455.png)

只有当事务处于提交的或者中⽌的状态 时，⼀个事务的⽣命周期才算是结束了

**不过⽐BEGIN语句⽜逼⼀点⼉的是，可以在START TRANSACTION语句后边跟随⼏个修饰符，就是它们⼏个**：

READ ONLY：标识当前事务是⼀个只读事务，也就是属 于该事务的数据库操作只能读取数据，⽽不能修改数据。

READ WRITE：标识当前事务是⼀个读写事务，也就是属 于该事务的数据库操作既可以读取数据，也可以修改数 据。

WITH CONSISTENT SNAPSHOT：启动⼀致性读（先不 ⽤关⼼啥是个⼀致性读，后边的章节才会唠叨）。

```
START TRANSACTION READ ONLY, WITH CONSISTENT SNAPSHOT;
```

当我们使⽤START TRANSACTION或者BEGIN语句开启了⼀个事 务，或者把系统变量autocommit的值设置为OFF时，事务就不会进 ⾏⾃动提交，但是如果我们输⼊了某些语句之后就会悄悄的提交掉， 就像我们输⼊了COMMIT语句了⼀样，这种因为某些特殊的语句⽽导 致事务提交的情况称为隐式提交，这些会导致事务隐式提交的语句包 括：

- 定义或修改数据库对象的数据定义语⾔（Data definition language，缩写为：DDL）。

所谓的数据库对象，指的就是数据库、表、视图、存储过程等 等这些东⻄。当我们使⽤CREATE、ALTER、DELETE等语句去 修改这些所谓的数据库对象时，就会隐式的提交前边语句所属 于的事务，就像这样：

```
BEGIN;
SELECT ... # 事务中的⼀条语句
UPDATE ... # 事务中的⼀条语句
... # 事务中的其它语句
CREATE TABLE ... # 此语句会隐式的提交前边语句所属于
的事务
```

- 隐式使⽤或修改mysql数据库中的表

隐式使⽤或修改mysql数据库中的表 当我们使⽤ALTER USER、CREATE USER、DROP USER、GRANT、RENAME USER、REVOKE、SET PASSWORD 等语句时也会隐式的提交前边语句所属于的事务。

- 事务控制或关于锁定的语句

当我们在⼀个事务还没提交或者回滚时就⼜使⽤START TRANSACTION或者BEGIN语句开启了另⼀个事务时，会隐式 的提交上⼀个事务，⽐如这样：

```
BEGIN;
SELECT ... # 事务中的⼀条语句
UPDATE ... # 事务中的⼀条语句
... # 事务中的其它语句
BEGIN; # 此语句会隐式的提交前边语句所属于的事务
```

或者当前的autocommit系统变量的值为OFF，我们⼿动把它 调为ON时，也会隐式的提交前边语句所属的事务。

或者使⽤LOCK TABLES、UNLOCK TABLES等关于锁定的语句 也会隐式的提交前边语句所属的事务。

- 加载数据的语句

⽐如我们使⽤LOAD DATA语句来批量往数据库中导⼊数据时， 也会隐式的提交前边语句所属的事务。

- 关于MySQL复制的⼀些语句

使⽤START SLAVE、STOP SLAVE、RESET SLAVE、CHANGE MASTER TO等语句时也会隐式的提交前边 语句所属的事务。

- 其它的⼀些语句

使⽤ANALYZE TABLE、CACHE INDEX、CHECK TABLE、FLUSH、 LOAD INDEX INTO CACHE、OPTIMIZE TABLE、REPAIR TABLE、RESET等语句也会隐式的提交前边 语句所属的事务。

保存点（英 ⽂：savepoint）的概念，就是在事务对应的数据库语句中打⼏个 点，我们在调⽤ROLLBACK语句时可以指定会滚到哪个点，⽽不是回 到最初的原点。

定义保存点的语法如下： SAVEPOINT 保存点名称;

ROLLBACK [WORK] TO [SAVEPOINT] 保存点名称;

删除某个保存点，可以使⽤这个语句： RELEASE SAVEPOINT 保存点名称;

我们只是想让已经提交了的事务对数 据库中数据所做的修改永久⽣效，即使后来系统崩溃，在重启后也能 把这种修改恢复出来。所以我们其实没有必要在每次事务提交时就把 该事务在内存中修改过的全部⻚⾯刷新到磁盘，只需要把修改了哪些 东⻄记录⼀下就好，⽐⽅说某个事务将系统表空间中的第100号⻚⾯ 中偏移量为1000处的那个字节的值1改成2我们只需要记录⼀下： 将第0号表空间的100号⻚⾯的偏移量为1000处的值更 新为2。

这样我们在事务提交时，把上述内容刷新到磁盘中，即使之后系统崩 溃了，重启之后只要按照上述内容所记录的步骤重新更新⼀下数据 ⻚，那么该事务对数据库中所做的修改⼜可以被恢复出来，也就意味 着满⾜持久性的要求。

**redo⽇志占⽤的空间⾮常⼩**

存储表空间ID、⻚号、偏移量以及需要更新的值所需的存储空间是很⼩的，关于redo⽇志的格式我们稍后会详细唠叨，现在只要知道⼀条redo⽇志占⽤的空间不是很⼤就好了。

**redo⽇志是顺序写⼊磁盘的**

在执⾏事务的过程中，每执⾏⼀条语句，就可能产⽣若⼲ 条redo⽇志，这些⽇志是按照产⽣的顺序写⼊磁盘的，也就是 使⽤顺序IO。

**redo⽇志格式**

对事务对数据库的不同修改场景定义了多种类型的redo⽇志，但是绝⼤部分类型的redo ⽇志都有下边这种通⽤的结构：

![image-20211203094733942](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203094735.png)

53种不同的类型

space ID：表空间ID。 page number：⻚号。 data：该条redo⽇志的具体内容。

**row_id隐藏列赋值的⽅式**

服务器会在内存中维护⼀个全局变量，每当向某个包含隐藏的 row_id列的表中插⼊⼀条记录时，就会把该变量的值当作新 记录的row_id列的值，并且把该变量⾃增1。

每当这个变量的值为256的倍数时，就会将该变量的值刷新到 系统表空间的⻚号为7的⻚⾯中⼀个称之为Max Row ID的属 性处（我们前边介绍表空间结构时详细说过）。

当系统启动时，会将上边提到的Max Row ID属性加载到内存 中，将该值加上256之后赋值给我们前边提到的全局变量（因 为在上次关机时该全局变量的值可能⼤于Max Row ID属性 值）。

**redo ⽇志中只需要记录⼀下在某个⻚⾯的某个偏移量处修改了⼏个字节的 值，具体被修改的内容是啥就好了。**

**这种极其 简单的redo⽇志称之为物理⽇志，并且根据在⻚⾯中写⼊数据的多 少划分了⼏种不同的redo⽇志类型：**

MLOG_1BYTE（type字段对应的⼗进制数字为1）：表示在⻚ ⾯的某个偏移量处写⼊1个字节的redo⽇志类型。

MLOG_2BYTE（type字段对应的⼗进制数字为2）：表示在⻚ ⾯的某个偏移量处写⼊2个字节的redo⽇志类型。

MLOG_4BYTE（type字段对应的⼗进制数字为4）：表示在⻚ ⾯的某个偏移量处写⼊4个字节的redo⽇志类型。

MLOG_8BYTE（type字段对应的⼗进制数字为8）：表示在⻚ ⾯的某个偏移量处写⼊8个字节的redo⽇志类型。

MLOG_WRITE_STRING（type字段对应的⼗进制数字 为30）：表示在⻚⾯的某个偏移量处写⼊⼀串数据。

我们上边提到的Max Row ID属性实际占⽤8个字节的存储空间，所 以在修改⻚⾯中的该属性时，会记录⼀条类型为MLOG_8BYTE的 redo⽇志，MLOG_8BYTE的redo⽇志结构如下所示：

![image-20211203095839832](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203095841.png)

其余MLOG_1BYTE、MLOG_2BYTE、MLOG_4BYTE类型的redo⽇志 结构和MLOG_8BYTE的类似，只不过具体数据中包含对应个字节的数 据罢了。MLOG_WRITE_STRING类型的redo⽇志表示写⼊⼀串数 据，但是因为不能确定写⼊的具体数据占⽤多少字节，所以需要在⽇ 志结构中添加⼀个len字段：

![image-20211203095946071](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203095947.png)

以⼀条INSERT语句为例，它除了要向B+树的⻚⾯中插⼊数据，也可 能更新系统数据Max Row ID的值，不过对于我们⽤户来说，平时更 关⼼的是语句对B+树所做更新： 表中包含多少个索引，⼀条INSERT语句就可能更新多少棵 B+树。 针对某⼀棵B+树来说，既可能更新叶⼦节点⻚⾯，也可能更新 内节点⻚⾯，也可能创建新的⻚⾯（在该记录插⼊的叶⼦节点 的剩余空间⽐较少，不⾜以存放该记录时，会进⾏⻚⾯的分 裂，在内节点⻚⾯中添加⽬录项记录）。

**每往叶⼦节点代表的数据⻚⾥插⼊⼀ 条记录时，还有其他很多地⽅会跟着更新，⽐如说：**

可能更新Page Directory中的槽信息。 Page Header中的各种⻚⾯统计信息，⽐如 PAGE_N_DIR_SLOTS表示的槽数量可能会更 改，PAGE_HEAP_TOP代表的还未使⽤的空间最⼩地址可能会 更改，PAGE_N_HEAP代表的本⻚⾯中的记录数量可能会更 改，吧啦吧啦，各种信息都可能会被修改。 我们知道在数据⻚⾥的记录是按照索引列从⼩到⼤的顺序组成 ⼀个单向链表的，每插⼊⼀条记录，还需要更新上⼀条记录的 记录头信息中的next_record属性来维护这个单向链表。

![image-20211203101531252](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203101533.png)

MLOG_REC_INSERT（对应的⼗进制数字为9）：表示插⼊⼀ 条使⽤⾮紧凑⾏格式的记录时的redo⽇志类型。

MLOG_COMP_REC_INSERT（对应的⼗进制数字为38）：表示 插⼊⼀条使⽤紧凑⾏格式的记录时的redo⽇志类型。

⼩贴⼠： Redundant是⼀种⽐较原始的⾏格式，它就是⾮紧凑的。⽽ Compact、Dynamic以及Compressed⾏格式是较新的⾏格式，它 们是紧凑的（占⽤更⼩的存储空间）。

MLOG_COMP_PAGE_CREATE（type字段对应的⼗进制数字 为58）：表示创建⼀个存储紧凑⾏格式记录的⻚⾯的redo⽇ 志类型。

MLOG_COMP_REC_DELETE（type字段对应的⼗进制数字 为42）：表示删除⼀条使⽤紧凑⾏格式记录的redo⽇志类 型。

MLOG_COMP_LIST_START_DELETE（type字段对应的⼗进 制数字为44）：表示从某条给定记录开始删除⻚⾯中的⼀系列 使⽤紧凑⾏格式记录的redo⽇志类型。

MLOG_COMP_LIST_END_DELETE（type字段对应的⼗进制数 字为43）：与MLOG_COMP_LIST_START_DELETE类型的 redo⽇志呼应，表示删除⼀系列记录直 到MLOG_COMP_LIST_END_DELETE类型的redo⽇志对应的记 录为⽌。

**MLOG_COMP_LIST_START_DELETE和 MLOG_COMP_LIST_END_DELETE类型的redo⽇志，可以很⼤程度 上减少redo⽇志的条数。**

MLOG_ZIP_PAGE_COMPRESS（type字段对应的⼗进制数字 为51）：表示压缩⼀个数据⻚的redo⽇志类型。

**这些类型的redo⽇志既包含物理层⾯的意思，也包含逻辑层⾯的意 思，具体指：**

物理层⾯看，这些⽇志都指明了对哪个表空间的哪个⻚进⾏了 修改。

逻辑层⾯看，在系统奔溃重启时，并不能直接根据这些⽇志⾥ 的记载，将⻚⾯内的某个偏移量处恢复成某个数据，⽽是需要 调⽤⼀些事先准备好的函数，执⾏完这些函数后才可以将⻚⾯ 恢复成系统奔溃前的样⼦。

![image-20211203103054795](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203103056.png)

额~ 突然间窗口关闭还未来得及保存~

![image-20211203124703732](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203124705.png)

![image-20211203124648681](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203124651.png)

![image-20211203124750634](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203124752.png)

![image-20211203124831710](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203124833.png)

每个mtr运⾏过程中产⽣的 ⽇志先暂时存到⼀个地⽅，当该mtr结束的时候，将过程中产⽣的⼀ 组redo⽇志再全部复制到log buffer中。

![image-20211203124915408](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203124916.png)

![image-20211203124938284](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203124939.png)

# 说过的话就一定要办到 —— redo 日志（下）

**刷盘时机**

我们前边说mtr运⾏过程中产⽣的⼀组redo⽇志在mtr结束时会被复 制到log buffer中，可是这些⽇志总在内存⾥呆着也不是个办法， 在⼀些情况下它们会被刷新到磁盘⾥，⽐如：

log buffer空间不⾜时 log buffer的⼤⼩是有限的（通过系统变量 innodb_log_buffer_size指定），如果不停的往这个有限 ⼤⼩的log buffer⾥塞⼊⽇志，很快它就会被填满。设计 InnoDB的⼤叔认为如果当前写⼊log buffer的redo⽇志量 已经占满了log buffer总容量的⼤约⼀半左右，就需要把这 些⽇志刷新到磁盘上。

事务提交时 我们前边说过之所以使⽤redo⽇志主要是因为它占⽤的空间 少，还是顺序写，在事务提交时可以不把修改过的Buffer Pool⻚⾯刷新到磁盘，但是为了保证持久性，必须要把修改这 些⻚⾯对应的redo⽇志刷新到磁盘。

Force Log at Commit 后台线程不停的刷刷刷

![image-20211203134656744](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203134658.png)

我们前边说过log buffer本质上是⼀⽚连续的内存空间，被划分成 了若⼲个512字节⼤⼩的block。将log buffer中的redo⽇志刷新到 磁盘的本质就是把block的镜像写⼊⽇志⽂件中，所以redo⽇志⽂件 其实也是由若⼲个512字节⼤⼩的block组成。

![image-20211203134916251](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203134917.png)

![image-20211203135036955](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203135038.png)

![image-20211203135106924](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203135108.png)

![image-20211203184159703](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203184208.png)

![image-20211203184229848](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203184231.png)

![image-20211203184245127](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203184246.png)

![image-20211203184259211](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203184300.png)

![image-20211203184357992](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205141847.png))![image-20211203184443499](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203184445.png)

**每⼀组由mtr⽣成的redo⽇志都有⼀个 唯⼀的LSN值与其对应，LSN值越⼩，说明redo⽇志产⽣的越早。**

**我们前边说lsn是表示当前系统中写⼊的redo⽇志量，这包括了写 到log buffer⽽没有刷新到磁盘的⽇志，相应的，设计InnoDB的 ⼤叔提出了⼀个表示刷新到磁盘中的redo⽇志量的全局变量，称之 为flushed_to_disk_lsn。**

![image-20211203185452431](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205141859.png)

![image-20211203185252487](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205141902.png)

![image-20211203185504894](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203185507.png)

综上所述，当有新的redo⽇志写⼊到log buffer时，⾸先lsn的 值会增⻓，但flushed_to_disk_lsn不变，随后随着不断有log buffer中的⽇志被刷新到磁盘上，flushed_to_disk_lsn的值也 跟着增⻓。如果两者的值相同时，说明log buffer中的所有redo⽇志 都已经刷新到磁盘中了。

如果某个写⼊操作要等到操作系统确认已经写到磁盘时才返回，那 需要调⽤⼀下操作系统提供的fsync函数。

因为lsn的值是代表系统写⼊的redo⽇志量的⼀个总和，⼀个mtr中 产⽣多少⽇志，lsn的值就增加多少（当然有时候要加上log block header和log block trailer的⼤⼩），这样mtr产⽣ 的⽇志写到磁盘中时，很容易计算某⼀个lsn值在redo⽇志⽂件组 中的偏移量，如图：

![image-20211203185835487](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203185836.png)

在mtr结束时，会把这⼀组 redo⽇志写⼊到log buffer中。除此之外，在mtr结束时还有⼀ 件⾮常重要的事情要做，就是把在mtr执⾏过程中可能修改过的⻚⾯ 加⼊到Buffer Pool的flush链表。

![image-20211203190254351](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203190255.png)

说flush链表中 的脏⻚是按照⻚⾯的第⼀次修改时间从⼤到⼩进⾏排序的。在这个过 程中会在缓存⻚对应的控制块中记录两个关于⻚⾯何时修改的属性：

oldest_modification：如果某个⻚⾯被加载到Buffer Pool后进⾏第⼀次修改，那么就将修改该⻚⾯的mtr开始时对 应的lsn值写⼊这个属性。

newest_modification：每修改⼀次⻚⾯，都会将修改该⻚ ⾯的mtr结束时对应的lsn值写⼊这个属性。也就是说该属性 表示⻚⾯最近⼀次修改后对应的系统lsn值。

![image-20211203190916691](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203190918.png)

![image-20211203192545249](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203192546.png)

![image-20211203192803021](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203192804.png)

**redo ⽇志只是为了系统奔溃后恢复脏⻚⽤的，如果对应的脏⻚已经刷新到 了磁盘，也就是说即使现在系统奔溃，那么在重启后也⽤不着使⽤ redo⽇志恢复该⻚⾯了，所以该redo⽇志也就没有存在的必要了， 那么它占⽤的磁盘空间就可以被后续的redo⽇志所重⽤。也就是 说：判断某些redo⽇志占⽤的磁盘空间是否可以覆盖的依据就是它 对应的脏⻚是否已经刷新到磁盘⾥。**

![image-20211203193143321](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203193144.png)

![image-20211203193400812](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203193402.png)

**如图，虽然mtr_1和mtr_2⽣成的redo⽇志都已经被写到了磁盘 上，但是它们修改的脏⻚仍然留在Buffer Pool中，所以它们⽣成 的redo⽇志在磁盘上的空间是不可以被覆盖的。之后随着系统的运 ⾏，如果⻚a被刷新到了磁盘，那么它对应的控制块就会从flush链 表中移除，就像这样⼦**

![image-20211203193504260](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203193505.png)

**这样mtr_1⽣成的redo⽇志就没有⽤了，它们占⽤的磁盘空间就可 以被覆盖掉了。设计InnoDB的⼤叔提出了⼀个全局变量 checkpoint_lsn来代表当前系统中可以被覆盖的redo⽇志总量是 多少，这个变量初始值也是8704。**

**⽐⽅说现在⻚a被刷新到了磁盘，mtr_1⽣成的redo⽇志就可以被覆 盖了，所以我们需要进⾏⼀个增加checkpoint_lsn的操作，我们 把这个过程称之为做⼀次checkpoint。做⼀次checkpoint其实可 以分为两个步骤**

步骤⼀：计算⼀下当前系统中可以被覆盖的redo⽇志对应的 lsn值最⼤是多少。

redo⽇志可以被覆盖，意味着它对应的脏⻚被刷到了磁盘，只 要我们计算出当前系统中被最早修改的脏⻚对应的 oldest_modification值，那凡是在系统lsn值⼩于该节点 的oldest_modification值时产⽣的redo⽇志都是可以被覆盖 掉的，我们就把该脏⻚的oldest_modification赋值给 checkpoint_lsn。

⽐⽅说当前系统中⻚a已经被刷新到磁盘，那么flush链表的 尾节点就是⻚c，该节点就是当前系统中最早修改的脏⻚了， 它的oldest_modification值为8916，我们就把8404赋值 给checkpoint_lsn（也就是说在redo⽇志对应的lsn值⼩于 8916时就可以被覆盖掉）。

![image-20211203195135872](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203195138.png)

批量从flush链表中刷出脏⻚ 我们在介绍Buffer Pool的时候说过，⼀般情况下都是后台的线程 在对LRU链表和flush链表进⾏刷脏操作，这主要因为刷脏操作⽐较 慢，不想影响⽤户线程处理请求。但是如果当前系统修改⻚⾯的操作 ⼗分频繁，这样就导致写⽇志操作⼗分频繁，系统lsn值增⻓过快。 如果后台的刷脏操作不能将脏⻚刷出，那么系统⽆法及时 做checkpoint，可能就需要⽤户线程同步的从flush链表中把那些 最早修改的脏⻚（oldest_modification最⼩的脏⻚）刷新到磁 盘，这样这些脏⻚对应的redo⽇志就没⽤了，然后就可以去 做checkpoint了。

```
mysql> SHOW ENGINE INNODB STATUS\G
(...省略前边的许多状态)
LOG
---
Log sequence number 124476971
Log flushed up to 124099769
Pages flushed up to 124052503
Last checkpoint at 124052494
0 pending log flushes, 0 pending chkp writes
24 log i/o's done, 2.00 log i/o's/second
----------------------
(...省略后边的许多状态
```

其中：

Log sequence number：代表系统中的lsn值，也就是当前 系统已经写⼊的redo⽇志量，包括写⼊log buffer中的⽇ 志。

Log flushed up to：代表flushed_to_disk_lsn的 值，也就是当前系统已经写⼊磁盘的redo⽇志量。

Pages flushed up to：代表flush链表中被最早修改的那 个⻚⾯对应的oldest_modification属性值。

Last checkpoint at：当前系统的checkpoint_lsn值。

**innodb_flush_log_at_trx_commit的⽤法**

如果有的同学对事务的持久性 要求不是那么强烈的话，可以选择修改⼀个称 为innodb_flush_log_at_trx_commit的系统变量的值，该变量 有3个可选的值：

0：当该系统变量值为0时，表示在事务提交时不⽴即向磁盘中 同步redo⽇志，这个任务是交给后台线程做的。 这样很明显会加快请求处理速度，但是如果事务提交后服务器 挂了，后台线程没有及时将redo⽇志刷新到磁盘，那么该事务 对⻚⾯的修改会丢失。

1：当该系统变量值为0时，表示在事务提交时需要将redo⽇ 志同步到磁盘，可以保证事务的持久性。1也 是innodb_flush_log_at_trx_commit的默认值。

2：当该系统变量值为0时，表示在事务提交时需要将redo⽇ 志写到操作系统的缓冲区中，但并不需要保证将⽇志真正的刷 新到磁盘。 这种情况下如果数据库挂了，操作系统没挂的话，事务的持久 性还是可以保证的，但是操作系统也挂了的话，那就不能保证 持久性了。

**checkpoint_lsn之前的redo⽇志都可以被覆盖， 也就是说这些redo⽇志对应的脏⻚都已经被刷新到磁盘中了，既然 它们已经被刷盘，我们就没必要恢复它们了。对于 checkpoint_lsn之后的redo⽇志，它们对应的脏⻚可能没被刷 盘，也可能被刷盘了，我们不能确定，所以需要从 checkpoint_lsn开始读取redo⽇志来恢复⻚⾯。**

确定恢复的起点

redo⽇志⽂件组的第⼀个⽂件的管理信息中有两个block都存 储了checkpoint_lsn的信息，我们当然是要选取最近发⽣的那次 checkpoint的信息。衡量checkpoint发⽣时间早晚的信息就是所 谓的checkpoint_no，我们只要把checkpoint1和checkpoint2 这两个block中的checkpoint_no值读出来⽐⼀下⼤⼩，哪个的 checkpoint_no值更⼤，说明哪个block存储的就是最近的⼀ 次checkpoint信息。**这样我们就能拿到最近发⽣的checkpoint对应的checkpoint_lsn值以及它在redo⽇志⽂件组中的偏移量 checkpoint_offset。**

确定恢复的终点

![image-20211203203812774](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203203814.png)

怎么恢复

![image-20211203204042433](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203204043.png)

![image-20211203204259593](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211203204300.png)

**之后就可以遍历哈希表，因为对同⼀个⻚⾯进⾏修改的redo⽇ 志都放在了⼀个槽⾥，所以可以⼀次性将⼀个⻚⾯修复好（避 免了很多读取⻚⾯的随机IO），这样可以加快恢复速度。另外 需要注意⼀点的是，同⼀个⻚⾯的redo⽇志是按照⽣成时间顺 序进⾏排序的，所以恢复的时候也是按照这个顺序进⾏恢复， 如果不按照⽣成时间顺序进⾏排序的话，那么可能出现错误。 ⽐如原先的修改操作是先插⼊⼀条记录，再删除该条记录，如 果恢复时不按照这个顺序来，就可能变成先删除⼀条记录，再 插⼊⼀条记录，这显然是错误的。**

> **跳过已经刷新到磁盘的⻚⾯ 我们前边说过，checkpoint_lsn之前的redo⽇志对应的脏 ⻚确定都已经刷到磁盘了，但是checkpoint_lsn之后的 redo⽇志我们不能确定是否已经刷到磁盘，主要是因为在最近 做的⼀次checkpoint后，可能后台线程⼜不断的从LRU链表 和flush链表中将⼀些脏⻚刷出Buffer Pool。这些 在checkpoint_lsn之后的redo⽇志，如果它们对应的脏⻚ 在奔溃发⽣时已经刷新到磁盘，那在恢复时也就没有必要根 据redo⽇志的内容修改该⻚⾯了。 那在恢复时怎么知道某个redo⽇志对应的脏⻚是否在奔溃发⽣时已经刷新到磁盘了呢？这还得从⻚⾯的结构说起，我们前边 说过每个⻚⾯都有⼀个称之为File Header的部分，在File Header⾥有⼀个称之为FIL_PAGE_LSN的属性，该属性记载 了最近⼀次修改⻚⾯时对应的lsn值（其实就是⻚⾯控制块中 的newest_modification值）。如果在做了某 次checkpoint之后有脏⻚被刷新到磁盘中，那么该⻚对应的 FIL_PAGE_LSN代表的lsn值肯定⼤于checkpoint_lsn的 值，凡是符合这种情况的⻚⾯就不需要做恢复操作了，所以更 进⼀步提升了奔溃恢复的速度。**
>
> 上面这段话巨难理解

遗漏的问题：LOG_BLOCK_HDR_NO是如何 计算的

我们前边说过，对于实际存储redo⽇志的普通的log block来说， 在log block header处有⼀个称之为LOG_BLOCK_HDR_NO的属 性（忘记了的话回头再看看哈），我们说这个属性代表⼀个唯⼀的标 号。这个属性是初次使⽤该block时分配的，跟当时的系统lsn值有 关。使⽤下边的公式计算该block的LOG_BLOCK_HDR_NO值：

((lsn / 512) & 0x3FFFFFFFUL) + 1

这个公式⾥的0x3FFFFFFFUL可能让⼤家有点困惑，其实它的⼆进 制表示可能更亲切⼀点：

从图中可以看出，0x3FFFFFFFUL对应的⼆进制数的前2位为0，后 30位的值都为1。我们刚开始学计算机的时候就学过，⼀个⼆进制位 与0做与运算（&）的结果肯定是0，⼀个⼆进制位与1做与运算 （&）的结果就是原值。让⼀个数和0x3FFFFFFFUL做与运算的意思 就是要将该值的前2个⽐特位的值置为0，这样该值就肯定⼩于或等 于0x3FFFFFFFUL了。这也就说明了，不论lsn多⼤，((lsn / 512) & 0x3FFFFFFFUL)的值肯定在0~~0x3FFFFFFFUL之间，再 加1的话肯定在1~~0x40000000UL之间。⽽0x40000000UL这个值 ⼤家应该很熟悉，这个值就代表着1GB。也就是说系统最多能产⽣不 重复的LOG_BLOCK_HDR_NO值只有1GB个。设计InnoDB的⼤叔规定 redo⽇志⽂件组中包含的所有⽂件⼤⼩总和不得超过512GB，⼀个 block⼤⼩是512字节，也就是说redo⽇志⽂件组中包含的block块 最多为1GB个，所以有1GB个不重复的编号值也就够⽤了。

另外，LOG_BLOCK_HDR_NO值的第⼀个⽐特位⽐较特殊，称之 为flush bit，如果该值为1，代表着本block是在某次将log buffer中的block刷新到磁盘的操作中的第⼀个被刷⼊的block。

# 后悔了怎么办 —— undo 日志（上）

事务回滚的需求

- 情景1：事务执行到一半，出现错误
- 情景2：手动回滚
- 所以需要让事务看起来什么都没做，就需要进行回滚

回滚之前的操作

- 插入需要记录主键值，才能回滚删除
- 删除需要保存记录的内容，才能回滚插入
- 修改需要记录旧值

记录这些东西的就是undo log，回滚日志

![image-20211205092803111](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205092811.png)

![image-20211205092820702](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205092822.png)

![image-20211205092837740](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205092839.png)

![image-20211205092847619](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205092849.png)

![image-20211205092858764](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205092900.png)

![image-20211205092908373](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205092909.png)

![image-20211205092938764](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205092940.png)

![image-20211205092950265](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205092951.png)

![image-20211205093019704](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205093021.png)

![image-20211205093036167](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205093037.png)

在不更新主键的情况下，又可以细分为被更新的列占用的存储空间不发生变化和发生变化的情况。 

- 就地更新（in-place update）
- 先删除掉旧记录，再插入新记录 

在不更新主键的情况下，如果有任何一个被更新的列更新前和更新后占 用的存储空间大小不一致，那么就需要先把这条旧的记录从聚簇索引页 面中删除掉，然后再根据更新后列的值创建一条新的记录插入到页面 中。 UPDATE undo_demo SET key1 = 'P92', col = '手枪' WHERE id = 2; 1 2 3 UPDATE undo_demo SET key1 = 'M249', col = '机枪' WHERE id = 2; 1 2 3 请注意一下，我们这里所说的删除并不是delete mark操作，而是真正的 删除掉，也就是把这条记录从正常记录链表中移除并加入到垃圾链表中， 并且修改页面中相应的统计信息（比如PAGE_FREE、PAGE_GARBAGE等这些 信息）。不过这里做真正删除操作的线程并不是在唠叨DELETE语句中做 purge操作时使用的另外专门的线程，而是由用户线程同步执行真正的 删除操作，真正删除之后紧接着就要根据各个列更新后的值创建的新记 录插入。 这里如果新创建的记录占用的存储空间大小不超过旧记录占用的空间， 那么可以直接重用被加入到垃圾链表中的旧记录所占用的存储空间，否 则的话需要在页面中新申请一段空间以供新记录使用，如果本页面内已 经没有可用的空间的话，那就需要进行页面分裂操作，然后再插入新记 录。

delete mark操作对应的undo日志的结构就是这样：

![image-20211205093050119](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205093051.png)

针对UPDATE不更新主键的情况

![image-20211205093440402](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205093442.png)

![image-20211205093538234](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205093539.png)

更新主键的情况

在聚簇索引中，记录是按照主键值的大小连成了一个单向链表的，如果我 们更新了某条记录的主键值，意味着这条记录在聚簇索引中的位置将会发 生改变，比如你将记录的主键值从1更新为10000，如果还有非常多的记录 的主键值分布在1 ~ 10000之间的话，那么这两条记录在聚簇索引中就有可 能离得非常远，甚至中间隔了好多个页面。针对UPDATE语句中更新了记录主键值的这种情况，InnoDB在聚簇索引中分了两步处理： 

将旧记录进行delete mark操作高能注意：这里是**delete mark操作**！这里是delete mark操作！这里是 delete mark操作！也就是说在UPDATE语句所在的事务提交前，对旧记录 只做一个delete mark操作，在事务提交后才由专门的线程做purge操 作，把它加入到垃圾链表中。这里一定要和我们上边所说的在不更新记 录主键值时，先真正删除旧记录，再插入新记录的方式区分开！ 小贴士： 之所以只对旧记录做delete mark操作，是因为别的事务同时 也可能访问这条记录，如果把它真正的删除加入到垃圾链表后，别的 事务就访问不到了。这个功能就是所谓的MVCC，我们后边的章节中 会详细唠叨什么是个MVCC。 

根据更新后各列的值创建一条新记录，并将其插入到聚簇索引中（需重 新定位插入的位置）。 由于更新后的记录主键值发生了改变，所以需要重新从聚簇索引中定位 这条记录所在的位置，然后把它插进去。 针对UPDATE语句更新记录主键值的这种情况，在对该记录进行delete mark 操作前，会记录一条类型为TRX_UNDO_DEL_MARK_REC的undo日志；之后插入新记录时，会记录一条类型为TRX_UNDO_INSERT_REC的undo日志，也就是说 每对一条记录的主键值做改动时，会记录2条undo日志。这些日志的格式我 们上边都唠叨过了，就不赘述了。

# 后悔了怎么办 —— undo 日志（下）

![image-20211205094338543](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205094340.png)

![image-20211205094457191](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205094458.png)

![image-20211205094509155](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205094510.png)

![image-20211205094532681](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205094534.png)

![image-20211205094602947](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205094604.png)

**TRX_UNDO_PAGE_TYPE**：本页面准备存储什么种类的undo日志。

- TRX_UNDO_INSERT 
- TRX_UNDO_UPDATE

不同大类的undo日志不能混着存储， 比如一个Undo页面的TRX_UNDO_PAGE_TYPE属性值为TRX_UNDO_INSERT，那 么这个页面就只能存储类型为TRX_UNDO_INSERT_REC的undo日志，其他类 型的undo日志就不能放到这个页面中了。

之 所 以 把 undo 日 志 分 成 两 个 大 类 ， 是 因 为 类 型 为 TRX_UNDO_INSERT_REC的undo日志在事务提交后可以直接删除 掉，而其他类型的undo日志还需要为所谓的MVCC服务，不能直接删 除掉，对它们的处理需要区别对待。

**TRX_UNDO_PAGE_START**：表示在当前页面中是从什么位置开始存储undo日 志的，或者说表示第一条undo日志在本页面中的起始偏移量。 

**TRX_UNDO_PAGE_FREE**：与上边的TRX_UNDO_PAGE_START对应，表示当前页 面中存储的最后一条undo日志结束时的偏移量，或者说从这个位置开 始，可以继续写入新的undo日志。

![image-20211205095039563](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205095041.png)

![image-20211205095120719](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205095122.png)

在first undo page中除了记录Undo Page Header之外，还 会记录其他的一些管理信息

同一个 Undo 页 面 要 么 只 存 储 TRX_UNDO_INSERT 大 类 的 undo 日 志 ， 要 么 只 存 储 TRX_UNDO_UPDATE大类的undo日志，反正不能混着存，所以在一个事务执行 过程中就可能需要2个Undo页面的链表，一个称之为insert undo链表，另一 个称之为update undo链表，画个示意图就是这样：

![image-20211205095303270](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205095305.png)

![image-20211205095315757](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205095317.png)

**多个事务中的Undo页面链表**

![image-20211205095405225](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205142003.png)

**undo日志具体写入过程**

段是一个逻辑上 的概念，本质上是由若干个零散页面和若干个完整的区组成的。

INODE Entry结构描述了这个段的各种信息，比如段的ID，段内的各种链表基节点，零散页面的页号有哪些 等信息

Segment Header是来定位INODE Entry：

![image-20211205095631991](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205142009.png)

段：Undo Log Segment

链表中的页面都是从这个段里边申请的

在Undo页面链表的第一个页面，也就是上边提到的first undo page中设计 了一个称之为Undo Log Segment Header的部分，这个部分中包含了该链表 对应的段的segment header信息以及其他的一些关于这个段的信息，所以 Undo页面链表的第一个页面其实长这样：

![image-20211205095803332](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205095804.png)

![image-20211205100111686](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205100113.png)

**TRX_UNDO_STATE：本Undo页面链表处在什么状态。**

**TRX_UNDO_ACTIVE**：活跃状态，也就是一个活跃的事务正在往这个段 里边写入undo日志。 

**TRX_UNDO_CACHED**：被缓存的状态。处在该状态的Undo页面链表等待着 之后被其他事务重用。 

**TRX_UNDO_TO_FREE**：对于insert undo链表来说，如果在它对应的事 务提交之后，该链表不能被重用，那么就会处于这种状态。 

**TRX_UNDO_TO_PURGE**：对于update undo链表来说，如果在它对应的事 务提交之后，该链表不能被重用，那么就会处于这种状态。 TRX_UNDO_PREPARED：包含处于PREPARE阶段的事务产生的undo日志。

**TRX_UNDO_PREPARED**：包含处于PREPARE阶段的事务产生的undo日志。事务的PREPARE阶段是在所谓的分布式事务中才出现的

**TRX_UNDO_LAST_LOG：本Undo页面链表中最后一个Undo Log Header的位 置。**

**TRX_UNDO_FSEG_HEADER：本Undo页面链表对应的段的Segment Header信 息（就是我们上一节介绍的那个10字节结构，通过这个信息可以找到该 段对应的INODE Entry）。**

**TRX_UNDO_PAGE_LIST：Undo页面链表的基节点。**

![image-20211205100711336](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205100712.png)

![image-20211205100744167](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205100745.png)

TRX_UNDO_DICT_TRANS：标记本组undo日志是不是由DDL语句产生的。

TRX_UNDO_TABLE_ID：如果TRX_UNDO_DICT_TRANS为真，那么本属性表示 DDL语句操作的表的table id。

TRX_UNDO_HISTORY_NODE：一个12字节的List Node结构，代表一个称之 为History链表的节点。

![image-20211205101323953](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205101325.png)

**重用Undo页面**

一个Undo页面链表是否可以被重用的条件很简单：

该链表中只包含一个Undo页面。 如果一个事务执行过程中产生了非常多的undo日志，那么它可能申请非 常多的页面加入到Undo页面链表中。在该事物提交后，如果将整个链表 中的页面都重用，那就意味着即使新的事务并没有向该Undo页面链表中 写入很多undo日志，那该链表中也得维护非常多的页面，那些用不到的 页面也不能被别的事务所使用，这样就造成了另一种浪费。所以设计 InnoDB的大叔们规定，只有在Undo页面链表中只包含一个Undo页面时， 该链表才可以被下一个事务所重用。

该Undo页面已经使用的空间小于整个页面空间的3/4。

**insert undo链表**

![image-20211205101645615](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205101646.png)

在重用Undo页面链表写入新的一组undo日志时，不 仅会写入新的Undo Log Header，还会适当调整Undo Page Header、 Undo Log Segment Header、Undo Log Header中的一些属性，比如 TRX_UNDO_PAGE_START、TRX_UNDO_PAGE_FREE等等等等。

**update undo链表** 

在一个事务提交后，它的update undo链表中的undo日志也不能立即删除 掉（这些日志用于MVCC，我们后边会说的）。所以如果之后的事务想 重用update undo链表时，就不能覆盖之前事务写入的undo日志。这样就 相当于在同一个Undo页面中写入了多组的undo日志，效果看起来就是这 样：

![image-20211205101821580](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205142053.png)

为了更好的管理这些链表，设 计InnoDB的大叔又设计了一个称之为**Rollback Segment Header的页面**，在 这个页面中存放了各个Undo页面链表的frist undo page的页号，他们把这 些页号称之为undo slot。

![image-20211205102013142](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205102014.png)

每一个Rollback Segment Header页面都对应着一 个段，这个段就称为Rollback Segment，翻译过来就是回滚段。为了某个目的去分 配页面的话都得先申请一个段。Rollback Segment里其实只有一个页面。

TRX_RSEG_MAX_SIZE：本Rollback Segment中管理的所有Undo页面链表中 的Undo页面数量之和的最大值。换句话说，本Rollback Segment中所有 Undo页面链表中的Undo页面数量之和不能超过TRX_RSEG_MAX_SIZE代表的 值。 该属性的值默认为无限大，也就是我们想写多少Undo页面都可以。 

小贴士： 无限大其实也只是个夸张的说法，4个字节能表示最大的数 也就是0xFFFFFFFF，但是我们之后会看到，0xFFFFFFFF这个数有特 殊用途，所以实际上TRX_RSEG_MAX_SIZE的值为0xFFFFFFFE。

TRX_RSEG_HISTORY_SIZE：History链表占用的页面数量。

TRX_RSEG_HISTORY：History链表的基节点。

TRX_RSEG_FSEG_HEADER ： 本 Rollback Segment 对 应 的 10 字 节 大 小 的 Segment Header结构，通过它可以找到本段对应的INODE Entry。

TRX_RSEG_UNDO_SLOTS：各个Undo页面链表的first undo page的页号集 合，也就是undo slot集合。

一 个 页 号 占 用 4 个 字 节 ， 对 于 16KB 大 小 的 页 面 来 说 ， 这 个 TRX_RSEG_UNDO_SLOTS部分共存储了1024个undo slot，所以共需1024 × 4 = 4096个字节。

**从回滚段中申请Undo页面链表**

如果是FIL_NULL，那么在表空间中新创建一个段（也就是Undo Log Segment），然后从段里申请一个页面作为Undo页面链表的first undo page，然后把该undo slot的值设置为刚刚申请的这个页面的地址，这样 也就意味着这个undo slot被分配给了这个事务。

如果不是FIL_NULL，说明该undo slot已经指向了一个undo链表，也就是 说这个undo slot已经被别的事务占用了，那就跳到下一个undo slot， 判断该undo slot的值是不是FIL_NULL，重复上边的步骤。

这1024个undo slot都已经 名花有主（被分配给了某个事务），此时由于新事务无法再获得新的Undo 页面链表，就会回滚这个事务并且给用户报错： Too many active concurrent transactions

用户看到这个错误，可以选择重新执行这个事务（可能重新执行时有别的 事务提交了，该事务就可以被分配Undo页面链表了）。

**如果该undo slot指向的Undo页面链表符合被重用的条件（就是我们上边 说的Undo页面链表只占用一个页面并且已使用空间小于整个页面的 3/4）。**

该undo slot就处于被缓存的状态，设计InnoDB的大叔规定这时该Undo页 面链表的TRX_UNDO_STATE属性（该属性在first undo page的Undo Log Segment Header部分）会被设置为TRX_UNDO_CACHED。

被缓存的undo slot都会被加入到一个链表，根据对应的Undo页面链表的 类型不同，也会被加入到不同的链表： 如果对应的Undo页面链表是insert undo链表，则该undo slot会被加 入insert undo cached链表。 如果对应的Undo页面链表是update undo链表，则该undo slot会被加 入update undo cached链表。 一个回滚段就对应着上述两个cached链表，如果有新事务要分配undo slot时，先从对应的cached链表中找。如果没有被缓存的undo slot，才 会到回滚段的Rollback Segment Header页面中再去找。

如果该undo slot指向的Undo页面链表不符合被重用的条件，那么针对该 undo slot对应的Undo页面链表类型不同，也会有不同的处理： 如果对应的Undo页面链表是insert undo链表，则该Undo页面链表的 TRX_UNDO_STATE属性会被设置为TRX_UNDO_TO_FREE，之后该Undo页面 链表对应的段会被释放掉（也就意味着段中的页面可以被挪作他 用），然后把该undo slot的值设置为FIL_NULL。 如果对应的Undo页面链表是update undo链表，则该Undo页面链表的 TRX_UNDO_STATE属性会被设置为TRX_UNDO_TO_PRUGE，则会将该undo slot的值设置为FIL_NULL，然后将本次事务写入的一组undo日志放到 所谓的History链表中（需要注意的是，这里并不会将Undo页面链表对 应的段给释放掉，因为这些undo日志还有用呢～）。

1024个undo slot也只 能支持1024个读写事务同时执行，再多了就崩溃了。

设计InnoDB的大叔一口气定义了128个回滚段，也就相当 于有了128 × 1024 = 131072个undo slot。

只有在事务执行过程中对记录做了某 些改动时才会被升级为读写事务。

**每个回滚段都对应着一个Rollback Segment Header页面，有128个回滚 段，自然就要有128个Rollback Segment Header页面，这些页面的地址总 得找个地方存一下吧！于是设计InnoDB的大叔在系统表空间的第5号页面的 某个区域包含了128个8字节大小的格子：**

![image-20211205103139506](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205103141.png)

每个8字节的各自的构造就像这样

![image-20211205103150711](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205103152.png)

![image-20211205103240240](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205103241.png)

回滚段的分类 

我们把这128个回滚段给编一下号，最开始的回滚段称之为第0号回滚段，之 后依次递增，最后一个回滚段就称之为第127号回滚段。

这128个回滚段可以被分成两大类： 第0号、第33～127号回滚段属于一类。**其中第0号回滚段必须在系统表空间中（就是说第0号回滚段对应的Rollback Segment Header页面必须在系统表空间中）**，第33～127号回滚段既可以在系统表空间中，也可以在 自己配置的undo表空间中，关于怎么配置我们稍后再说。 如果一个事务在执行过程中由于对普通表的记录做了改动需要分配Undo 页面链表时，必须从这一类的段中分配相应的undo slot。 

第1～32号回滚段属于一类。**这些回滚段必须在临时表空间**（对应着数据 目录中的ibtmp1文件）中。 如果一个事务在执行过程中由于对临时表的记录做了改动需要分配Undo 页面链表时，必须从这一类的段中分配相应的undo slot。

**也就是说如果一个事务在执行过程中既对普通表的记录做了改动，又对临 时表的记录做了改动，那么需要为这个记录分配2个回滚段，再分别到这两 个回滚段中分配对应的undo slot。**

我们向Undo页面写入undo日志本身也是 一个写页面的过程。也就是说我们对Undo页面做的任何改动都会记录相应类型的 redo日志。但是对于临时表来说，因为修改临时表而产生的undo日志只需要 在系统运行过程中有效，如果系统奔溃了，那么在重启时也不需要恢复这 些undo日志所在的页面，所以在写针对临时表的Undo页面时，并不需要记 录相应的redo日志。

**在修改针对普通表的回滚段中的Undo页面时，需要记录对应的redo 日志，而修改针对临时表的回滚段中的Undo页面时，不需要记录对应的 redo日志**

并发执行的 不同事务其实也可以被分配相同的回滚段，只要分配不同的undo slot就可 以了。

我们前边说系统中一共有128个回滚段，其实这只是默认值，我们可以通过 启动参数innodb_rollback_segments来配置回滚段的数量，可配置的范围 是1~128。但是这个参数并不会影响针对临时表的回滚段数量，针对临时表 的回滚段数量一直是32，也就是说：

如果我们把innodb_rollback_segments的值设置为1，那么只会有1个针 对普通表的可用回滚段，但是仍然有32个针对临时表的可用回滚段。 

如果我们把innodb_rollback_segments的值设置为2～33之间的数， 效果和将其设置为1是一样的。 

如果我们把innodb_rollback_segments设置为大于33的数，那么针对普 通表的可用回滚段数量就是该值减去32。

配置undo表空间 

默认情况下，针对普通表设立的回滚段（第0号以及第33~127号回滚段）都 是被分配到系统表空间的。其中的第第0号回滚段是一直在系统表空间的， 但是第33~127号回滚段可以通过配置放到自定义的undo表空间中。但是这种 配置只能在系统初始化（创建数据目录时）的时候使用，一旦初始化完 成，之后就不能再次更改了。

我们看一下相关启动参数： 通过innodb_undo_directory指定undo表空间所在的目录，如果没有指定 该参数，则默认undo表空间所在的目录就是数据目录。 通过innodb_undo_tablespaces定义undo表空间的数量。该参数的默认值 为0，表明不创建任何undo表空间。 

第33~127号回滚段可以平均分布到不同的undo表空间中。 

小贴士： 如果我们在系统初始化的时候指定了创建了undo表空间，那 么系统表空间中的第0号回滚段将处于不可用状态。 比 如 我 们 在 系 统 初 始 化 时 指 定 的 innodb_rollback_segments 为 35 ， innodb_undo_tablespaces为2，这样就会将第33、34号回滚段分别分布到 一个undo表空间中。 

设立undo表空间的一个好处就是在undo表空间中的文件大到一定程度时，可 以自动的将该undo表空间截断（truncate）成一个小文件。而系统表空间的 大小只能不断的增大，却不能截断

# 一条记录的多幅面孔 —— 事务的隔离级别与MVCC

Oracle就只支持READ COMMITTED和SERIALIZABLE隔离级别。本书中所讨论 的MySQL虽然支持4种隔离级别，但与SQL标准中所规定的各级隔离级别允许 发生的问题却有些出入，MySQL在REPEATABLE READ隔离级别下，是可 以禁止幻读问题的发生的

MySQL的默认隔离级别为REPEATABLE READ，我们可以手动修改一下事务的 隔离级别

SET [GLOBAL|SESSION] TRANSACTION ISOLATION LEVEL level;

**SET GLOBAL TRANSACTION ISOLATION LEVEL SERIALIZABLE;**

只对执行完该语句之后产生的会话起作用。 当前已经存在的会话无效。

**SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE;**

对当前会话的所有后续的事务有效 ；该语句可以在已经开启的事务中间执行，但不会影响当前正在执行的 事务。 如果在事务之间执行，则对后续的事务有效。

**SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;**

只对当前会话中下一个即将开启的事务有效。 下一个事务执行完后，后续事务将恢复到之前的隔离级别。 该语句不能在已经开启的事务中间执行，会报错的。

如果我们在服务器启动时想改变事务的默认隔离级别，可以修改启动参数 transaction-isolation 的 值 ， 比 方 说 我 们 在 启 动 服 务 器 时 指 定 了 -- transaction-isolation=SERIALIZABLE，那么事务的默认隔离级别就从原来 的REPEATABLE READ变成了SERIALIZABLE。

**MVCC原理 版本链**

trx_id：每次一个事务对某条聚簇索引记录进行改动时，都会把该事务 的事务id赋值给trx_id隐藏列。 roll_pointer：每次对某条聚簇索引记录进行改动时，都会把旧的版本 写入到undo日志中，然后这个隐藏列就相当于一个指针，可以通过它来 找到该记录修改前的信息。

实际上insert undo只在事务回滚时起作用，当事务提交后， 该类型的undo日志就没用了，它占用的Undo Log Segment也会被系统 回收（也就是该undo日志占用的Undo页面链表要么被重用，要么被释 放）。虽然真正的insert undo日志占用的存储空间被释放了，但是 roll_pointer的值并不会被清除，roll_pointer属性占用7个字节，第一个 比特位就标记着它指向的undo日志的类型，如果该比特位的值为1时， 就代表着它指向的undo日志类型为insert undo。所以我们之后在画图时 都会把insert undo给去掉，大家留意一下就好了。

![image-20211205110224972](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205110226.png)

每次对记录进行改动，都会记录一条undo日志，每条undo日志也都有一个 roll_pointer属性（INSERT操作对应的undo日志没有该属性，因为该记录并没有更早的版本），可以将这些undo日志都连起来，串成一个链表，所 以现在的情况就像下图一样：

![image-20211205110341139](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205110342.png)

我们把这个链表称之为版本链，版本链的头节点就是当 前记录最新的值

另外，每个版本中还包含生成该版本时对应的事务id

**ReadView**

对于使用READ UNCOMMITTED隔离级别的事务来说，由于可以读到未提交事 务修改过的记录，所以直接读取记录的最新版本就好了；对于使用 SERIALIZABLE隔离级别的事务来说，设计InnoDB的大叔规定使用加锁的方 式 来 访 问 记 录

**对 于 使 用 READ COMMITTED和REPEATABLE READ隔离级别的事务来说，都必须保证读到已经 提交了的事务修改过的记录，也就是说假如另一个事务已经修改了记录但 是尚未提交，是不能直接读取最新版本的记录的，核心问题就是：需要判 断一下版本链中的哪个版本是当前事务可见的。为此，设计InnoDB的大叔 提出了一个ReadView的概念，这个ReadView中主要包含4个比较重要的内 容：**

m_ids：表示在生成ReadView时当前系统中活跃的读写事务的事务id列 表。 

min_trx_id：表示在生成ReadView时当前系统中活跃的读写事务中最小 的事务id，也就是m_ids中的最小值。 

max_trx_id：表示生成ReadView时系统中应该分配给下一个事务的id 值。 

小贴士： 注意max_trx_id并不是m_ids中的最大值，事务id是递增分配 的。比方说现在有id为1，2，3这三个事务，之后id为3的事务提交 了。那么一个新的读事务在生成ReadView时，m_ids就包括1和2， min_trx_id的值就是1，max_trx_id的值就是4。

creator_trx_id：表示生成该ReadView的事务的事务id。 小贴士： 我们前边说过，只有在对表中的记录做改动时（执行 INSERT、DELETE、UPDATE这些语句时）才会为事务分配事务id， 否则在一个只读事务中的事务id值都默认为0。

**如果被访问版本的trx_id属性值与ReadView中的creator_trx_id值相 同，意味着当前事务在访问它自己修改过的记录，所以该版本可以被当 前事务访问。 如果被访问版本的trx_id属性值小于ReadView中的min_trx_id值，表明 生成该版本的事务在当前事务生成ReadView前已经提交，所以该版本可 以被当前事务访问。 如果被访问版本的trx_id属性值大于ReadView中的max_trx_id值，表明 生成该版本的事务在当前事务生成ReadView后才开启，所以该版本不可 以被当前事务访问。 如果被访问版本的trx_id属性值在ReadView的min_trx_id和max_trx_id 之间，那就需要判断一下trx_id属性值是不是在m_ids列表中，如果在， 说明创建ReadView时生成该版本的事务还是活跃的，该版本不可以被访 问；如果不在，说明创建ReadView时生成该版本的事务已经被提交，该 版本可以被访问。**

在MySQL中，READ COMMITTED和REPEATABLE READ隔离级别的的一个非常大的区别就是它们生成ReadView的时机不同。

**READ COMMITTED —— 每次读取数据前都生成一个ReadView**

![image-20211205111520076](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205111522.png)

假设现在有一个使用READ COMMITTED隔离级别的事务开始执行：

这个SELECT的执行过程如下： 在执行SELECT语句时会先生成一个ReadView，ReadView的m_ids列表的内 容 就 是 [100, 200] ， min_trx_id 为 100 ， max_trx_id 为 201 ， creator_trx_id为0。 然后从版本链中挑选可见的记录，从图中可以看出，最新版本的列name 的内容是'张飞'，该版本的trx_id值为100，在m_ids列表内，所以不符 合可见性要求，根据roll_pointer跳到下一个版本。 下一个版本的列name的内容是'关羽'，该版本的trx_id值也为100，也在 m_ids列表内，所以也不符合要求，继续跳到下一个版本。 下一个版本的列name的内容是'刘备'，该版本的trx_id值为80，小于 ReadView中的min_trx_id值100，所以这个版本是符合要求的，最后返回 给用户的版本就是这条列name为'刘备'的记录。

![image-20211205111709989](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205111711.png)

然后再到刚才使用READ COMMITTED隔离级别的事务中继续查找这个number 为1的记录，如下：

这个SELECT的执行过程如下： 在执行SELECT语句时会又会单独生成一个ReadView，该ReadView的m_ids 列表的内容就是[200]（事务id为100的那个事务已经提交了，所以再次 生成快照时就没有它了），min_trx_id为200，max_trx_id为201， creator_trx_id为0。 然后从版本链中挑选可见的记录，从图中可以看出，最新版本的列name 的内容是'诸葛亮'，该版本的trx_id值为200，在m_ids列表内，所以不 符合可见性要求，根据roll_pointer跳到下一个版本。 下一个版本的列name的内容是'赵云'，该版本的trx_id值为200，也在 m_ids列表内，所以也不符合要求，继续跳到下一个版本。 下一个版本的列name的内容是'张飞'，该版本的trx_id值为100，小于 ReadView中的min_trx_id值200，所以这个版本是符合要求的，最后返回 给用户的版本就是这条列name为'张飞'的记录。

**REPEATABLE READ —— 在第一次读取数据时生成一个ReadView**

对于使用REPEATABLE READ隔离级别的事务来说，只会在第一次执行查询语 句时生成一个ReadView，之后的查询就不会重复生成了。

也就是说两次SELECT查询得到的结果是重复的，记录的列c值都是'刘备'， 这就是可重复读的含义。如果我们之后再把事务id为200的记录提交了，然 后再到刚才使用REPEATABLE READ隔离级别的事务中继续查找这个number为 1的记录，得到的结果还是'刘备'，具体执行过程大家可以自己分析一下。

**MVCC小结** 

从上边的描述中我们可以看出来，所谓的MVCC（Multi-Version Concurrency Control ，多版本并发控制）指的就是在使用READ COMMITTD、REPEATABLE READ这两种隔离级别的事务在执行普通的SEELCT操作时访问记录的版本链 的过程，这样子可以使不同事务的读-写、写-读操作并发执行，从而提升系 统性能。READ COMMITTD、REPEATABLE READ这两个隔离级别的一个很大不 同就是：生成ReadView的时机不同，READ COMMITTD在每一次进行普通 SELECT操作前都会生成一个ReadView，而REPEATABLE READ只在第一 次进行普通SELECT操作前生成一个ReadView，之后的查询操作都重复使 用这个ReadView就好了。 

小贴士： 我们之前说执行DELETE语句或者更新主键的UPDATE语句 并不会立即把对应的记录完全从页面中删除，而是执行一个所谓的 delete mark操作，相当于只是对记录打上了一个删除标志位，这主要 就是为MVCC服务的，大家可以对比上边举的例子自己试想一下怎么 使用。 另外，所谓的MVCC只是在我们进行普通的SEELCT查询时才 生效，截止到目前我们所见的所有SELECT语句都算是普通的查询， 至于啥是个不普通的查询，我们稍后再说哈～

**关于purge**

我们说insert undo在事务提交之后就可以被释放掉了，而update undo 由于还需要支持MVCC，不能立即删除掉。 为了支持MVCC，对于delete mark操作来说，仅仅是在记录上打一个删除 标记，并没有真正将它删除掉。 

随着系统的运行，在确定系统中包含最早产生的那个ReadView的事务不会 再访问某些update undo日志以及被打了删除标记的记录后，有一个后台运 行的purge线程会把它们真正的删除掉。

# **工**作面试老大难 —— 锁

**写-写情况：即并发事务相继对相同的记录做出改动。** 

我们前边说过，在这种情况下会发生脏写的问题，任何一种隔离级别都 不允许这种问题的发生。所以在多个未提交事务相继对一条记录做改动 时，需要让它们排队执行，这个排队的过程其实是通过锁来实现的。这 个所谓的锁其实是一个内存中的结构，在事务执行前本来是没有锁的， 也就是说一开始是没有锁结构和记录进行关联的，如图所示：

当一个事务想对这条记录做改动时，首先会看看内存中有没有与这条记 录关联的锁结构，当没有的时候就会在内存中生成一个锁结构与之关联。 比方说事务T1要对这条记录做改动，就需要生成一个锁结构与之关联：

![image-20211205112606609](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205112607.png)

![image-20211205112756978](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205112758.png)

![image-20211205112811398](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205112812.png)

不加锁 意思就是不需要在内存中生成对应的锁结构，可以直接执行操作。 

获取锁成功，或者加锁成功 意思就是在内存中生成了对应的锁结构，而且锁结构的is_waiting属 性为false，也就是事务可以继续执行操作。 

获取锁失败，或者加锁失败，或者没有获取到锁

**读-写或写-读情况：也就是一个事务进行读取操作，另一个进行改动操 作。 我们前边说过，这种情况下可能发生脏读、不可重复读、幻读的问题。** 

小贴士： 幻读问题的产生是因为某个事务读了一个范围的记录，之后 别的事务在该范围内插入了新记录，该事务再次读取该范围的记录 时，可以读到新插入的记录，所以幻读问题准确的说并不是因为读取 和写入一条相同记录而产生的，这一点要注意一下。

在REPEATABLE READ隔离级别下，幻读可能发生，脏读和不可重复读不 可以发生。

**不过各个数据库厂商对SQL标准的支持都可能不一样，与SQL标准不同的 一点就是，MySQL在REPEATABLE READ隔离级别实际上就已经解决了幻读 问题。**

怎么解决脏读、不可重复读、幻读这些问题呢？其实有两种可选的解决方 案：

**方案一：读操作利用多版本并发控制（MVCC），写操作进行加锁。**

**所谓的MVCC我们在前一章有过详细的描述，就是通过生成一个 ReadView，然后通过ReadView找到符合条件的记录版本（历史版本是 由undo日志构建的），其实就像是在生成ReadView的那个时刻做了一 次时间静止（就像用相机拍了一个快照），查询语句只能读到在生成 ReadView之前已提交事务所做的更改，在生成ReadView之前未提交的 事务或者之后才开启的事务所做的更改是看不到的。而写操作肯定针 对的是最新版本的记录，读记录的历史版本和改动记录的最新版本本 身并不冲突，也就是采用MVCC时，读-写操作并不冲突。**

**方案二：读、写操作都采用加锁的方式。**

如果我们的一些业务场景不允许读取记录的旧版本，而是每次都必须 去读取记录的最新版本，比方在银行存款的事务中，你需要先把账户 的余额读出来，然后将其加上本次存款的数额，最后再写到数据库 中。在将账户余额读取出来后，就不想让别的事务再访问该余额，直 到本次存款事务执行完成，其他事务才可以访问账户的余额。这样在 读取记录的时候也就需要对其进行加锁操作，这样也就意味着读操作 和写操作也像写-写操作那样排队执行。

**很明显，采用MVCC方式的话，读-写操作彼此并不冲突，性能更高，采用 加锁方式的话，读-写操作彼此需要排队执行，影响性能。一般情况下我 们当然愿意采用MVCC来解决读-写操作并发执行的问题，但是业务在某些 特殊情况下，要求必须采用加锁的方式执行，那也是没有办法的事。**

**读操作**

**一致性读（Consistent Reads）**

事务利用MVCC进行的读取操作称之为一致性读，或者一致性无锁读，有的地 方也称之为快照读。所有普通的SELECT语句（plain SELECT）在READ COMMITTED、REPEATABLE READ隔离级别下都算是一致性读

**锁定读（Locking Reads）**

我们前边说过，并发事务的读-读情况并不会引起什么问题，不过对于写写、读-写或写-读这些情况可能会引起一些问题，需要使用MVCC或者加锁的 方式来解决它们。在使用加锁的方式解决问题时，由于既要允许读-读情况 不受影响，又要使写-写、读-写或写-读情况中的操作相互阻塞，所以设计 MySQL的大叔给锁分了个类：

共享锁，英文名：Shared Locks，简称S锁。在事务要读取一条记录时， 需要先获取该记录的S锁。 

独占锁，也常称排他锁，英文名：Exclusive Locks，简称X锁。在事务要 改动一条记录时，需要先获取该记录的X锁。

**所以我们说S锁和S锁是兼容的，S锁和X锁是不兼容的，X锁和X锁也是不兼容 的，画个表表示一下就是这样：**

![image-20211205114230284](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205114232.png)

**我们前边说在采用加锁方式解决脏读、不可重复读、幻读这些问题时，读取 一条记录时需要获取一下该记录的S锁，其实这是不严谨的，有时候想在读 取记录时就获取记录的X锁，来禁止别的事务读写该记录，为此设计MySQL 的大叔提出了两种比较特殊的SELECT语句格式：**

**对读取的记录加S锁：** 

SELECT ... LOCK IN SHARE MODE;

也就是在普通的SELECT语句后边加LOCK IN SHARE MODE，如果当前事务执行了该语句，那么它会为读取到的记录加S锁，这样允许别的事务继续获取这些记录的S锁（比方说别的事务也使用SELECT ... LOCK IN SHARE MODE语句来读取这些记录），但是不能获取这些记录的X锁（比方说使用 SELECT ... FOR UPDATE语句来读取这些记录，或者直接修改这些记 录）。如果别的事务想要获取这些记录的X锁，那么它们会阻塞，直到当 前事务提交之后将这些记录上的S锁释放掉。 

**对读取的记录加X锁：** 

SELECT ... FOR UPDATE;

也就是在普通的SELECT语句后边加FOR UPDATE，如果当前事务执行了该 语句，那么它会为读取到的记录加X锁，这样既不允许别的事务获取这些 记录的S锁（比方说别的事务使用SELECT ... LOCK IN SHARE MODE语句 来读取这些记录），也不允许获取这些记录的X锁（比方也说使用SELECT ... FOR UPDATE语句来读取这些记录，或者直接修改这些记录）。如果 别的事务想要获取这些记录的S锁或者X锁，那么它们会阻塞，直到当前事务提交之后将这些记录上的X锁释放掉。 

**写操作**

平常所用到的写操作无非是DELETE、UPDATE、INSERT这三种： 

DELETE： 对一条记录做DELETE操作的过程其实是先在B+树中定位到这条记录的位 置，然后获取一下这条记录的X锁，然后再执行delete mark操作。我们 也可以把这个定位待删除记录在B+树中位置的过程看成是一个获取X锁的 锁定读。

UPDATE： 在对一条记录做UPDATE操作时分为三种情况： 如果未修改该记录的键值并且被更新的列占用的存储空间在修改前后 未发生变化，则先在B+树中定位到这条记录的位置，然后再获取一下 记录的X锁，最后在原记录的位置进行修改操作。其实我们也可以把 这个定位待修改记录在B+树中位置的过程看成是一个获取X锁的锁定读。 

如果未修改该记录的键值并且至少有一个被更新的列占用的存储空间 在修改前后发生变化，则先在B+树中定位到这条记录的位置，然后获取一下记录的X锁，将该记录彻底删除掉（就是把记录彻底移入垃圾 链表），最后再插入一条新记录。这个定位待修改记录在B+树中位置 的过程看成是一个获取X锁的锁定读，**新插入的记录由INSERT操作提供的隐式锁进行保护**。 

**如果修改了该记录的键值，则相当于在原记录上做DELETE操作之后再来一次INSERT操作，加锁操作就需要按照DELETE和INSERT的规则进行 了。**

INSERT： 一般情况下，新插入一条记录的操作并不加锁，设计InnoDB的大叔通过 一种称之为隐式锁的东东来保护这条新插入的记录在本事务提交前不被 别的事务访问。

给表加S锁： 如果一个事务给表加了S锁，那么：

- 别的事务可以继续获得该表的S锁 
- 别的事务可以继续获得该表中的某些记录的S锁 
- 别的事务不可以继续获得该表的X锁 
- 别的事务不可以继续获得该表中的某些记录的X锁 

给表加X锁： 如果一个事务给表加了X锁（意味着该事务要独占这个表），那么： 

- 别的事务不可以继续获得该表的S锁 
- 别的事务不可以继续获得该表中的某些记录的S锁 
- 别的事务不可以继续获得该表的X锁 
- 别的事务不可以继续获得该表中的某些记录的X锁

我们在对教学楼整体上锁（表锁）时，怎么知道教学楼中有没有教室已经 被上锁（行锁）了呢？依次检查每一间教室门口有没有上锁？那这效率也 太慢了吧！遍历是不可能遍历的，这辈子也不可能遍历的，于是乎设计 InnoDB的大叔们提出了一种称之为意向锁（英文名：Intention Locks）的 东东：

意向共享锁，英文名：Intention Shared Lock，简称IS锁。当事务准备在某条记录上加S锁时，需要先在表级别加一个IS锁。 

意向独占锁，英文名：Intention Exclusive Lock，简称IX锁。当事务 准备在某条记录上加X锁时，需要先在表级别加一个IX锁。

总结一下：IS、IX锁是表级锁，它们的提出仅仅为了在之后加表级别的S锁和X锁时可以快速判断表中的记录是否被上锁，以避免用遍历的方式来查看表中有没有上锁的记录，也就是说其实IS锁和IX锁是兼容的，IX锁和IX 锁是兼容的。我们画个表来看一下表级别的各种锁的兼容性

![image-20211205115715055](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205115716.png)

对于MyISAM、MEMORY、MERGE这些存储引擎来说，它们只支持表级锁，而且 这些引擎并不支持事务，所以使用这些存储引擎的锁一般都是针对当前会 话来说的。比方说在Session 1中对一个表执行SELECT操作，就相当于为这 个表加了一个表级别的S锁，如果在SELECT操作未完成时，Session 2中对 这个表执行UPDATE操作，相当于要获取表的X锁，此操作会被阻塞，直到 Session 1中的SELECT操作完成，释放掉表级别的S锁后，Session 2中对这 个表执行UPDATE操作才能继续获取X锁，然后执行具体的更新语句。

小贴士： 因为使用MyISAM、MEMORY、MERGE这些存储引擎的表 在同一时刻只允许一个会话对表进行写操作，所以这些存储引擎实际 上最好用在只读，或者大部分都是读操作，或者单用户的情景下。 另 外，在MyISAM存储引擎中有一个称之为Concurrent Inserts的特性，支 持在对MyISAM表读取时同时插入记录，这样可以提升一些插入速 度。

**InnoDB中的表级锁** 

**表级别的S锁、X锁** 

在对某个表执行SELECT、INSERT、DELETE、UPDATE语句时，InnoDB存储 引擎是不会为这个表添加表级别的S锁或者X锁的。 另外，在对某个表执行一些诸如ALTER TABLE、DROP TABLE这类的DDL语 句时，其他事务对这个表并发执行诸如SELECT、INSERT、DELETE、 UPDATE的语句会发生阻塞，同理，某个事务中对某个表执行SELECT、 INSERT、DELETE、UPDATE语句时，在其他会话中对这个表执行DDL语句也 会发生阻塞。这个过程其实是通过在server层使用一种称之为元数据锁 （英文名：Metadata Locks，简称MDL）东东来实现的，一般情况下也不 会使用InnoDB存储引擎自己提供的表级别的S锁和X锁。

小贴士： 在事务简介的章节中我们说过，DDL语句执行时会隐式的提 交当前会话中的事务，这主要是DDL语句的执行一般都会在若干个特 殊事务中完成，在开启这些特殊事务前，需要将当前会话中的事务提交掉。

在系统变量autocommit=0，innodb_table_locks = 1 时，手动获取InnoDB存储引擎提供的表t的S锁或者X锁可以这么写：

**LOCK TABLES t READ：InnoDB存储引擎会对表t加表级别的S锁** 

**LOCK TABLES t WRITE：InnoDB存储引擎会对表t加表级别的X锁**

不过请尽量避免在使用InnoDB存储引擎的表上使用LOCK TABLES这样的手 动锁表语句，它们并不会提供什么额外的保护，只是会降低并发能力而 已。InnoDB的厉害之处还是实现了更细粒度的行锁，关于表级别的S锁和 X锁大家了解一下就罢了。

**表级别的IS锁、IX锁** 

当我们在对使用InnoDB存储引擎的表的某些记录加S锁之前，那就需要先 在表级别加一个IS锁，当我们在对使用InnoDB存储引擎的表的某些记录 加X锁之前，那就需要先在表级别加一个IX锁。IS锁和IX锁的使命只是为 了后续在加表级别的S锁和X锁时判断表中是否有已经被加锁的记录，以 避免用遍历的方式来查看表中有没有上锁的记录。更多关于IS锁和IX锁 的解释我们上边都唠叨过了，就不赘述了。 

**表级别的AUTO-INC锁** 

在使用MySQL过程中，我们可以为表的某个列添加AUTO_INCREMENT属性， 之后在插入记录时，可以不指定该列的值，系统会自动为它赋上递增的值，比方说我们有一个表：

**系统实现这种自动给AUTO_INCREMENT修饰的列递增赋值的原理主要是两 个：**

**采用AUTO-INC锁，也就是在执行插入语句时就在表级别加一个AUTO_INC锁，然后为每条待插入记录的AUTO_INCREMENT修饰的列分配递增的值，在该语句执行结束后，再把AUTO-INC锁释放掉。这样一个事务在持有AUTO-INC锁的过程中，其他事务的插入语句都要被阻塞，可以保证一个语句中分配的递增值是连续的。**

采用AUTO-INC锁，也就是在执行插入语句时就在表级别加一个AUTOINC锁，然后为每条待插入记录的AUTO_INCREMENT修饰的列分配递增 的值，在该语句执行结束后，再把AUTO-INC锁释放掉。这样一个事务 在持有AUTO-INC锁的过程中，其他事务的插入语句都要被阻塞，可以 保证一个语句中分配的递增值是连续的。 如果我们的插入语句在执行前不可以确定具体要插入多少条记录（无 法预计即将插入记录的数量），比方说使用INSERT ... SELECT、 REPLACE ... SELECT或者LOAD DATA这种插入语句，一般是使用AUTOINC锁为AUTO_INCREMENT修饰的列生成对应的值。 

小贴士： 需要注意一下的是，这个AUTO-INC锁的作用范围只是单 个插入语句，插入语句执行完成后，这个锁就被释放了，跟我们之 前介绍的锁在事务结束时释放是不一样的。

**采用一个轻量级的锁，在为插入语句生成AUTO_INCREMENT修饰的列的 值时获取一下这个轻量级锁，然后生成本次插入语句需要用到的 AUTO_INCREMENT列的值之后，就把该轻量级锁释放掉，并不需要等到 整个插入语句执行完才释放锁。 如果我们的插入语句在执行前就可以确定具体要插入多少条记录，比方说我们上边举的关于表t的例子中，在语句执行前就可以确定要插 入2条记录，那么一般采用轻量级锁的方式对AUTO_INCREMENT修饰的 列进行赋值。这种方式可以避免锁定表，可以提升插入性能。**

小 贴 士 ： 设 计 InnoDB 的 大 叔 提 供 了 一 个 称 之 为 innodb_autoinc_lock_mode的系统变量来控制到底使用上述两种方式中 的 哪 种 来 为 AUTO_INCREMENT 修 饰 的 列 进 行 赋 值 ， 当 innodb_autoinc_lock_mode 值 为 0 时 ， 一 律 采 用 AUTO-INC 锁 ； 当 innodb_autoinc_lock_mode 值 为 2 时 ， 一 律 采 用 轻 量 级 锁 ； 当 innodb_autoinc_lock_mode值为1时，两种方式混着来（也就是在插入 记录数量确定时采用轻量级锁，不确定时使用AUTO-INC锁）。【**不过 当innodb_autoinc_lock_mode值为2时，可能会造成不同事务中的插入 语句为AUTO_INCREMENT修饰的列生成的值是交叉的，在有主从复制的场景中是不安全的。**】这句话我不理解。。。

**行锁类型**

**Record Locks：** 我们前边提到的记录锁就是这种类型，也就是仅仅把一条记录锁上，我决定给这种类型的锁起一个比较不正经的名字：正经记录锁（请允许我 皮 一 下 ， 我 实 在 不 知 道 该 叫 个 啥 名 好 ） 。 官 方 的 类 型 名 称 为 ： LOCK_REC_NOT_GAP。比方说我们把number值为8的那条记录加一个正经记 录锁的示意图如下：

![image-20211205131707854](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205131709.png)

**Gap Locks**： 我们说MySQL在REPEATABLE READ隔离级别下是可以解决幻读问题的，解 决方案有两种，可以使用MVCC方案解决，也可以采用加锁方案解决。但 是在使用加锁方案解决时有个大问题，就是事务在第一次执行读取操作 时，那些幻影记录尚不存在，我们无法给这些幻影记录加上正经记录 锁。不过这难不倒设计InnoDB的大叔，他们提出了一种称之为Gap Locks 的锁，官方的类型名称为：LOCK_GAP，我们也可以简称为gap锁。比方说 我们把number值为8的那条记录加一个gap锁的示意图如下：

![image-20211205131936888](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205131938.png)

**如图中为number值为8的记录加了gap锁，意味着不允许别的事务在 number值为8的记录前边的间隙插入新记录，其实就是number列的值(3, 8)这个区间的新记录是不允许立即插入的。比方说有另外一个事务再想 插入一条number值为4的新记录，它定位到该条新记录的下一条记录的 number值为8，而这条记录上又有一个gap锁，所以就会阻塞插入操作， 直到拥有这个gap锁的事务提交了之后，number列的值在区间(3, 8)中的 新记录才可以被插入。 这个gap锁的提出仅仅是为了防止插入幻影记录而提出的，虽然有共享 gap锁和独占gap锁这样的说法，但是它们起到的作用都是相同的。而且 如果你对一条记录加了gap锁（不论是共享gap锁还是独占gap锁），并不 会限制其他事务对这条记录加正经记录锁或者继续加gap锁，再强调一 遍，gap锁的作用仅仅是为了防止插入幻影记录的而已。**

那对于最后一条记录之后的间 隙，也就是hero表中number值为20的记录之后的间隙该咋办呢？也就是 说给哪条记录加gap锁才能阻止其他事务插入number值在(20, +∞)这个区 间的新记录呢？这时候应该想起我们在前边唠叨数据页时介绍的两条伪 记录了： 

Infimum记录，表示该页面中最小的记录。 Supremum记录，表示该页面中最大的记录。 

为了实现阻止其他事务插入number值在(20, +∞)这个区间的新记录，我 们可以给索引中的最后一条记录，也就是number值为20的那条记录所在 页面的Supremum记录加上一个gap锁，画个图就是这样：

![image-20211205132139710](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205132141.png)

这样就可以阻止其他事务插入number值在(20, +∞)这个区间的新记录。 为了大家理解方便，之后的索引示意图中都会把这个Supremum记录画出来。

**Next-Key Locks**： 有时候我们既想锁住某条记录，又想阻止其他事务在该记录前边的间隙 插入新记录，所以设计InnoDB的大叔们就提出了一种称之为Next-Key Locks的锁，官方的类型名称为：LOCK_ORDINARY，我们也可以简称为 next-key锁。比方说我们把number值为8的那条记录加一个next-key锁的 示意图如下

![image-20211205132306241](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205132307.png)

next-key锁的本质就是一个正经记录锁和一个gap锁的合体，它既能保护 该条记录，又能阻止别的事务将新记录插入被保护记录前边的间隙。

**Insert Intention Locks：** 我们说一个事务在插入一条记录时需要判断一下插入位置是不是被别的 事务加了所谓的gap锁（next-key锁也包含gap锁，后边就不强调了），如 果有的话，插入操作需要等待，直到拥有gap锁的那个事务提交。但是设 计InnoDB的大叔规定事务在等待的时候也需要在内存中生成一个锁结 构，表明有事务想在某个间隙中插入新记录，但是现在在等待。设计 InnoDB的大叔就把这种类型的锁命名为Insert Intention Locks，**官方 的类型名称为：LOCK_INSERT_INTENTION，我们也可以称为插入意向锁。 比方说我们把number值为8的那条记录加一个插入意向锁的示意图如下：**

![image-20211205132451101](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205132452.png)

![image-20211205132512759](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205132514.png)

T2和T3之间也并不会相 互阻塞，它们可以同时获取到number值为8的插入意向锁，然后执行插入 操作。事实上插入意向锁并不会阻止别的事务继续获取该记录上任何类 型的锁（插入意向锁就是这么鸡肋）。

**隐式锁**：

我们前边说一个事务在执行INSERT操作时，如果即将插入的间隙已经被 其他事务加了gap锁，那么本次INSERT操作会阻塞，并且当前事务会在该 间隙上加一个插入意向锁，否则一般情况下INSERT操作是不加锁的。那 如果一个事务首先插入了一条记录（此时并没有与该记录关联的锁结 构），然后另一个事务：

**然后另一个事务： 立即使用SELECT ... LOCK IN SHARE MODE语句读取这条事务，也就 是在要获取这条记录的S锁，或者使用SELECT ... FOR UPDATE语句读 取这条事务或者直接修改这条记录，也就是要获取这条记录的X锁， 该咋办？**

**如果允许这种情况的发生，那么可能产生脏读问题。 立即修改这条记录，也就是要获取这条记录的X锁，该咋办？ 如果允许这种情况的发生，那么可能产生脏写问题。**

情景一：对于聚簇索引记录来说，有一个trx_id隐藏列，该隐藏列记 录着最后改动该记录的事务id。那么如果在当前事务中新插入一条聚 簇索引记录后，该记录的trx_id隐藏列代表的的就是当前事务的事务 id，如果其他事务此时想对该记录添加S锁或者X锁时，首先会看一下 该记录的trx_id隐藏列代表的事务是否是当前的活跃事务，如果是的 话，那么就帮助当前事务创建一个X锁（也就是为当前事务创建一个 锁结构，is_waiting属性是false），然后自己进入等待状态（也就 是为自己也创建一个锁结构，is_waiting属性是true）。 

情景二：对于二级索引记录来说，本身并没有trx_id隐藏列，但是在 二级索引页面的Page Header部分有一个PAGE_MAX_TRX_ID属性，该属 性代表对该页面做改动的最大的事务id，如果PAGE_MAX_TRX_ID属性值 小于当前最小的活跃事务id，那么说明对该页面做修改的事务都已经 提交了，否则就需要在页面中定位到对应的二级索引记录，然后回表 找到它对应的聚簇索引记录，然后再重复情景一的做法。 

**通过上边的叙述我们知道，一个事务对新插入的记录可以不显式的加锁 （生成一个锁结构），但是由于事务id这个牛逼的东东的存在，相当于 加了一个隐式锁。别的事务在对这条记录加S锁或者X锁时，由于隐式锁的存在，会先帮助当前事务生成一个锁结构，然后自己再生成一个锁结构 后进入等待状态。**

小贴士： 除了插入意向锁，在一些特殊情况下INSERT还会获取一些锁，我们稍后唠叨哈。

**InnoDB锁的内存结构**

决定在对不同记录加锁时，如果符合下边这些条件： 

- 在同一个事务中进行加锁操作 
- 被加锁的记录在同一个页面中 
- 加锁的类型是一样的 
- 等待状态是一样的

**那么这些记录的锁就可以被放到一个锁结构中**。当然，这么空口白牙的说有点儿抽象，我们还是画个图来看看InnoDB存储引擎中的锁结构具体长啥 样吧：

![image-20211205133737442](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205133739.png)

- **锁所在的事务信息**： 不论是表锁还是行锁，都是在事务执行过程中生成的，哪个事务生成了 这个锁结构，这里就记载着这个事务的信息。 

小贴士： 实际上这个所谓的锁所在的事务信息在内存结构中只是一个 指针而已，所以不会占用多大内存空间，通过指针可以找到内存中关 于该事务的更多信息，比方说事务id是什么。下边介绍的所谓的索引信息其实也是一个指针。

- **索引信息**： 对于行锁来说，需要记录一下加锁的记录是属于哪个索引的。 

- **表锁／行锁信息**： 表锁结构和行锁结构在这个位置的内容是不同的： 

表锁： 记载着这是对哪个表加的锁，还有其他的一些信息。 行锁： 记载了三个重要的信息： Space ID：记录所在表空间。 Page Number：记录所在页号。 n_bits：对于行锁来说，一条记录就对应着一个比特位，一个页面中包含很多记录，用不同的比特位来区分到底是哪一条记录加了 锁。为此在行锁结构的末尾放置了一堆比特位，这个n_bits属性代 表使用了多少比特位。

小贴士： 并不是该页面中有多少记录，n_bits属性的值就是多少。 为了让之后在页面中插入了新记录后也不至于重新分配锁结构，所 以n_bits的值一般都比页面中记录条数多一些。

- **type_mode**： 这是一个32位的数，被分成了lock_mode、lock_type和rec_lock_type三 个部分，如图所示：

  ![image-20211205134113013](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205134114.png)

**锁的模式（lock_mode）**，占用低4位，可选的值如下： 

LOCK_IS（十进制的0）：表示共享意向锁，也就是IS锁。 

LOCK_IX（十进制的1）：表示独占意向锁，也就是IX锁。 

LOCK_S（十进制的2）：表示共享锁，也就是S锁。 

LOCK_X（十进制的3）：表示独占锁，也就是X锁。

LOCK_AUTO_INC（十进制的4）：表示AUTO-INC锁。

小 贴 士 ： 在 InnoDB 存 储 引 擎 中 ， LOCK_IS ， LOCK_IX ， LOCK_AUTO_INC都算是表级锁的模式，LOCK_S和LOCK_X既可 以算是表级锁的模式，也可以是行级锁的模式。

**锁的类型（lock_type）**，占用第5～8位，不过现阶段只有第5位和第 6位被使用： 

LOCK_TABLE（十进制的16），也就是当第5个比特位置为1时，表示表级锁。 

LOCK_REC（十进制的32），也就是当第6个比特位置为1时，表示行 级锁。

**行锁的具体类型（rec_lock_type）**，使用其余的位来表示。只有在 lock_type的值为LOCK_REC时，也就是只有在该锁为行级锁时，才会 被细分为更多的类型：

LOCK_ORDINARY（十进制的0）：表示next-key锁。 LOCK_GAP（十进制的512）：也就是当第10个比特位置为1时，表示 gap锁。 LOCK_REC_NOT_GAP（十进制的1024）：也就是当第11个比特位置为 1时，表示正经记录锁。 LOCK_INSERT_INTENTION（十进制的2048）：也就是当第12个比特 位置为1时，表示插入意向锁。 其他的类型：还有一些不常用的类型我们就不多说了。

怎么还没看见is_waiting属性呢？这主要还是设计InnoDB的大叔太抠 门了，一个比特位也不想浪费，所以他们把is_waiting属性也放到了 **type_mode**这个32位的数字中：

LOCK_WAIT（十进制的256） ：也就是当第9个比特位置为1时，表 示is_waiting为true，也就是当前事务尚未获取到锁，处在等待状 态；当这个比特位为0时，表示is_waiting为false，也就是当前事 务获取锁成功。

- 其他信息： 为了更好的管理系统运行过程中生成的各种锁结构而设计了各种哈希表 和链表，为了简化讨论，我们忽略这部分信息哈～ 一堆比特位

- 一堆比特位： 如果是行锁结构的话，在该结构末尾还放置了一堆比特位，比特位的数量是由上边提到的n_bits属性表示的。我们前边唠叨InnoDB记录结构的 时候说过，**页面中的每条记录在记录头信息中都包含一个heap_no属性**， 伪记录Infimum的heap_no值为0，Supremum的heap_no值为1，之后每插入 一条记录，heap_no值就增1。锁结构最后的一堆比特位就对应着一个页面中的记录，一个比特位映射一个heap_no，不过为了编码方便，映射 方式有点怪：

![image-20211205135045387](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205135046.png)

**分析一下生成行锁结构的过程：**

事务T1要进行加锁，所以锁结构的锁所在事务信息指的就是T1。 

直接对聚簇索引进行加锁，所以索引信息指的其实就是PRIMARY索 引。 由于是行锁，所以接下来需要记录的是三个重要信息：

- Space ID：表空间号为67。
- Page Number：页号为3。
- n_bits：我们的hero表中现在只插入了5条用户记录，但是在初始 分配比特位时会多分配一些，这主要是为了在之后新增记录时不用 频繁分配比特位。其实计算n_bits有一个公式：

n_bits = (1 + ((n_recs + LOCK_PAGE_BITMAP_MARGIN) / 8)) * 8

其中n_recs指的是当前页面中一共有多少条记录（算上伪记录和在 垃圾链表中的记录），比方说现在hero表一共有7条记录（5条用户 记 录 和 2 条 伪 记 录 ） ， 所 以 n_recs 的 值 就 是 7 ， LOCK_PAGE_BITMAP_MARGIN是一个固定的值64，所以本次加锁的 n_bits值就是：n_bits = (1 + ((7 + 64) / 8)) * 8 = 72

type_mode是由三部分组成的： 

- lock_mode，这是对记录加S锁，它的值为LOCK_S。 
- lock_type，这是对记录进行加锁，也就是行锁，所以它的值为 LOCK_REC。 
- rec_lock_type，这是对记录加正经记录锁，也就是类型为 LOCK_REC_NOT_GAP的锁。另外，由于当前没有其他事务对该记 录加锁，所以应当获取到锁，也就是LOCK_WAIT代表的二进制位 应该是0。

一堆比特位 因为number值为15的记录heap_no值为5，根据上边列举的比特位和 heap_no的映射图来看，应该是第一个字节从低位往高位数第6个比特 位被置为1，就像这样：

![image-20211205135724149](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205142236.png)

综上所述，事务T1为number值为5的记录加锁生成的锁结构就如下图所示：

![image-20211205135749573](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205135750.png)

T2想对number值为3、8、15的这三条记录加X型的next-key锁，在对记录 加行锁之前，需要先加表级别的IX锁，也就是会生成一个表级锁的内存 结构，不过我们这里不关心表级锁，所以就忽略掉了哈～ 

现在T2要为3条记录加锁，number为3、8的两条记录由于没有其他事务加 锁，所以可以成功获取这条记录的X型next-key锁，也就是生成的锁结构 的is_waiting属性为false；但是number为15的记录已经被T1加了S型正 经记录锁，T2是不能获取到该记录的X型next-key锁的，也就是生成的锁 结构的is_waiting属性为true。因为等待状态不相同，所以这时候会生 成两个锁结构。这两个锁结构中相同的属性如下： 

- 事务T2要进行加锁，所以锁结构的锁所在事务信息指的就是T2。 
- 直接对聚簇索引进行加锁，所以索引信息指的其实就是PRIMARY索 引。 
- 由于是行锁，所以接下来需要记录是三个重要信息： Space ID：表空间号为67。 Page Number：页号为3。 n_bits：此属性生成策略同T1中一样，该属性的值为72。 type_mode是由三部分组成的： lock_mode，这是对记录加X锁，它的值为LOCK_X。 lock_type，这是对记录进行加锁，也就是行锁，所以它的值为 LOCK_REC。 rec_lock_type ， 这 是 对 记 录 加 next-key 锁 ， 也 就 是 类 型 为 LOCK_ORDINARY的锁。 
- 其他信息 略～ 
- 不同的属性如下： 为number为3、8的记录生成的锁结构： type_mode值。 由 于 可 以 获 取 到 锁 ， 所 以 is_waiting 属 性 为 false ， 也 就 是 LOCK_WAIT代表的二进制位被置0。
- 一堆比特位 因为number值为3、8的记录heap_no值分别为3、4，根据上边列举 的比特位和heap_no的映射图来看，应该是第一个字节从低位往高 位数第4、5个比特位被置为1，就像这样：

![image-20211205140319531](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205140320.png)

综上所述，事务T2为number值为3、8两条记录加锁生成的锁结构就如 下图所示：

![image-20211205140338991](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205140340.png)

- 为number为15的记录生成的锁结构： type_mode值。 由 于 可 以 获 取 到 锁 ， 所 以 is_waiting 属 性 为 true ， 也 就 是 LOCK_WAIT代表的二进制位被置1。

- 一堆比特位：因为number值为15的记录heap_no值为5，根据上边列举的比特位和 heap_no的映射图来看，应该是第一个字节从低位往高位数第6个比 特位被置为1，就像这样：

  ![image-20211205140421153](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205142256.png)

  综上所述，事务T2为number值为15的记录加锁生成的锁结构就如下图 所示

  ![image-20211205140439476](https://gitee.com/YuerryHUAHUA/figure/raw/master/img/20211205140441.png)

  **综上所述，事务T1先获取number值为15的S型正经记录锁，然后事务T2获取 number值为3、8、15的X型正经记录锁共需要生成3个锁结构。**噗～ 关于锁结 构我本来就想写一点点的，没想到一写起来就停不下了，大家乐呵乐呵看 哈～

  小贴士： 上边事务T2在对number值分别为3、8、15这三条记录加锁的 情景中，是按照先对number值为3的记录加锁、再对number值为8的记 录加锁，最后对number值为15的记录加锁的顺序进行的，如果我们一 开始就对number值为15的记录加锁，那么该事务在为number值为15的 记录生成一个锁结构后，直接就进入等待状态，就不为number值为3、 8的两条记录生成锁结构了。在事务T1提交后会把在number值为15的 记录上获取的锁释放掉，然后事务T2就可以获取该记录上的锁，这时 再对number值为3、8的两条记录加锁时，就可以复用之前为number值 为15的记录加锁时生成的锁结构了。

  