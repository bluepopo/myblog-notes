【Mysql】高级

![image-20200714224928090](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200714224929.png)



# 一、MySql介绍与安装



## 1.1 CentOS安装Mysql

![image-20200715121017464](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715121018.png)



![image-20200715123320750](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715123322.png)



## 1.2 修改默认配置文件



/usr/share/mysql/my-huge.cnf 文件即mysql的默认配置文件，为了不破坏这个文件，我们copy一份出来叫做 my.cnf



![image-20200715122810904](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715123527.png)

修改字符集，编辑 my.cnf



![image-20200715123756364](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715123757.png)

![image-20200715124050289](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715124051.png)

注意：对于原来创建过的database，可能中文字符还会乱码，可以重新创建一个database

## 1.3 主要配置文件



![image-20200715124442088](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715124506.png)

1. 二进制日志文件 log-bin :主要用于主从复制

2. 错误日志 log-erro ：默认是关闭的，记录严重的警告和错误信息，每次启动和关闭的信息等。

3. 查询日志log : 默认关闭，记录查询的sql语句，开启后可能会降低mysql性能，因为记录日志也会消耗系统资源。

![image-20200715130001680](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717131818.png)





## 1.4 MySQL 逻辑架构

1. 总体概览

![image-20200715131238361](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715131239.png)

![image-20200715131410224](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715131411.png)





## 1.5 存储引擎





![image-20200715131524652](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717131835.png)

![image-20200715131832651](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715132014.png)





# 二、索引优化



![image-20200715132009666](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715132137.png)







## 2.1  SQL慢问题

![image-20200715132138903](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715132142.png)



索引：把我们经常查询的字段建立索引，这样在大量查询发生时可以提高查询的效率。

单值索引：

复合索引：

## 2.2 Join查询



![image-20200715132933009](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717131843.png)



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715133546.png" alt="image-20200715133545640" style="zoom:67%;" />



![image-20200715133249995](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717131849.png)



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715134333.png" alt="image-20200715134331737" style="zoom:80%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715135614.png" alt="image-20200715135613744" style="zoom: 80%;" />



## 2.3 索引介绍



MySQL官方定义：索引index，是帮助MySQL高效获取数据的数据结构。索引是一种数据结构。

==可以简单理解为“排好序的快速查找数据结构”。==



![image-20200715140124463](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715140125.png)

![image-20200715141040495](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715141041.png)

结论：

数据本身之外，数据库还维护着一个满足特定查找算法的数据结构，这些数据结构以某种方式指向数据。这样就可以在这些数据结构的基础上实现高级查找算法。这种数据结构就是索引。

一般来说，索引也很多，通常会以索引文件形式存储在磁盘上。

平常我们说的索引，荣国没有特别指明，都是B树结构组织的索引，（多路索引搜索树，不一定是二叉索引）。

复合索引，前缀索引，唯一索引默认都是使用B+树索引，统称索引。当然除了，B树索引以外也有哈希索引。



![image-20200715143157323](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717131900.png)





## 2.4 索引分类



![image-20200715144312189](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715144313.png)

### ✔添加索引的语法

![image-20200715144138894](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715174032.png)



## 2.5 索引结构



![image-20200715164304256](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715171757.png)





![image-20200715170345191](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715170346.png)

![image-20200715172800575](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717131909.png)





![image-20200715172902787](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715172903.png)

![image-20200715173111218](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715173112.png)

![image-20200715173305427](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717131920.png)

## 2.6 索引性能分析

![image-20200715173624344](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715173625.png)



### explain

查询出的性能分析结果含有如下字段

id

select_type

table

type

possible_keys

key

key_len

ref

rows

Exetrs





![image-20200715174410114](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717131927.png)

![image-20200715180218024](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715180219.png)







### ==id：==

![image-20200715180834515](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717131934.png)





![image-20200715181000540](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715181001.png)



![image-20200715181612180](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715181613.png)





### ==select_type==

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715183007.png" alt="image-20200715183005469" style="zoom:80%;" />

![image-20200715183042963](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715183044.png)



### ==type==



![image-20200715183434402](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715183435.png)

![image-20200715185810549](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715185811.png)





### ==possible_keys==

显示可能用在这张表中的索引，可能有一个也可能有多个。

