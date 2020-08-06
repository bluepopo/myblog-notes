
@[toc]

# 一、字符函数

查看当前mysql客户端的字符集

```mysql
SHOW VARIABLES LIKE '%char%' 
```



## 1. Concat( ) :拼接字符串

```sql
SELECT CONCAT(last_name,'_',first_name) AS 姓名 
FROM employees;
```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200802140629.png" alt="image-20200802140628432" style="zoom:100%;" />

```sql
# 案例：姓名中首字符大写，其他字符小写然后用_拼接，显示出来
SELECT CONCAT(UPPER(SUBSTR(last_name,1,1)),'_',LOWER(SUBSTR(last_name,2)))  out_put FROM employees;
```







## 2. trim( ) :去掉首尾空格

 MySQL除了支持 RTrim() （正如刚才所见，它去掉串右边的空格），还支持 LTrim() （去掉串左边的空格）以及
Trim() （去掉串左右两边的空格）。



trim 去掉首尾的一个空格字符

结果：长度等于9，一个汉字的length=3

```sql
SELECT LENGTH(TRIM('    张翠山    ')) AS out_put; 
```



去掉指定的 ‘aa’字符串

 结果：a张aaaaaaaaaaaa翠山a

```sql
SELECT TRIM('aa' FROM 'aaaaaaaaa张aaaaaaaaaaaa翠山aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa')  AS out_put;
```





## 3. 常用的文本处理函数

```sql
Left()  返回串左边的字符
Right()  返回串右边的字符

Length()  返回串的长度
Locate()  找出串的一个子串
SubString()  返回子串的字符
substr()
INSTR()  返回子串的索引

Upper()  将串转换为大写
Lower()  将串转换为小写

LTrim()  去掉串左边的空格
RTrim()  去掉串右边的空格

RPAD() 3个参数，填充字符串到指定长度

REPLACE() 替换指定字符串

Soundex()  返回串的SOUNDEX值（发音）

```



==substr( )，截取==

```sql
# substr、substring 注意：文本的索引是从1开始的
# 截取从指定索引处后面所有字符
SELECT SUBSTR('李莫愁爱上了陆展元',7)  out_put; -- 陆展元

# 截取从指定索引处指定字符长度的字符
SELECT SUBSTR('李莫愁爱上了陆展元',1,3) out_put; -- 李莫愁
```





==rpad( ) ：填充==

rpad 用指定的字符实现右填充指定长度

```sql
SELECT RPAD('殷素素',12,'ab') AS out_put; #殷素素ababababa
```



使用 ab 去右填充字符串使其长度为12

lpad 用指定的字符实现左填充指定长度

```sql
SELECT LPAD('殷素素',2,'*') AS out_put; # 殷素，填充使得字符串总长度为2(长度变短就截取)
```



==instr( ) ：子串的索引==

instr 返回子串第一次出现的索引，如果找不到返回0

```sql
SELECT INSTR('喜羊羊美羊羊懒羊羊灰太狼','美羊羊') AS out_put; -- 4
```



==replace 替换==

```sql
SELECT REPLACE('周芷若周芷若周芷若周芷若张无忌爱上了周芷若','周芷若','赵敏') AS out_put;
```





## 4. SOUNDEX () ：匹配发音

 SOUNDEX 需要做进一步的解释。 SOUNDEX 是一个将任何文本串转换为描述其语音表示的字母数字模式的算法。 SOUNDEX 考虑了类似的发音字符和音节，使得能对串进行发音比较而不是字母比较。虽然SOUNDEX 不是SQL概念，但MySQL（就像多数DBMS一样）都提供对SOUNDEX 的支持。

下面给出一个使用 Soundex() 函数的例子。 customers 表中有一个顾客 Coyote Inc. ，其联系名为 Y.Lee 。但如果这是输入错误，此联系名实际应该是 Y.Lie ，怎么办？显然，按正确的联系名搜索不会返回数据，如下所示：

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200802141648.png" alt="image-20200802141647703" style="zoom:100%;" />









# 二、数学函数

## 1. 常见数值函数

```sql
Abs()  返回一个数的绝对值
Mod()  返回除操作的余数
Rand()  返回一个随机数
round() 四舍五入小数

Pi()  返回圆周率
Sqrt()  返回一个数的平方根
Exp()  返回一个数的指数值

Sin()  返回一个角度的正弦
Cos()  返回一个角度的余弦
Tan()  返回一个角度的正切

```



