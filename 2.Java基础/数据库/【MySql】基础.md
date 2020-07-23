【MySql】基础





# 一、基础SQL



## 1.1 操作数据库

DDL:定义数据库对象：数据库，表，列等，关键字：==create, drop,alter== 等



1. 操作数据库：CRUD
    1. 创建

    ```sql
    -- 创建数据库
    create database 数据库名称;
    -- 创建数据库，判断不存在，再创建：
    create database if not exists 数据库名称;
    -- 创建数据库，并指定字符集
    create database 数据库名称 character set 字符集名;
     
    -- 练习： 创建db4数据库，判断是否存在，并制定字符集为gbk
     create database if not exists db4 character set gbk;
    
    ```

    2. 查询

    ```sql
    -- 查询所有数据库的名称:
       show databases;
    -- 查询某个数据库的字符集:查询某个数据库的创建语句
       show create database 数据库名称;
       
    -- 查询当前正在使用的数据库名称
       select database();
    ```

    3. 修改

    ```sql
    -- 修改数据库的字符集
    alter database 数据库名称 character set 字符集名称;
    ```

    4. 删除

    ```sql
    -- 删除数据库
       drop database 数据库名称;
    -- 判断数据库存在，存在再删除
       drop database if exists 数据库名称;
    ```

    

    5. 使用数据库

    ```sql
    use 数据库名称;
    ```

    

## 1.2 操作表

1. 创建表
     ```sql
     /**语法：
     create table 表名(
         列名1 数据类型1,
         列名2 数据类型2,
         ....
         列名n 数据类型n
     );
     */
     
     create table student(
         id int,
         name varchar(32),
         age int ,
         score double(4,1),
         birthday date,
         insert_time timestamp
     );
     ```

     数据库类型：
     > 1. int：整数类型
     > 2. double:小数类型：score double(5,2)
     > 3. date:日期，只包含年月日，yyyy-MM-dd
     > 4. datetime:日期，包含年月日时分秒     yyyy-MM-dd HH:mm:ss
     > 5. timestamp:时间戳类型    包含年月日时分秒    yyyy-MM-dd HH:mm:ss    
     >      * 如果将来不给这个字段赋值，或赋值为null，则默认使用当前的系统时间，来自动赋值
     >
     > 6. varchar：字符串
     >      * name varchar(20):姓名最大20个字符
     >      * zhangsan 8个字符  张三 2个字符

     

     

     复制表：

     ```mysql
     # 复制全表以及数据
     create table 表名 like 被复制的表名;      

     # 复制表结构以及name=张三的数据
create table copy01 
     select id，name
     form stu
     where name=‘张三’;
     
     # 仅复制表结构
     create table copy01 
     select id，name
     form stu
     where 1=2;
     ```
     
     
     
2. 查询表
     ```sql
     -- 查询某个数据库中所有的表名称
        show tables;
     -- 查询表结构
        desc 表名;
     ```

     

3. 修改表

```sql
-- 1. 修改表名
   alter table 表名 rename to 新的表名;
-- 2. 修改表的字符集
   alter table 表名 character set 字符集名称;
-- 3. 添加一列
   alter table 表名 add 列名 数据类型;
-- 4. 修改列名称 类型
   alter table 表名 change 列名 新列别 新数据类型;
   alter table 表名 modify 列名 新数据类型;
-- 5. 删除列
   alter table 表名 drop 列名;
```



4. 删除表

```sql
 drop table 表名;
 drop table  if exists 表名 ;
```

 

## 1.3 操作数据

2) 表的数据进行增删改。关键字==：insert, delete, update== 等


1. **添加数据：**

    ```sql
    insert into 表名(列名1,列名2,...列名n) values(值1,值2,...值n);
    
    INSERT INTO stu(id,NAME,age) VALUES(1,"张无忌",20);
    ```

 

2. **删除数据：**

    ```sql
    delete from 表名 [where 条件]
    ```

    

    * 注意：
         1. 如果不加条件，则删除表中所有记录。
         2. 如果要删除所有记录
             1. delete from 表名; -- 不推荐使用。有多少条记录就会执行多少次删除操作
             2. `TRUNCATE TABLE 表名`; -- 删除表语句：
                      推荐使用，效率更高 先删除表，然后再创建一张一样的空表。
    代码：
    DELETE FROM stu WHERE id=1;

 


3. **修改数据：**

    ```sql
    update 表名 set 列名1 = 值1, 列名2 = 值2,... [where 条件];
    
    UPDATE stu SET age=117 WHERE id=10;
    -- 注意 如果不加任何条件，则会将表中所有记录全部修改
    ```



4. **查询数据**

==分组查询==

```mysql
--1.排序查询：order by 分组后排序
SELECT * FROM stu ORDER BY math ASC,english ASC;

-- 2.列的纵向计算（排除了空值）math：max min sum avg
select count(math) from stu;

-- 3.分组查询: group by
 -- 按照性别分组。分别查询男、女同学的平均分
SELECT sex , AVG(math) FROM student GROUP BY sex;
 -- 按照性别分组。分别查询男、女同学的平均分,人数
SELECT sex , AVG(math),COUNT(id) FROM student GROUP BY sex;
 --  按照性别分组。分别查询男、女同学的平均分,人数 要求：分数低于70分的人，不参与分组
SELECT sex , AVG(math),COUNT(id) FROM student WHERE math > 70 GROUP BY sex;
 --  按照性别分组。分别查询男、女同学的平均分,人数 要求：分数低于70分的人，不参与分组,分组之后。人数要大于2个人
SELECT sex , AVG(math),COUNT(id) FROM student WHERE math > 70 GROUP BY sex HAVING COUNT(id)> 2;
            

#3、分组后筛选

#案例：查询哪个部门的员工个数>5
SELECT COUNT(*),department_id
FROM employees
GROUP BY department_id
HAVING COUNT(*)>5;

#案例2：每个工种有奖金的员工的最高工资>12000的工种编号和该工种最高工资

SELECT job_id,MAX(salary)
FROM employees
WHERE commission_pct IS NOT NULL
GROUP BY job_id
HAVING MAX(salary)>12000;


```

