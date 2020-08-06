

# 一、变量

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200804115101.png" alt="image-20200804115100120" style="zoom:100%;" />



## 2. 自定义变量

分为用户变量、自定义变量

说明：变量由用户自定义，而不是系统提供的
使用步骤：
1、变量声明
2、变量赋值
3、变量使用（查看、比较、运算等）



### 用户变量

==注意：变量名前加 @==

```sql
-- 赋值操作符：=或:=
-- 声明并初始化
SET @变量名=值;
SET @变量名:=值;
SELECT @变量名:=值;
```

```sql
-- 赋值（更新变量的值）
-- 方式一：
	SET @变量名=值;
	SET @变量名:=值;
	SELECT @变量名:=值;
-- 方式二：
	SELECT 字段 INTO @变量名
	FROM 表;
	
-- 使用（查看变量的值）
SELECT @变量名;

```

### 局部变量

作用域：仅仅在定义它的begin end块中有效
应用在 begin end中的第一句话



```sql
-- 声明
DECLARE 变量名 类型;
DECLARE 变量名 类型 【DEFAULT 值】;


-- 赋值（更新变量的值）

-- 方式一：
	SET 局部变量名=值;
	SET 局部变量名:=值;
	SELECT 局部变量名:=值;
-- 方式二：
	SELECT 字段 INTO 具备变量名
	FROM 表;
	
	
-- 使用（查看变量的值）
SELECT 局部变量名;
```

看一个例子：声明两个变量，求和并打印

```sql
-- 用户变量
SET @m=1; 
SET @n=1;
SET @sum=@m+@n;
SELECT @sum;

-- 局部变量
DECLARE m INT DEFAULT 1;
DECLARE n INT DEFAULT 1;
DECLARE SUM INT;
SET SUM=m+n;
SELECT SUM;
```





# 二、存储过程

## 1. 为什么要使用存储过程

- 通过把处理封装在容易使用的单元中，简化复杂的操作（正如前面例子所述）。
-   由于不要求反复建立一系列处理步骤，这保证了数据的完整性。
-  简化对变动的管理。如果表名、列名或业务逻辑（或别的内容）有变化，只需要更改存储过程的代码。使用它的人员甚至不需要
  知道这些变化。这一点的延伸就是安全性。
- 通过存储过程限制对基础数据的访问减少了数据讹误（无意识的或别的原因所导致的数据讹误）的机会。
-  提高性能。因为使用存储过程比使用单独的SQL语句要快。
- 存在一些只能用在单个请求中的MySQL元素和特性，存储过程可以使用它们来编写功能更强更灵活的代码（在下一章的例子中可
  以看到。）

## 2. 执行存储过程

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200804140404.png" alt="image-20200804111513508" style="zoom:100%;" />



## 3. 创建存储过程

请看一个例子——一个返回产品平均价格的存储过程。以下是其代码：

```sql
delimiter //
create procedure pruductpricing()
begin
	select avg(pro?_price) as avgPrice
	from products;
end //
```

果存储过程接受参数，它们将在 () 中列举出来。此存储过程没有参数

 BEGIN 和 END 语句用来限定存储过程体

 DELIMITER // 告诉命令行实用程序使用 // 作为新的语句结束分隔符，可以看到标志存储过程结束的 END 定义为 END
// 而不是 END; 。这样，存储过程体内的 ; 仍然保持不动，并且正确地传递给数据库引擎。

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200804135806.png" alt="image-20200804112146375" style="zoom:100%;" />



## 4. 删除存储过程

```sql
drop procedure productpricing;

drop  IF EXISTS procedure productpricing;

```



## 5. 使用参数



> 参数的数据类型 存储过程的参数允许的数据类型与表中使用的数据类型相同。附录D列出了这些类型。
> 注意，记录集不是允许的类型，因此，不能通过一个参数返回多个行和列。这就是前面的例子为什么要使用3个参数（和3
> 条 SELECT 语句）的原因

### out 参数

```sql
create procedure productpricing(out pl decimal(8,2),out ph decimal(8,2),out pa decimal(8,2))
begin
	select min(pro_price) into pl from product
	select max(pro_price) into ph from product
	select avg(pro_price) into pa from product
end;

call procedure productpricing(@priceLow,@priceHigh,@priceAvg);

select @priceLow,@priceHigh,@priceAvg;
```

此存储过程接受3个参数： pl 存储产品最低价格， ph 存储产品最高价格， pa 存储产品平均价格。每个参数必须具有指定的类型，这里使用十进制值。

关键字 OUT 指出相应的参数用来从存储过程传出一个值（返回给调用者）。



### IN 参数

```sql
create procedure totalPrice(IN orderId int ,out totalPrice decimal(8,2))
degin
	select sum(price * quantity) from orders where id = orderId into totalPrice;
end;   

call procedure totalPrice(1002,@result);

select @result;
```



## 6. 练习：复杂存储过程



案例1 ：创建存储过程实现，用户是否登录成功

```sql
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
```

案例2：

考虑这个场景。你需要获得与以前一样的订单合计，但需要对合计增加营业税，不过只针对某些顾客（或许是你所在州中那些顾客）。那么，你需要做下面几件事情：

- 获得合计（与以前一样）；
- 把营业税有条件地添加到合计；
- 返回合计（带或不带税）。

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200804135714.png" alt="image-20200804135713200" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200804135747.png" alt="image-20200804135745993" style="zoom:100%;" />







# 三、函数

## 1. 函数的定义

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
SELECT 函数名(参数列表) -- 区别于 调用过程时的 call

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
> - 2.函数体：==肯定会有return语句，如果没有会报错==
>   如果return语句没有放在函数体的最后也不报错，但不建议
>
> - return 值 ：有且仅有一个;
>   3.函数体中仅有一句话，则可以省略begin end
>   4.使用 delimiter语句设置结束标记

## 2. 创建函数



```mysql
#1.无参有返回
#案例：返回公司的员工个数
CREATE FUNCTION myf1() RETURNS INT
BEGIN

	DECLARE c INT DEFAULT 0;#定义局部变量
	SELECT COUNT(*) INTO c  FROM employees;
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

## 3. 使用循环结构

分类：while、loop、repeat

iterate类似于 continue，继续，结束本次循环，继续下一次
leave 类似于  break，跳出，结束当前所在的循环





### while

语法：
while 循环条件
do
	循环体;
end while【 标签】;

```sql
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
```



也可以在while循环体内添加 if 语句控制 leave

```sql
-- 案例：批量插入，根据次数插入到admin表中多条记录，如果次数>20则停止
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
```

```sql

-- 案例：批量插入，根据次数插入到admin表中多条记录，只插入偶数次
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
```



### loop

【标签:】loop
	循环体;
end loop 【标签】;

可以用来模拟简单的死循环



### repeat


语法：
【标签：】repeat
	循环体;
until 结束循环的条件
end repeat 【标签】;







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