```sql
#round 四舍五入
SELECT ROUND(-1.55);
SELECT ROUND(1.567,2); #四舍五入，并保留2位小数


#ceil 向上取整,返回>=该参数的最小整数
SELECT CEIL(-1.02);

#floor 向下取整，返回<=该参数的最大整数
SELECT FLOOR(-9.99);

#truncate 截断
SELECT TRUNCATE(1.69999,2); -- 1.69

#mod取余
/*
mod的计算原理
mod(a,b) ：  a-a/b*b
mod(-10,-3):10- (10)/(-3)*（-3）=1
*/
SELECT MOD(10,-3); #结果 1
SELECT MOD(-10,-3); #结果 -1
SELECT 10%3; #结果 1
```



# 三、日期函数

## 1. 常用日期和时间函数

《Mysql必知必会》第93页

```sql
AddDate()  增加一个日期（天、周等）
AddTime()  增加一个时间（时、分等）

Now()  返回当前日期和时间
CurDate()  返回当前日期
CurTime()  返回当前时间
Date()  返回日期时间的日期部分
Time()  返回一个日期时间的时间部分
Year()  返回一个日期的年份部分

DateDiff()  计算两个日期之差
Date_Add()  高度灵活的日期运算函数
Date_Format()  返回一个格式化的日期或时间串

Month()  返回一个日期的月份部分
Day()  返回一个日期的天数部分
DayOfWeek()  对于一个日期，返回对应的星期几
Hour()  返回一个时间的小时部分
Minute()  返回一个时间的分钟部分
Second()  返回一个时间的秒部分

```

这是重新复习用 WHERE 进行数据过滤的一个好时机。迄今为止，我们都是用比较数值和文本的 WHERE 子句过滤数据，但数据经常需要用日期进行过滤。用日期进行过滤需要注意一些别的问题和使用特殊的MySQL函数

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200802163536.png" alt="image-20200802145140937" style="zoom:100%;" />

但是，使用 WHERE order_date = '2005-09-01' 可靠吗？ order_date 的数据类型为 `datetime` 。这种类型存储日期及时间值。样例表中的值全都具有时间值 00:00:00 ，但实际中很可能并不总是这样。

如果用当前日期和时间存储订单日期（因此你不仅知道订单日期，还知道下订单当天的时间），怎么办？比如，存储的 order_date 值为`2005-09-01 11:30:05` ，则 <font color=red>WHERE order_date = '2005-09-01' 失败。</font>

即使表中存在该日期的数据，也不会把它检索出来，因为 WHERE 匹配失败。

解决办法:

是指示MySQL仅将给出的日期与列中的日期部分进行比较，而不是将给出的日期与整个列值进行比较。为此，必须使用 Date()函数。 Date(order_date) 指示MySQL仅提取列的日期部分，更可靠的SELECT 语句为：

```sql
select cust_id,order_num
from orders
where Data(Order_data) = '2005-09-01';
```

> 如果要的是日期，请使用 Date() 如果你想要的仅是日期，则使用 `Date()` 是一个良好的习惯，即使你知道列只包含日期也应该如此。预防由于某种原因表中以后有日期和时间值，你的SQL代码也不用改变。
>
> 当然，也存在一个 `Time()函数`，在你只想要时间时使用它。
>
> Date() 和 Time() 都是在MySQL 4.1.1中第一次引入的。





问题2：如果你想检索出2005年9月下的所有订单，怎么办？

```sql
-- 方法一
select cust_id,order_num
from orders
where Data(Order_data) between '2005-09-01' and '2005-09-30';

-- 方法二
select cust_id,order_num
from orders
where Year(order_data)=2005 AND Month(order_data)=9;

```