分组查询注意：

1. 分组之后查询的字段：分组字段、聚合函数

2. where 和 having 的区别？

      - where 在分组之前进行限定，如果不满足条件，则不参与分组。
      - having在分组之后进行限定，如果不满足结果，则不会被查询列出结果来

      - where 后不可以跟聚合函数，having可以进行聚合函数的判断。

==分页查询==

注意： limit 是一个MySQL"方言"

```sql
-- 语法：limit 开始的索引,每页查询的条数;
-- 公式：开始的索引 = （当前的页码 - 1） * 每页显示的条数，比如设置每页3条记录，我想看第3页，索引应该是 （3 - 1）*3=6

 
 -- 每页显示3条记录 
             SELECT * FROM student LIMIT 0,3; -- 第1页
             
             SELECT * FROM student LIMIT 3,3; -- 第2页
             
             SELECT * FROM student LIMIT 6,3; -- 第3页

```



==条件查询==

```sql
			-- 查询年龄大于20岁
             SELECT * FROM student WHERE age > 20;             
             SELECT * FROM student WHERE age >= 20;
             
             -- 查询年龄等于20岁
             SELECT * FROM student WHERE age = 20;
             
             -- 查询年龄不等于20岁
             SELECT * FROM student WHERE age != 20;
             SELECT * FROM student WHERE age <> 20;
             
             -- 查询年龄大于等于20 小于等于30             
             SELECT * FROM student WHERE age >= 20 &&  age <=30;
             SELECT * FROM student WHERE age >= 20 AND  age <=30;
             SELECT * FROM student WHERE age BETWEEN 20 AND 30;--闭区间[20,30]
             
             -- 查询年龄22岁，18岁，25岁的信息
             SELECT * FROM student WHERE age = 22 OR age = 18 OR age = 25
             SELECT * FROM student WHERE age IN (22,18,25);
             
             -- 查询英语成绩为null
             SELECT * FROM student WHERE english = NULL; -- 语法不对的。null值不能使用 = （!=） 判断 
             SELECT * FROM student WHERE english IS NULL; --正确语法
             
             -- 查询英语成绩不为null
             SELECT * FROM student WHERE english  IS NOT NULL;
 
```

==模糊查询==

```sql
 			 -- 查询姓马的有哪些？ like
             SELECT * FROM student WHERE NAME LIKE '马%';
             -- 查询姓名第二个字是化的人
             SELECT * FROM student WHERE NAME LIKE "_化%";
             
             -- 查询姓名是3个字的人
             SELECT * FROM student WHERE NAME LIKE '___';
             
             
             -- 查询姓名中包含德的人
             SELECT * FROM student WHERE NAME LIKE '%德%';
```



## 1.4 数据约束



什么是约束：对表中的数据进行限定，保证数据的正确性、有效性和完整性。

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720130513.png" alt="image-20200713232055853" style="zoom:100%;" />

### primary key：主键约束

含义：非空且唯一

一张表只能有一个字段为主键

主键就是表中记录的**唯一**标识

```sql
       -- 2. 在创建表时，添加主键约束
             create table stu(
                 id int primary key,-- 给id添加主键约束
                 name varchar(20)
             );
 
       -- 3. 删除主键
             -- 错误 alter table stu modify id int ;
             ALTER TABLE stu DROP PRIMARY KEY;
 
       -- 4. 创建完表后，后添加主键
             ALTER TABLE stu MODIFY id INT PRIMARY KEY;
 
       -- 5. 自动增长：
             -- 1.  概念：如果某一列是数值类型的，使用 auto_increment 可以来完成值得自动增长
             -- 2. 在创建表时，添加主键约束，并且完成主键自增长
             create table stu(
                 id int primary key auto_increment,-- 给id添加主键约束
                 name varchar(20)
             );
 
             
        -- 6. 删除自动增长
             ALTER TABLE stu MODIFY id INT;
        -- 7. 添加自动增长
             ALTER TABLE stu MODIFY id INT AUTO_INCREMENT;

```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720130520.png" alt="image-20200713232553115" style="zoom: 67%;" />



### auto_increment : 自动增长

 使用该约束的为标识列

自动增长必须和主键搭配吗？ 答：不一定

标识列自动增长必须是数值类型，也可以通过 auto_increment_increment = 3 设置步长。



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720130526.png" alt="image-20200714183550301" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720130533.png" alt="image-20200714184001577" style="zoom:100%;" />

###  not null：非空约束

```sql
-- 创建表时添加约束
CREATE TABLE stu(
    id INT,
    NAME VARCHAR(20) NOT NULL -- name为非空
);

-- 创建表完后，添加非空约束
 ALTER TABLE stu MODIFY NAME VARCHAR(20) NOT NULL;
-- 删除name的非空约束
 ALTER TABLE stu MODIFY NAME VARCHAR(20);
```
### unique：唯一约束

注意mysql中，唯一约束限定的列的值可以有多个null

```sql
-- 1.创建表时，添加唯一约束 ，表示值不重复
CREATE TABLE stu(
    id INT,
    phone_number VARCHAR(20) UNIQUE -- 添加了唯一约束
);

-- 2.删除唯一约束， 不能通过MODIFLY删除unique
 ALTER TABLE stu DROP INDEX phone_number;
     
-- 3. 在创建表后，添加唯一约束
 ALTER TABLE stu MODIFY phone_number VARCHAR(20) UNIQUE;
```