若这次查询中的字段中如果存在索引，则该索引将被列出来，**但是该索引不一定被查询实际使用。**

简言之：就是可能会被用到的索引。MySQL告诉用户这张表的本次查询理论上会用到哪些索引，`key`中是本次查询实际用的的所引。

`实际被用到的索引由key`

### ==key==

### ==key_len==

表示索引中使用的字节数，也就是查询中使用的索引的索引长度，在不损失精度的前提下，该值越小越好。



key_len 显示的值为索引字段的最大可能长度，而非实际使用长度。即key_len 是通过表定义计算而得出的，而不是通过检索实际值而得得。



![image-20200715191406246](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715191408.png)





### ==ref==



显示索引的哪一列被使用了，如果可能的话，是一个常数。哪些列或常量被用于查找索引列上的值。

简言之：索引到底有没有被充分使用，比如t1创建了索引，t1 创建了索引 idx_col1_col2, 该索引被 t2.col1检索时充分使用

所以 ref：shared.t2.col1 ,const



![image-20200715192611367](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715192612.png)



==rows==



根据表统计信息以及索引选用的情况，大致估算找到所需的记录需要读取的行数。

![image-20200715193310482](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715193311.png)



==Exetrs==

十分重要的额外信息



![image-20200715192512753](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717132158.png)

![image-20200715194301197](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720130836.png)



![image-20200715194620178](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715194621.png)





![image-20200715194809805](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720130846.png)

![image-20200715195359286](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720130854.png)

![image-20200715195556683](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715195557.png)

![image-20200715195745035](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715195746.png)



### 练习

![image-20200715201207562](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715201209.png)



# 三、索引优化案例

## 3.1 单表优化案例：文章分类评论观看量

![image-20200715203206062](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720130903.png)

![image-20200715202415652](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715202417.png)

![image-20200715202620446](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720130910.png)

![image-20200715203126034](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720131509.png)



## 3.2 两表优化案例：class与book

book：id，card

class：id，card（card的数值表示所属的一个分类)



![image-20200715204539708](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715204540.png)





## 3.3 三表优化案例：join的使用

![image-20200715210303881](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715210305.png)

![image-20200715210433169](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715210434.png)







![image-20200715210050890](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715210052.png)





# 四、索引失效（避免）



## 4.1 索引失效的原因

![image-20200715211236005](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200715211237.png)



索引失效的可能原因：



![image-20200716091111389](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720131524.png)

![image-20200716091713556](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200716091714.png)





![image-20200716092601457](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200716092605.png)



![image-20200716093414363](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720131532.png)

![image-20200716094532267](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720131539.png)

![image-20200716094807331](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200716094808.png)





![image-20200716095035050](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200716095036.png)

![image-20200716095206953](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720133504.png)



![image-20200716095506262](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200716095507.png)

![image-20200716100521601](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200716100522.png)

![image-20200716101212696](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200716101213.png)



![image-20200716102503681](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200716102505.png)



![image-20200716102641376](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200716102642.png)





## 4.2 小总结

==in、or、通配符、不等于、可以试试覆盖索引解决索引失效。==



![image-20200716102928867](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200716102929.png)





## 4.3 面试题

![image-20200716103155963](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720133514.png)

![image-20200716103309298](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200716103310.png)

![image-20200716103701744](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200716104213.png)

![image-20200716104125271](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200716104126.png)

![image-20200716104609212](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200716104610.png)



![image-20200716104830656](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200716104831.png)

![image-20200716105013032](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200716105014.png)

![image-20200716105316678](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720133524.png)



![image-20200716105548231](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720134410.png)



![image-20200716105755526](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720134418.png)





![image-20200716105927685](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200716105930.png)

![image-20200716110445332](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200716110446.png)

![image-20200716110923862](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200716110924.png)





![image-20200716113045284](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720134436.png)





![image-20200716113226118](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720134442.png)





# 五、慢查询优化



## 5.1 小表驱动大表



### ==in 与 exists==



![image-20200717133139299](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717134221.png)

![image-20200717134210000](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717134210.png)



## 5.2 排序优化

### ==order by==

![image-20200717134743824](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717134745.png)



第一步：建表，建索引

以前我们explain都是在测试有没有使用到索引、以及避免索引失效。