```sql
#now 返回当前系统日期+时间
SELECT NOW();

#curdate 返回当前系统日期，不包含时间
SELECT CURDATE();

#curtime 返回当前时间，不包含日期
SELECT CURTIME();


#可以获取指定的部分，年、月、日、小时、分钟、秒
SELECT YEAR(NOW()) 年;
SELECT YEAR('1998-1-1') 年;

SELECT  YEAR(hiredate) 年 FROM employees;

SELECT MONTH(NOW()) 月;
SELECT MONTHNAME(NOW()) 月;


#str_to_date 将字符通过指定的格式转换成日期

SELECT STR_TO_DATE('1998-3-2','%Y-%c-%d') AS out_put;

#查询入职日期为1992--4-3的员工信息
SELECT * FROM employees WHERE hiredate = '1992-4-3';

SELECT * FROM employees WHERE hiredate = STR_TO_DATE('4-3 1992','%c-%d %Y');


#date_format 将日期转换成字符

SELECT DATE_FORMAT(NOW(),'%y年%m月%d日') AS out_put;

#查询有奖金的员工名和入职日期(xx月/xx日 xx年)
SELECT last_name,DATE_FORMAT(hiredate,'%m月/%d日 %y年') 入职日期
FROM employees
WHERE commission_pct IS NOT NULL;
```





# 四、分支函数

```sql
SELECT VERSION();  # 查看版本号
SELECT DATABASE(); # 查询所有数据库
SELECT USER(); # 查看当前用户
```



### if 函数

```sql
#1.if函数： if else 的效果

SELECT IF(10<5,'大','小');

SELECT last_name,commission_pct, IF(commission_pct IS NULL,'没奖金','有奖金') 备注
FROM employees;
```



### case 函数

case .. when.. then..else

1. 匹配常量值
   case 要判断的字段或表达式
   when 常量1 then 要显示的值1或语句1;
   when 常量2 then 要显示的值2或语句2;
   ...
   else 要显示的值n或语句n;
   end


```sql
/*案例：查询员工的工资，要求

部门号=30，显示的工资为1.1倍
部门号=40，显示的工资为1.2倍
部门号=50，显示的工资为1.3倍
其他部门，显示的工资为原工资

*/
SELECT salary 原始工资,department_id,
CASE department_id
WHEN 30 THEN salary*1.1
WHEN 40 THEN salary*1.2
WHEN 50 THEN salary*1.3
ELSE salary
END AS 新工资
FROM employees;

select id,name,deptId,depetName
case depetId
when 1 then deptName='财务部'
when 2 then deptName='人力部'
when 3 then deptName='技术部'
else then deptName='其他'
end 
from employee;




```

2. 匹配条件，.case 空

   case 
   when 条件1 then 要显示的值1或语句1
   when 条件2 then 要显示的值2或语句2
   。。。
   else 要显示的值n或语句n
   end

```sql
#案例：查询员工的工资的情况
如果工资>20000,显示A级别
如果工资>15000,显示B级别
如果工资>10000，显示C级别
否则，显示D级别


SELECT salary,
CASE 
WHEN salary>20000 THEN 'A'
WHEN salary>15000 THEN 'B'
WHEN salary>10000 THEN 'C'
ELSE 'D'
END AS 工资级别
FROM employees;



select id,name,score
case
when score<60 then '不及格'
when score<=80 then '及格'
when score<=90 then '良好'
when score<=100 then '优秀'
end
as 成绩等级
from stu;

```



# 五、聚集函数



## 1. 常见的聚集函数

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200802151203.png" alt="image-20200802151202752" style="zoom:100%;" />

## 2. AVG() :平均值

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200802151532.png" alt="image-20200802151531436" style="zoom:100%;" />

```sql
SELECT AVG(salary) FROM employees;

#6、和聚集函数一同查询的其他字段有限制
# 例如：把平均工资和每个员工的id一同查询出来
SELECT AVG(salary),employee_id  FROM employees; -- 错误
```



## 3. COUNT( )

COUNT() 函数有两种使用方式。

-  使用 `COUNT(*) `对表中行的数目进行计数，包含空值（  NULL ）.
-   使用` COUNT(column) `对特定列中具有值的行进行计数，忽略NULL 值。

```sql
#5、count函数的详细介绍
SELECT COUNT(salary) FROM employees; #计算salary列的行数，忽略该列为NULL的行

SELECT COUNT(*) FROM employees;#计算行数，其实是判断一行中所有列，只要有一列不为空就+1

SELECT COUNT(1) FROM employees; #计算行数，括号里写常量值，其实是会在表中添加一列全是1的列，然后计算该列1的个数。这个括号里的值可以是任意常量值，但是不能为null值。
 


# 效率：
MYISAM存储引擎下  ，COUNT(*)的效率高
INNODB存储引擎下，COUNT(*)和COUNT(1)的效率差不多，比COUNT(字段)要高一些
```



## 4. MAX() 

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200802152126.png" alt="image-20200802152125084" style="zoom:100%;" />