​          
### foreign key：外键约束

* 外键约束：foreign key,让表于表之间的联系关系，从而保证数据的正确性。
         在创建表时，可以添加外键
             * 语法：
                 
     
     ```sql
     -- 1. 创建表时给列名指定外键约束
     create table 表名(
     ....
          外键列
          constraint 外键名称 foreign key (外键列名称) references 主表名称(主表列名称)
);
     -- 2. 删除外键
         ALTER TABLE student DROP foreign key fk_courseid;
      
     -- 3. 创建表之后，添加外键
     ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名称) REFERENCES 主表名称(主表列名称);
     ALTER TABLE student ADD constraint fk_courseid FOREIGN KEY (stu_courseid) references course(course_id);
     
     -- 4. 级联更新：ON UPDATE CASCADE 
     -- 5. 级联删除：ON DELETE CASCADE
     ALTER TABLE student ADD constraint fk_courseid FOREIGN KEY (stu_courseid) references course(course_id) ON UPDATE CASCADE; 
     ```
     





## 1.5 多表查询

按功能分类：
		内连接：
			等值连接
			非等值连接
			自连接
		外连接：
			左外连接
			右外连接
			全外连接
		交叉连接	





1. 隐式内连接：使用where条件消除无用数据，where根据等于判断，就是等值连接

```mysql
-- 例子：
    -- 查询所有员工信息和对应的部门信息
SELECT * FROM emp,dept WHERE emp.ept_id = dept.id;

-- 查询员工表的名称，性别。部门表的名称
SELECT emp.name,emp.gender,dept.name FROM emp,dept WHERE emp.`dept_id` = dept.`id`;

SELECT  t1.name,  t1.gender, t2.name 
FROM   emp t1,dept t2
WHERE 
     t1.`dept_id` = t2.`id`;
     
     
#案例：查询每个工种的工种名和员工的个数，并且按员工个数降序
SELECT job_title,COUNT(*)
FROM employees e,jobs j
WHERE e.`job_id`=j.`job_id`
GROUP BY job_title
ORDER BY COUNT(*) DESC;  

#7、可以实现三表连接？

#案例：查询员工名、部门名和该部门所在的城市
SELECT last_name,department_name,city
FROM employees e,departments d,locations l
WHERE e.`department_id`=d.`department_id`
AND d.`location_id`=l.`location_id`
AND city LIKE 's%'

ORDER BY department_name DESC;
```

​     
​     

2. 显式内连接：INNER JOIN  [表名] ...ON [条件]

```sql
        
-- 语法： select 字段列表 from 表名1 [inner] join 表名2 on 条件
SELECT * FROM emp INNER JOIN dept ON emp.`dept_id` = dept.`id`;  
SELECT * FROM emp JOIN dept ON emp.`dept_id` = dept.`id`; 
 
-- 3. 内连接查询：
             1. 从哪些表中查询数据
             2. 条件是什么
             3. 查询哪些字段
```




3. 外连接：如果主表中想要查询的条件值不在本表中，在另一个表中，可以使用外连接与另一个表进行连接

```sql
-- 左外连接：左边的是主表

SELECT emp.`ename`,dept.`dname`
FROM emp
LEFT OUTER JOIN dept
ON emp.`dept_id`=dept.`id`;
 
-- 右外连接： 右边是主表
SELECT emp.`ename`,dept.`dname`
FROM emp
RIGHT OUTER JOIN dept
ON emp.`dept_id`=dept.`id`;

```

4. 全外连接：Full outer jion

查询结果=内连接结果+主表中有但从表中没有的字段+主表中没有但从表中有的字段



5. 交叉连接：cross join 两表进行笛卡尔乘积









子查询

```sql
-- 子查询不同情况 ：1. 子查询的结果是单行单列的：

-- 查询工资最多的员工的信息
SELECT * FROM emp
WHERE emp.`salary`=(
    SELECT MAX(emp.`salary`)
    FROM emp
);

-- 查询员工工资小于平均工资的人
SELECT * FROM emp
WHERE emp.`salary` <(
    SELECT AVG(emp.`salary`)
    FROM emp
);
 
--  查询财务部门、销售部门的所有员工信息
 SELECT * FROM emp
 where emp.dept_id 
 IN
  (select id from dept
   where dname=‘财务部’ or dname='销售部'
  )；
  
  
  
-- 查询员工入职日期是2011-11-11日之后的员工信息和部门信息
 
SELECT t1.ename,t1.joindate,t2.`dname`
FROM (SELECT * FROM emp WHERE emp.`joindate`>'2001-12-1') t1,
    dept t2
WHERE t1.dept_id=t2.`id`;
```

练习