这次我们的关注点是 fileSort，它与 排序 order by又息息相关。



![image-20200717135527601](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717135528.png)

![image-20200717135623252](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717135625.png)

![image-20200717135829698](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717140035.png)

![image-20200717140049249](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717140050.png)

![image-20200717140636466](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720134455.png)

![image-20200717141131407](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720134503.png)

![image-20200717143819956](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717144444.png)



![image-20200717145154326](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717145155.png)



![](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717145427.png)



## 5.3 慢查询日志

![image-20200717145601421](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717145629.png)



![image-20200717145722902](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717145724.png)

![image-20200717145944955](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717145946.png)

![image-20200717150022083](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720134518.png)







![image-20200717150336557](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717150337.png)

![image-20200717150551646](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717155721.png)

![image-20200717150727190](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717150728.png)



![image-20200717160001967](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717160003.png)





![image-20200717160123213](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720134526.png)

![image-20200717160219393](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717160220.png)



### ==mysqldumpslow：日志分析工具==

![image-20200717161002403](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717161003.png)

![image-20200717160631183](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717160838.png)

![image-20200717161019494](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717161020.png)



## 5.4 批量数据脚本

![image-20200717161237320](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717161238.png)



2. 修改配置参数，创建函数

![image-20200717161938040](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717163035.png)







**3. 创建函数**

![image-20200717162517758](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717162519.png)

![image-20200717163029338](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717163030.png)



4. 创建存储过程，往表中插入大量数据



![image-20200717163808908](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720134538.png)

![image-20200717163547001](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720134547.png)





5.调用存储过程



![image-20200717164032873](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717164033.png)



## 5.5 show profile

开启profiling功能后，后台会开启自动抓取sql语句的执行情况，保存下来。



![image-20200717164831547](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720134555.png)

![image-20200717165048348](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717165049.png)



![image-20200717165717557](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720134603.png)

![image-20200717170132902](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717170134.png)

![image-20200717170422470](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717170424.png)





### 全局查询日志



![image-20200717171057799](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717173850.png)

![image-20200717171016943](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717173841.png)

![image-20200717171135088](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717171136.png)

![image-20200717174445239](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717174446.png)







# 六、MySQL锁机制



## 6.1 概念

1. 什么是锁

![image-20200717175143556](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720134618.png)



2. 锁分类

   对数据操作得类型分：

- 读锁：共享锁

- 写锁：排它锁

对锁得粗细粒度分

- 表锁
- 行锁
- 页锁



## 6.2 表锁

![image-20200717175744842](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717175745.png)

![image-20200717175807715](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717175808.png)

![image-20200717180042654](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717180211.png)

![image-20200717180213061](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720134627.png)





==读锁案例1==

![image-20200717181335611](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717181645.png)





==写锁案例2==

![image-20200717182140857](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717182141.png)



![image-20200717182401937](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720134634.png)





如何查看那些表被加锁，以及锁定得状况

![image-20200717205927957](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717205929.png)

![image-20200717210144359](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717210145.png)





## 6.3 行锁



![image-20200717211122135](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720134643.png)

![image-20200717211511835](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717211513.png)

![image-20200717211632462](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717211633.png)







![image-20200717211659739](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717211700.png)

![image-20200717212251942](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717212253.png)

![image-20200717213151863](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717213153.png)



![image-20200717213335445](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720134743.png)

![image-20200717215023531](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717215024.png)





## 6.4 间隙锁

![image-20200717215502513](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717215505.png)

![image-20200717215837698](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717215838.png)



![image-20200717222058809](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717222059.png)



## 6.5 如何锁定一行

![image-20200717223346322](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720134754.png)



## 6.6 小总结

![image-20200717223549291](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717223550.png)

![image-20200717223813091](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720134800.png)

![image-20200717223904076](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720134812.png)

![image-20200717224022233](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720134821.png)

## 6.7 页锁



![image-20200717224044863](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717224049.png)

# 七、主从复制



![image-20200717224257020](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717224749.png)

![image-20200717224558042](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717224559.png)

## 7.1 一主一从配置

![image-20200717225707797](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720134829.png)

![image-20200717225751534](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720134838.png)

![image-20200717225806974](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720134849.png)