## 5. min()

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200802152410.png" alt="image-20200802152229415" style="zoom:100%;" />



## 6. SUM( ) 

SUM() 用来返回指定列值的和（总计）。

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200802152417.png" alt="image-20200802152415881" style="zoom:100%;" />



## 7. DISTINCT 参数



```sql
#4、和distinct搭配
SELECT SUM(DISTINCT salary),SUM(salary) FROM employees;
SELECT COUNT(DISTINCT salary),COUNT(salary) FROM employees;

```



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200802152850.png" alt="image-20200802152729024" style="zoom:100%;" />





## 8. 以上组合使用

```sql
#2.查询员工表中的最大入职时间和最小入职时间的相差天数 （DIFFRENCE）
SELECT 
MAX(hiredate) 最最晚入职
MIN(hiredate) 最早入职
(MAX(hiredate)-MIN(hiredate))/1000/3600/24 DIFFRENCE
FROM employees;

SELECT DATEDIFF(MAX(hiredate),MIN(hiredate)) DIFFRENCE
FROM employees;

```





# 六、分组函数

## 1. group by 分组的使用

目前为止的所有计算都是在**表的所有数据** 或 **匹配特定的 WHERE 子句**的数据上进行的。

问题：products 表中有多个供应商提供的产品列表，如果想查询 供应商1003 提供的产品有多少条信息

```sql
select count(*) 
from productts
where vend_id = 1003;
```

但是如果想同时查询，供应商 1001、1002的供应数量怎么办？

<font color=blue>这就是分组显身手的时候了。分组允许把数据分为多个逻辑组，以便能对每个组进行聚集计算。</font>

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200802163207.png" alt="image-20200802154355061" style="zoom:100%;" />



## 2. group by注意事项

在具体使用 GROUP BY 子句前，需要知道一些重要的规定。

- GROUP BY 子句可以包含任意数目的列。这使得能对分组进行嵌套，为数据分组提供更细致的控制。
-   如果在 GROUP BY 子句中嵌套了分组，数据将在最后规定的分组上进行汇总。换句话说，在建立分组时，指定的所有列都一起计算（所以不能从个别的列取回数据）。
-   <font color=red>GROUP BY 子句中列出的每个列都必须是检索列或有效的表达式（但不能是聚集函数）。</font>如果在 SELECT 中使用表达式，则必须在GROUP BY 子句中指定相同的表达式。不能使用别名。
-   除聚集计算语句外， SELECT 语句中的每个列都必须在 GROUP BY 子句中给出。
-   如果分组列中具有 NULL 值，则 NULL 将作为一个分组返回。如果列中有多行 NULL 值，它们将分为一组。
-  <font color=red>GROUP BY 子句必须出现在 WHERE 子句之后， ORDER BY 子句之前。</font>



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200802163202.png" alt="image-20200802154916052" style="zoom:100%;" />



## 3. HAVING 子句：过滤分组

除了能用 GROUP BY 分组数据外，MySQL还允许过滤分组，规定包括哪些分组，排除哪些分组。

例如，已经通过供应商的出现的行数进行了分组，但是我想要查询 产品供应次数超过5次的供应商怎么办？

在前面的学习中我们知道 **WHERE 子句**的作用：过滤，但是 WHERE 过滤是指定行而不是分组。事实上， WHERE 没有分组的概念。

那么，不使用 WHERE 使用什么呢？
**那就是 HAVING 子句**。 HAVING 非常类似于 WHERE 。<font color=red>唯一的差别是WHERE 过滤行，而 HAVING 过滤分组。</font>



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200802163155.png" alt="image-20200802160045838" style="zoom:100%;" />



> <font color=red>**HAVING 和 WHERE 的差别** </font>
>
>  WHERE 在数据分组前进行过滤， 
>
> HAVING 在数据分组后进行过滤。这是一个重要的区别，
>
>  WHERE 排除的行不包括在分组中。

## 4. 分组并排序

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200802162130.png" alt="image-20200802162127550" style="zoom:100%;" />

下面是使用了 order by 排序子句后的结果

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200802162702.png" alt="image-20200802162700351" style="zoom:100%;" />





# <font color=red>七、总结：select 语句的执行顺序</font>

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200802163135.png" alt="image-20200802162850809" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200802163102.png" alt="image-20200802163101054" style="zoom:100%;" />