```sql
-- 3.查询员工姓名，工资，工资等级
  /*
 分析：
 1.员工姓名，工资 emp  工资等级 salarygrade
 2.条件 emp.salary >= salarygrade.losalary and emp.salary <= salarygrade.hisalary
   emp.salary BETWEEN salarygrade.losalary and salarygrade.hisalary
  */
 SELECT 
 t1.ename ,
 t1.`salary`,
 t2.*
FROM emp t1, salarygrade t2
WHERE t1.`salary` BETWEEN t2.`losalary` AND t2.`hisalary`;
             
 
-- 5.查询出部门编号、部门名称、部门位置、部门人数
/*
分析：
    1.部门编号、部门名称、部门位置 dept 表。 部门人数 emp表
    2.使用分组查询。按照 emp.dept_id 完成分组，查询 count(id)
    3.使用子查询将第2步的查询结果和dept表进行关联查询
*/
SELECT dept_id,COUNT(id) renshu
FROM emp
GROUP BY dept_id;
 
SELECT 
    t1.`id`,t1.`dname`,t1.`loc`,t2.renshu
FROM 
    dept t1,
    (SELECT dept_id,COUNT(id) renshu
    FROM emp
    GROUP BY dept_id) t2
    
WHERE 
    t1.id=t2.dept_id;   
         
 
-- 6.查询所有员工的姓名及其直接上级的姓名,没有领导的员工也需要查询
/*
分析：
    1.姓名 emp， 直接上级的姓名 emp
         * emp表的id 和 mgr 是自关联
    2.条件 emp.id = emp.mgr
    3.查询左表的所有数据，和 交集数据
         * 使用左外连接查询                     
*/
 
-- 内连接了自身
SELECT
    t1.`ename`,
    t1.`mgr`,
    t2.`id`,
    t2.`ename`
FROM emp t1,emp t2
WHERE t1.`mgr`=t2.`id`;
 
-- 外连接了自身
SELECT 
    t1.`ename`,
    t1.`mgr`,
    t2.`id`,
    t2.`ename`
         
FROM emp t1
LEFT OUTER JOIN emp t2
ON t1.`mgr`=t2.`id`;       

```



## 1.6 常用函数

### 字符函数

```mysql
# 1.length 获取参数值的字节个数
SELECT LENGTH('john');
SELECT LENGTH('张三丰hahaha');

SHOW VARIABLES LIKE '%char%' # 查看当前mysql客户端的字符集

# 2.concat 拼接字符串
SELECT CONCAT(last_name,'_',first_name) 姓名 FROM employees;


#3.upper、lower
#示例：将姓变大写，名变小写，然后拼接
SELECT CONCAT(UPPER(last_name),LOWER(first_name))  姓名 FROM employees;


#4.substr、substring 注意：索引从1开始
#截取从指定索引处后面所有字符
SELECT SUBSTR('李莫愁爱上了陆展元',7)  out_put;
#截取从指定索引处指定字符长度的字符
SELECT SUBSTR('李莫愁爱上了陆展元',1,3) out_put;
#案例：姓名中首字符大写，其他字符小写然后用_拼接，显示出来
SELECT CONCAT(UPPER(SUBSTR(last_name,1,1)),'_',LOWER(SUBSTR(last_name,2)))  out_put FROM employees;


#5.instr 返回子串第一次出现的索引，如果找不到返回0
SELECT INSTR('杨不殷六侠悔爱上了殷六侠','殷八侠') AS out_put;


#6.trim 去掉首尾空格字符
SELECT LENGTH(TRIM('    张翠山    ')) AS out_put; #长度等于9
# 去掉指定的 ‘aa’字符串 结果：a张aaaaaaaaaaaa翠山a
SELECT TRIM('aa' FROM 'aaaaaaaaa张aaaaaaaaaaaa翠山aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa')  AS out_put;


#7.lpad 用指定的字符实现左填充指定长度
SELECT LPAD('殷素素',2,'*') AS out_put; # 殷素，填充使得字符串总长度为2

#8.rpad 用指定的字符实现右填充指定长度
SELECT RPAD('殷素素',12,'ab') AS out_put; #ababababa殷素素


#9.replace 替换
SELECT REPLACE('周芷若周芷若周芷若周芷若张无忌爱上了周芷若','周芷若','赵敏') AS out_put;
```



### 数学函数

```mysql
#round 四舍五入
SELECT ROUND(-1.55);
SELECT ROUND(1.567,2); #四舍五入，并保留2位小数


#ceil 向上取整,返回>=该参数的最小整数
SELECT CEIL(-1.02);

#floor 向下取整，返回<=该参数的最大整数
SELECT FLOOR(-9.99);

#truncate 截断
SELECT TRUNCATE(1.69999,1);

#mod取余
/*
mod的计算原理
mod(a,b) ：  a-a/b*b
mod(-10,-3):-10- (-10)/(-3)*（-3）=-1
*/
SELECT MOD(10,-3); #结果 -1
SELECT 10%3; #结果 1
```



### 日期函数

```mysql
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





### 其他函数

```mysql
SELECT VERSION();  # 查看版本号
SELECT DATABASE(); # 查询所有数据库
SELECT USER(); # 查看当前用户
```



### if 函数

```mysql
#1.if函数： if else 的效果

SELECT IF(10<5,'大','小');

SELECT last_name,commission_pct, IF(commission_pct IS NULL,'没奖金，呵呵','有奖金，嘻嘻') 备注
FROM employees;
```



### case 函数

case .. when.. then..else



```mysql
/*
case 要判断的字段或表达式
when 常量1 then 要显示的值1或语句1;
when 常量2 then 要显示的值2或语句2;
...
else 要显示的值n或语句n;
end
*/

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


#3.case 空
/*
case 
when 条件1 then 要显示的值1或语句1
when 条件2 then 要显示的值2或语句2
。。。
else 要显示的值n或语句n
end


#案例：查询员工的工资的情况
如果工资>20000,显示A级别
如果工资>15000,显示B级别
如果工资>10000，显示C级别
否则，显示D级别
*/

SELECT salary,
CASE 
WHEN salary>20000 THEN 'A'
WHEN salary>15000 THEN 'B'
WHEN salary>10000 THEN 'C'
ELSE 'D'
END AS 工资级别
FROM employees;

```



### 聚合函数

```mysql
#二、分组函数
/*
功能：用作统计使用，又称为聚合函数或统计函数或组函数
分类：sum 求和、avg 平均值、max 最大值 、min 最小值 、count 计算个数
特点：
1、sum、avg一般用于处理数值型  max、min、count可以处理任何类型
2、以上分组函数都忽略null值
3、可以和distinct搭配实现去重的运算

5、和分组函数一同查询的字段要求是group by后的字段
*/

