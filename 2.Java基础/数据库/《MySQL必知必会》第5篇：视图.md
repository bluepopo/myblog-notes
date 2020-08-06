# 一、视图

## 1. 什么是视图

视图是虚拟的表。与包含数据的表不一样，视图只包含动态检索的 select 查询语句的逻辑。

视图和表的区别：

- 语法不同 ，一个叫 table、一个叫 view
- view 不保存数据，不为数据开辟空间，只是保存了sql逻辑，table保存实际字段数据

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200804104048.png" alt="image-20200804104040044" style="zoom:100%;" />





## 2. 为什么使用视图

我们已经看到了视图应用的一个例子。下面是视图的一些常见应用。

- 重用SQL语句。
- 简化复杂的SQL操作。在编写查询后，可以方便地重用它而不必知道它的基本查询细节。
-  使用表 的组成部分而不是整个表。
-  保护数据。可以给用户授予表的特定部分的访问权限而不是整个表的访问权限。
-  更改数据格式和表示。视图可返回与底层表的表示和格式不同的数据。

在视图创建之后，可以用与表基本相同的方式利用它们。可以对视图执行 SELECT 操作，过滤和排序数据，将视图联结到其他视图或表，甚至能添加和更新数据（添加和更新数据存在某些限制。关于这个内容稍后还要做进一步的介绍）。



重要的是知道视图仅仅是用来查看存储在别处的数据的一种设施。视图本身不包含数据，



> 性能问题 
>
> 因为视图不包含数据，所以每次使用视图时，都必须处理查询执行时所需的任一个检索。如果你用多个联结和过滤创建了复杂的视图或者嵌套了视图，可能会发现性能下降得很厉害。因此，在部署使用了大量视图的应用前，应该进行测试。





## 3. 视图的规则和限制

下面是关于视图创建和使用的一些最常见的规则和限制。

- 与表一样，视图必须唯一命名（不能给视图取与别的视图或表相同的名字）。
-  对于可以创建的视图数目没有限制。
-   为了创建视图，必须具有足够的访问权限。这些限制通常由数据库管理人员授予。
-  视图可以嵌套，即可以利用从其他视图中检索数据的查询来构造一个视图。
-   ORDER BY 可以用在视图中，但如果从该视图检索数据 SELECT 中也含有 ORDER BY ，那么该视图中的 ORDER BY 将被覆盖
- 视图不能索引，也不能有关联的触发器或默认值。
-  视图可以和表一起使用。例如，编写一条联结表和视图的 SELECT语句。



# 二、 使用视图

在理解什么是视图（以及管理它们的规则及约束）后，我们来看一下视图的创建。

- 视图用 CREATE VIEW 语句来创建。
-  使用 SHOW CREATE VIEW viewname ；来查看创建视图的语句。
- 用 DROP 删除视图，其语法为 DROP VIEW viewname;。
-  更新视图时，可以先用DROP再用CREATE，也可以直接用CREATE OR REPLACE VIEW。如果要更新的视图不存在，则第 2 条新语句会创建一个视图；如果要更新的视图存在，则第 2 条更新语句会替换原有视图。



## 5. 利用视图简化复杂的联结

视图的最常见的应用之一是隐藏复杂的SQL，这通常都会涉及联结。请看下面的例子：

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200804104846.png" alt="image-20200804104845331" style="zoom:100%;" />

这条语句创建一个名为 productcustomers 的视图，它联结三个表，以返回已订购了任意产品的所有客户的列表。如果执行SELECT * FROM productcustomers ，将列出订购了任意产品的客户。

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200804105209.png" alt="image-20200804104929615" style="zoom:100%;" />



## 6. 用视图重新格式化检索出的数据

如上所述，视图的另一常见用途是重新格式化检索出的数据。下面的 SELECT 语句（来自第10章）在单个组合计算列中返回供应商名和位置：

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200804105159.png" alt="image-20200804105158678" style="zoom:100%;" />

现在，假如经常需要这个格式的结果。不必在每次需要时执行联结，创建一个视图，每次需要时使用它即可。为把此语句转换为视图，可按如下进行：

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200804105306.png" alt="image-20200804105305121" style="zoom:100%;" />



## 7. 用视图过滤不想要的数据



视图对于应用普通的 WHERE 子句也很有用。例如，可以定义customeremaillist 视图，它过滤没有电子邮件地址的客户。为此目的，可使用下面的语句：







## 8. 使用视图与计算字段

视图对于简化计算字段的使用特别有用。下面是第10章中介绍的一条 SELECT 语句。它检索某个特定订单中的物品，计算每种物品的总价格：

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200804105749.png" alt="image-20200804105559361" style="zoom:100%;" />



# 三、更新视图

通常，视图是可更新的（即，可以对它们使用 INSERT 、 UPDATE 和DELETE ）。更新一个视图将更新其基表（可以回忆一下，视图本身没有数据）。如果你对视图增加或删除行，实际上是对其基表增加或删除行。

但是，并非所有视图都是可更新的。基本上可以说，如果MySQL不能正确地确定被更新的基数据，则不允许更新（包括插入和删除）。这实际上意味着，如果视图定义中有以下操作，则不能进行视图的更新：

- 分组（使用 GROUP BY 和 HAVING ）；
- 联结；
- 子查询；
- 并；
- 聚集函数（ Min() 、 Count() 、 Sum() 等）；
-  DISTINCT；
- 导出（计算）列。

句话说，本章许多例子中的视图都是不可更新的。这听上去好像是一个严重的限制，但实际上不是，**因为视图主要用于数据检索。**



# 四、练习

案例：查询姓张的学生名和专业名

```sql
-- 普通对基本多表查询
SELECT stuname,majorname
FROM stuinfo s
INNER JOIN major m 
ON s.`majorid`= m.`id`
WHERE s.`stuname` LIKE '张%';

-- 创建视图，并查询视图
CREATE VIEW stu_major
AS
SELECT stuname,majorname
FROM stuinfo s
INNER JOIN major m ON s.`majorid`= m.`id`;

-- 查询视图
SELECT * FROM v1 WHERE stuname LIKE '张%';





```

**二、视图的修改**

create or replace view  视图名  as
查询语句;

```sql
SELECT * FROM myv3 

CREATE OR REPLACE VIEW myv3
AS
SELECT AVG(salary),job_id
FROM employees
GROUP BY job_id;
```

方式二：

语法：
alter view 视图名  as 
查询语句;

```sql
ALTER VIEW myv3
AS
SELECT * FROM employees;
```

1.插入

```sql
INSERT INTO myv1 VALUES('张飞','zf@qq.com');
```

2.修改

```sql
UPDATE myv1 SET last_name = '张无忌' WHERE last_name='张飞';
```

3.删除

```sql
DELETE FROM myv1 WHERE last_name = '张无忌';
```







**三、删除视图**

语法：drop view 视图名,视图名,...;

```sql
DROP VIEW emp_v1,emp_v2,myv3;
```

语法：drop view 视图名,视图名,...;



**四、查看视图**

```sql
DESC myv3; -- 表也是这么查看

SHOW CREATE VIEW myv3;-- 表也是这么查看
```