#1、简单 的使用
SELECT SUM(salary) FROM employees;
SELECT AVG(salary) FROM employees;
SELECT MIN(salary) FROM employees;
SELECT MAX(salary) FROM employees;
SELECT COUNT(salary) FROM employees;

#4、和distinct搭配
SELECT SUM(DISTINCT salary),SUM(salary) FROM employees;
SELECT COUNT(DISTINCT salary),COUNT(salary) FROM employees;

#5、count函数的详细介绍
SELECT COUNT(salary) FROM employees; #计算salary列的数值个数
SELECT COUNT(*) FROM employees;#计算行数，其实是判断一行中所有列，只要有一列不为空就+1
SELECT COUNT(1) FROM employees; #计算行数，括号里写常量值，其实是会在表中添加一列全是1的列，然后计算该列1的个数。这个括号里的值可以是任意常量值，但是不能为null值。
 
#6、和分组函数一同查询的字段有限制
# 例如：把平均工资和每个员工的id一同查询出来
SELECT AVG(salary),employee_id  FROM employees;

# 效率：
MYISAM存储引擎下  ，COUNT(*)的效率高
INNODB存储引擎下，COUNT(*)和COUNT(1)的效率差不多，比COUNT(字段)要高一些
```

```mysql
#2.查询员工表中的最大入职时间和最小入职时间的相差天数 （DIFFRENCE）

SELECT MAX(hiredate) 最大,MIN(hiredate) 最小,(MAX(hiredate)-MIN(hiredate))/1000/3600/24 DIFFRENCE
FROM employees;

SELECT DATEDIFF(MAX(hiredate),MIN(hiredate)) DIFFRENCE
FROM employees;

```









# 二、数据库设计原则

## 2.1 三大范式

1NF ： 每一列都是不可分割的原子数据项



2NF：在1NF的基础上，非码属性必须完全依赖于码（在1NF基础上**消除**非主属性对主码的**部分函数依赖**）

**第二范式需要确保数据库表中的每一列都和主键相关，而不能只与主键的某一部分相关（主要针对联合主键而言）**



3NF ： 在2NF基础上，任何非主属性不依赖于其它非主属性（在2NF基础上消除传递依赖）

[关系型数据库设计：三大范式的通俗理解](https://www.cnblogs.com/wsg25/p/9615100.html)







# 三、事务

## 3.1 四大特性 ACID

原子性 ：是不可分割的最小操作单位，要么同时成功，要么同时失败。

持久性 ： 当事务提交或回滚后，数据库会持久化的保存数据。

隔离性 ： 多个事务之间。相互独立。

一致性 ：事务操作前后，数据总量不变





## 3.2 事务的隔离级别

==并发性问题==

* 概念：多个事务之间隔离的，相互独立的。但是如果多个事务操作同一批数据，则会引发一些问题，设置不同的隔离级别就可以解决这些问题。

*  存在问题：
                1. **脏读：**一个事务，读取到另一个事务中没有提交的数据
                2. **不可重复读**：在同一个事务中，多次查询同一数据返回不同结果。

     > 不可重复读是指对于数据库中的某个数据，一个事务执行过程中多次查询返回不同查询结果，这就是在事务执行过程中，数据被其他事务提交修改了。
     > <font color=red>不可重复读同脏读的区别在于，脏读是一个事务读取了另一未完成的事务执行过程中的数据，而不可重复读是一个事务执行过程中，另一事务提交并修改了当前事务正在读取的数据。</font>

     

     

                1. **幻读**：一个事务操作(DML)数据表中所有记录，另一个事务添加了一条数据，则第一个事务查询不到自己的修改。

     > 幻读是事务非独立执行时发生的一种现象，例如事务T1批量对一个表中某一列列值为1的数据修改为2的变更，但是在这时，事务T2对这张表插入了一条列值为1的数据，并完成提交。此时，如果事务T1查看刚刚完成操作的数据，发现还有一条列值为1的数据没有进行修改，而这条数据其实是T2刚刚提交插入的，这就是幻读。
     >
     > <font color=red>幻读和不可重复读都是读取了另一条已经提交的事务（这点同脏读不同），所不同的是不可重复读查询的都是同一个数据项，而幻读针对的是一批数据整体（比如数据的个数）</font>







==隔离级别==

 1. <font color=blue>read uncommitted：读未提交</font>
              
                 * 产生的问题：脏读、不可重复读、幻读
              
              > 最低的隔离级别，但是并发性能最高，指的是一个事务可以读取到另外一个事务并未提交的数据。
              >
              > 如果一个事务已经开始写数据，则另外一个事务则不允许同时进行写操作，<font color=red>但允许其他事务读此行数据（写操作加写锁，读操作不加锁）</font>。
              
              

 2. <font color=blue>read committed：读已提交 （Oracle）</font>
              
                 * 产生的问题：不可重复读、幻读
              
              > Read Committed（读已提交）：指的是一个事务的更新操作在未提交之前，另外一个事务是读取不到同一数据更新后的结果，这就避免了脏读。读已提交会锁定当前正在读取的行的数据（ 写操作加写锁，读操作加读锁）。
              
              

   3. <font color=blue>repeatable read：可重复读 （MySQL默认）</font>
                
                 * 产生的问题：幻读
                
                > ​	Repeatable Read（可重复读）：mysql的默认的隔离级别，指的是在一个事务内，对相同条件的数据读取结果是相同的，不管其他事务有没有对其进行更新，也不管更新是否已经提交到数据库。<font color=red>可重复读会锁定读取到的所有行直到事务结束，其他事务的更新操作只能等到事务结束之后才能进行。</font>可重复读避免了脏读、不可重复读，但是会出现幻读问题。



  4. <font color=blue>serializable：串行化</font>
           
             * 可以解决所有的问题
           
           > Serializable（串行化）：最高的隔离级别，当然并发性能最低。<font color=red>指的是所有的事务操作依次顺序执行，事务只能一个接着一个执行，不能并发执行。</font>可序列化对表进行加锁，可以有效避免脏读、不可重复读、幻读问题，但是效率比较低，通常会用其他并发级别加上相应的并发锁机制来取代它。
           
           

## 3.3 修改隔离级别的方法

https://blog.csdn.net/sinat_35322593/article/details/81040479?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.nonecase





## 3.4 事务演示



```mysql
# 演示savepoint 的使用
SET autocommit=0; # 开启事务
START TRANSACTION;
DELETE FROM account WHERE id=25;
SAVEPOINT a;#设置保存点
DELETE FROM account WHERE id=28;
ROLLBACK TO a;#回滚到保存点
```





# 四、视图

视图和表的区别：

- 语法不同 ，一个叫 table、一个叫 view
- view 不保存数据，不为数据开辟空间，只是保存了sql逻辑，table保存实际数据
- 



```mysql
#案例：查询姓张的学生名和专业名

# 普通多表查询
SELECT stuname,majorname
FROM stuinfo s
INNER JOIN major m ON s.`majorid`= m.`id`
WHERE s.`stuname` LIKE '张%';

# 创建视图，并查询视图
CREATE VIEW v1
AS
SELECT stuname,majorname
FROM stuinfo s
INNER JOIN major m ON s.`majorid`= m.`id`;

SELECT * FROM v1 WHERE stuname LIKE '张%';


#二、视图的修改

#方式一：
/*
create or replace view  视图名
as
查询语句;
*/
SELECT * FROM myv3 

CREATE OR REPLACE VIEW myv3
AS
SELECT AVG(salary),job_id
FROM employees
GROUP BY job_id;

#方式二：
/*
语法：
alter view 视图名
as 
查询语句;

*/
ALTER VIEW myv3
AS
SELECT * FROM employees;


#三、删除视图
/*
语法：drop view 视图名,视图名,...;
*/
DROP VIEW emp_v1,emp_v2,myv3;


#四、查看视图

DESC myv3; # 表也是这么查看

SHOW CREATE VIEW myv3;# 表也是这么查看

# 对视图中数据操作
#1.插入
INSERT INTO myv1 VALUES('张飞','zf@qq.com');

#2.修改
UPDATE myv1 SET last_name = '张无忌' WHERE last_name='张飞';

#3.删除
DELETE FROM myv1 WHERE last_name = '张无忌';


```







# 五、注意要点



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200714190903.png" alt="image-20200714190857278" style="zoom:100%;" />



## 5.1 变量


系统变量：全局变量、会话变量

自定义变量：用户变量、局部变量


一、系统变量
说明：变量由系统定义，不是用户定义，属于服务器层面
注意：全局变量需要添加global关键字，会话变量需要添加session关键字，如果不写，默认会话级别

```mysql
/*
使用步骤：
1、查看所有系统变量
show global|【session】variables;
2、查看满足条件的部分系统变量
show global|【session】 variables like '%char%';
3、查看指定的系统变量的值
select @@global|【session】系统变量名;
4、为某个系统变量赋值
方式一：
set global|【session】系统变量名=值;
方式二：
set @@global|【session】系统变量名=值;

*/
#1》全局变量
/*
作用域：针对于所有会话（连接）有效，但不能跨重启
*/
#①查看所有全局变量
SHOW GLOBAL VARIABLES;
#②查看满足条件的部分系统变量
SHOW GLOBAL VARIABLES LIKE '%char%';
#③查看指定的系统变量的值
SELECT @@global.autocommit;
#④为某个系统变量赋值
SET @@global.autocommit=0;
SET GLOBAL autocommit=0;

#2》会话变量
/*
作用域：针对于当前会话（连接）有效
*/
#①查看所有会话变量
SHOW SESSION VARIABLES;
#②查看满足条件的部分会话变量
SHOW SESSION VARIABLES LIKE '%char%';
#③查看指定的会话变量的值
SELECT @@autocommit;
SELECT @@session.tx_isolation;
#④为某个会话变量赋值
SET @@session.tx_isolation='read-uncommitted';
SET SESSION tx_isolation='read-committed';

```



二、自定义变量

分为用户变量、自定义变量

说明：变量由用户自定义，而不是系统提供的
使用步骤：
1、声明
2、赋值
3、使用（查看、比较、运算等）

```properties
#用户变量和局部变量的对比

作用域		定义位置									语法
用户变量	当前会话，会话的任何地方					加@符号，不用指定类型
局部变量	定义它的BEGIN END中 	BEGIN END的第一句话	一般不用加@,需要指定类型
```



```mysql
#1.用户变量
/*
作用域：针对于当前会话（连接）有效，作用域同于会话变量
*/

#赋值操作符：=或:=
#①声明并初始化
SET @变量名=值;
SET @变量名:=值;
SELECT @变量名:=值;

#②赋值（更新变量的值）
#方式一：
	SET @变量名=值;
	SET @变量名:=值;
	SELECT @变量名:=值;
#方式二：
	SELECT 字段 INTO @变量名
	FROM 表;
#③使用（查看变量的值）
SELECT @变量名;


#2。局部变量
/*
作用域：仅仅在定义它的begin end块中有效
应用在 begin end中的第一句话
*/

#①声明
DECLARE 变量名 类型;
DECLARE 变量名 类型 【DEFAULT 值】;


#②赋值（更新变量的值）

#方式一：
	SET 局部变量名=值;
	SET 局部变量名:=值;
	SELECT 局部变量名:=值;
#方式二：
	SELECT 字段 INTO 具备变量名
	FROM 表;
#③使用（查看变量的值）
SELECT 局部变量名;


#案例：声明两个变量，求和并打印

#用户变量
SET @m=1;
SET @n=1;
SET @sum=@m+@n;
SELECT @sum;

#局部变量
DECLARE m INT DEFAULT 1;
DECLARE n INT DEFAULT 1;
DECLARE SUM INT;
SET SUM=m+n;
SELECT SUM;



			

```



## 5.2 存储过程和函数

#存储过程
/*
含义：一组预先编译好的SQL语句的集合，理解成批处理语句
1、提高代码的重用性
2、简化操作
3、减少了编译次数并且减少了和数据库服务器的连接次数，提高了效率

```mysql
#一、创建语法

CREATE PROCEDURE 存储过程名(参数列表)
BEGIN

	存储过程体（一组合法的SQL语句）
END
```



```mysql

#注意：
/*
1、参数列表包含三部分
参数模式  参数名  参数类型
举例：
in stuname varchar(20)

参数模式：
in：该参数可以作为输入，也就是该参数需要调用方传入值
out：该参数可以作为输出，也就是该参数可以作为返回值
inout：该参数既可以作为输入又可以作为输出，也就是该参数既需要传入值，又可以返回值

2、如果存储过程体仅仅只有一句话，begin end可以省略
存储过程体中的每条sql语句的结尾要求必须加分号。
存储过程的结尾可以使用 delimiter 重新设置
语法：
delimiter 结束标记
案例：
delimiter $
*/


#二、调用语法

CALL 存储过程名(实参列表);

#--------------------------------案例演示-----------------------------------
#1.空参列表
#案例：插入到admin表中五条记录

SELECT * FROM admin;

DELIMITER $
CREATE PROCEDURE myp1()
BEGIN
	INSERT INTO admin(username,`password`) 
	VALUES('john1','0000'),('lily','0000'),('rose','0000'),('jack','0000'),('tom','0000');
END $


#调用
CALL myp1()$

#2.创建带in模式参数的存储过程
#案例1：创建存储过程实现 根据女神名，查询对应的男神信息
CREATE PROCEDURE myp2(IN beautyName VARCHAR(20))
BEGIN
	SELECT bo.*
	FROM boys bo
	RIGHT JOIN beauty b ON bo.id = b.boyfriend_id
	WHERE b.name=beautyName;
END $
#调用
CALL myp2('柳岩')$


#案例2 ：创建存储过程实现，用户是否登录成功

CREATE PROCEDURE myp4(IN username VARCHAR(20),IN PASSWORD VARCHAR(20))
BEGIN
	DECLARE result INT DEFAULT 0; #声明局部变量并初始化
	
	SELECT COUNT(*) INTO result #赋值
	FROM admin
	WHERE admin.username = username #表中数据与传入参数比较
	AND admin.password = PASSWORD;
	
	SELECT IF(result>0,'成功','失败'); #使用if函数判断变量值
END $

#调用
CALL myp3('张飞','8888')$


#3.创建out 模式参数的存储过程
#案例1：根据输入的女神名，返回对应的男神名

CREATE PROCEDURE myp6(IN beautyName VARCHAR(20),OUT boyName VARCHAR(20))
BEGIN
	SELECT bo.boyname INTO boyname #给变量赋值
	FROM boys bo
	RIGHT JOIN
	beauty b ON b.boyfriend_id = bo.id
	WHERE b.name=beautyName ;
	
END $


#案例2：根据输入的女神名，返回对应的男神名和魅力值

CREATE PROCEDURE myp7(IN beautyName VARCHAR(20),OUT boyName VARCHAR(20),OUT usercp INT) 
BEGIN
	SELECT boys.boyname ,boys.usercp INTO boyname,usercp
	FROM boys 
	RIGHT JOIN beauty b ON b.boyfriend_id = boys.id
	WHERE b.name=beautyName ;
	
END $


#调用
CALL myp7('小昭',@name,@cp)$
SELECT @name,@cp$ #获得用户变量



#4.创建带inout模式参数的存储过程
#案例1：传入a和b两个值，最终a和b都翻倍并返回

CREATE PROCEDURE myp8(INOUT a INT ,INOUT b INT)
BEGIN
	SET a=a*2;
	SET b=b*2;
END $

#调用
SET @m=10$
SET @n=20$
CALL myp8(@m,@n)$
SELECT @m,@n$


#三、删除存储过程
#语法：drop procedure 存储过程名
DROP PROCEDURE p1;
DROP PROCEDURE p2,p3;#×

#四、查看存储过程的信息
DESC myp2;×
SHOW CREATE PROCEDURE  myp2;



```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200714202137.png" alt="image-20200714202135810" style="zoom:100%;" />





## 5.3 函数


含义：一组预先编译好的SQL语句的集合，理解成批处理语句
1、提高代码的重用性
2、简化操作
3、减少了编译次数并且减少了和数据库服务器的连接次数，提高了效率

区别：

存储过程：可以有0个返回，也可以有多个返回，适合做批量插入、批量更新
函数：有且仅有1 个返回，适合做处理数据后返回一个结果

```mysql

# 一、创建语法
CREATE FUNCTION 函数名(参数列表) RETURNS 返回类型
BEGIN
	函数体
END

#二、调用语法
SELECT 函数名(参数列表)

#三、查看函数
SHOW CREATE FUNCTION myf3;

#四、删除函数
DROP FUNCTION myf3;
```

> 注意：
>
> - 1.参数列表 包含两部分：
>   参数名 参数类型
>
> - 2.函数体：肯定会有return语句，如果没有会报错
>   如果return语句没有放在函数体的最后也不报错，但不建议
>
> - return 值 ：有且仅有一个;
>   3.函数体中仅有一句话，则可以省略begin end
>   4.使用 delimiter语句设置结束标记

**案例演示**



```mysql
#1.无参有返回
#案例：返回公司的员工个数
CREATE FUNCTION myf1() RETURNS INT
BEGIN

	DECLARE c INT DEFAULT 0;#定义局部变量
	SELECT COUNT(*) INTO c#赋值
	FROM employees;
	RETURN c;
	
END $

SELECT myf1()$


#2.有参有返回
#案例1：根据员工名，返回它的工资

CREATE FUNCTION myf2(empName VARCHAR(20)) RETURNS DOUBLE
BEGIN
	SET @sal=0;#定义用户变量 
	SELECT salary INTO @sal   #赋值
	FROM employees
	WHERE last_name = empName;
	
	RETURN @sal;
END $

SELECT myf2('k_ing') $

#案例2：根据部门名，返回该部门的平均工资

CREATE FUNCTION myf3(deptName VARCHAR(20)) RETURNS DOUBLE
BEGIN
	DECLARE sal DOUBLE ;
	SELECT AVG(salary) INTO sal
	FROM employees e
	JOIN departments d ON e.department_id = d.department_id
	WHERE d.department_name=deptName;
	RETURN sal;
END $

SELECT myf3('IT')$


#案例
#一、创建函数，实现传入两个float，返回二者之和

CREATE FUNCTION test_fun1(num1 FLOAT,num2 FLOAT) RETURNS FLOAT
BEGIN
	DECLARE SUM FLOAT DEFAULT 0;
	SET SUM=num1+num2;
	RETURN SUM;
END $

SELECT test_fun1(1,2)$


```

## 5.4 循环结构



```mysql
#二、循环结构
/*
分类：
while、loop、repeat

循环控制：

iterate类似于 continue，继续，结束本次循环，继续下一次
leave 类似于  break，跳出，结束当前所在的循环

*/

#1.while
/*

语法：

【标签:】while 循环条件 do
	循环体;
end while【 标签】;

联想：

while(循环条件){

	循环体;
}

*/

#2.loop
/*

语法：
【标签:】loop
	循环体;
end loop 【标签】;

可以用来模拟简单的死循环



*/

#3.repeat
/*
语法：
【标签：】repeat
	循环体;
until 结束循环的条件
end repeat 【标签】;


*/

#1.没有添加循环控制语句
#案例：批量插入，根据次数插入到admin表中多条记录
DROP PROCEDURE pro_while1$
CREATE PROCEDURE pro_while1(IN insertCount INT)
BEGIN
	DECLARE i INT DEFAULT 1;
	WHILE i<=insertCount DO
		INSERT INTO admin(username,`password`) VALUES(CONCAT('Rose',i),'666');
		SET i=i+1;
	END WHILE;
	
END $

CALL pro_while1(100)$


/*

int i=1;
while(i<=insertcount){

	//插入
	
	i++;

}

*/


#2.添加leave语句，循环结束

#案例：批量插入，根据次数插入到admin表中多条记录，如果次数>20则停止
TRUNCATE TABLE admin$
DROP PROCEDURE test_while1$
CREATE PROCEDURE test_while1(IN insertCount INT)
BEGIN
	DECLARE i INT DEFAULT 1;
	a:WHILE i<=insertCount DO
		INSERT INTO admin(username,`password`) VALUES(CONCAT('xiaohua',i),'0000');
		IF i>=20 THEN LEAVE a;
		END IF;
		SET i=i+1;
	END WHILE a;
END $


CALL test_while1(100)$


#3.添加iterate语句

#案例：批量插入，根据次数插入到admin表中多条记录，只插入偶数次
TRUNCATE TABLE admin$
DROP PROCEDURE test_while1$
CREATE PROCEDURE test_while1(IN insertCount INT)
BEGIN
	DECLARE i INT DEFAULT 0;
	a:WHILE i<=insertCount DO
		SET i=i+1;
		IF MOD(i,2)!=0 THEN ITERATE a;
		END IF;
		
		INSERT INTO admin(username,`password`) VALUES(CONCAT('xiaohua',i),'0000');
		
	END WHILE a;
END $


CALL test_while1(100)$

/*

int i=0;
while(i<=insertCount){
	i++;
	if(i%2==0){
		continue;
	}
	插入
	
}

*/

```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200714222720.png" alt="image-20200714222718965" style="zoom:100%;" />



```mysql
/*一、已知表stringcontent
其中字段：
id 自增长
content varchar(20)

向该表插入指定个数的，随机的字符串
*/
DROP TABLE IF EXISTS stringcontent;
CREATE TABLE stringcontent(
	id INT PRIMARY KEY AUTO_INCREMENT,
	content VARCHAR(20)
	
);
DELIMITER $
CREATE PROCEDURE test_randstr_insert(IN insertCount INT)
BEGIN
	DECLARE i INT DEFAULT 1;
	DECLARE str VARCHAR(26) DEFAULT 'abcdefghijklmnopqrstuvwxyz';
	DECLARE startIndex INT;#代表初始索引
	DECLARE len INT;#代表截取的字符长度
	WHILE i<=insertcount DO
		SET startIndex=FLOOR(RAND()*26+1);#代表初始索引，随机范围1-26
		SET len=FLOOR(RAND()*(20-startIndex+1)+1);#代表截取长度，随机范围1-（20-startIndex+1）
		INSERT INTO stringcontent(content) VALUES(SUBSTR(str,startIndex,len));
		SET i=i+1;
	END WHILE;

END $

CALL test_randstr_insert(10)$
```

