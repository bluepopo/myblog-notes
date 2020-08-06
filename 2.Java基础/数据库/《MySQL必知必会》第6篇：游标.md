



[toc]



# 一、使用游标

![image-20200805131350767](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200805134626.png)

## 1. 创建游标

declare 游标名 cursor
for 

select 检索语句

```sql
create procedure processorders()
begin
declare ordernumbers cursor
for
select order_num from orders;

open ordernumbers;

close ordernumbers;

end;
```

这个存储过程声明、打开和关闭一个游标。但对检索出的数据什么也没做。



## 2. fetch 语句

在一个游标被打开后，可以使用 FETCH 语句分别访问它的每一行。FETCH 指定检索什么数据（所需的列），检索出来的数据存储在什么地方。它还向前移动游标中的内部行指针，使下一条 FETCH 语句检索下一行（不重复读取同一行）。

在下一个例子中，循环检索数据，从第一行到最后一行：

```sql
create procedure processorders()
begin
declare done boolen default 0;
declare o int;

declare ordernumbers cursor
for
select order_num from orders;

-- declare continue handler
declare continue handler for sqlstatus '02000' set done=1;

open ordernumbers;

repeat 
-- 使用 FETCH 检索当前 order_num到声明的名为 o 的变量中
fetch ordernumbers into o;

-- 反复执行直到 done 为真
until done 
end repeat;

close ordernumbers;

end;
```

> 如果调用这个存储过程，它将定义几个变量和一个 CONTINUEHANDLER ，定义并打开一个游标，重复读取所有行，然后关闭游标。如果一切正常，你可以在循环内放入任意需要的处理（在 FETCH 语句之后，循环结束之前）。
>
> 其中 FETCH 用来检索当前行的 order_num 列（将自动从第一行开始）到一个名为 o 的局部声明的变量中。对检索出的数据不做任何处理。
>
>  done 怎样才能在结束时被设置为真呢？答案是用以下语句：
>
> declare continue handler for sqlstatus '02000' set done=1;
>
> 这条语句定义了一个 CONTINUE HANDLER ，它是在条件出现时被执行的代码。这里，它指出当 SQLSTATE '02000' 出现时， SET done=1。SQLSTATE'02000' 是一个未找到条件，当 REPEAT 由于没有更多的行供循环而不能继续时，出现这个条件
>
> MySQL的错误代码 关于MySQL 5使用的MySQL错误代码列
> 表，请参阅http://dev.mysql.com/doc/mysql/en/error-handling.html



下面一个案例，来将游标检索得每一行数据进行操作

```sql
create prodecure processororders()
begin
declare done boolen dedault 0;
declare o int;
declare t default decimal(8,2);

declare ordernumbers cursor
for 
select order_num from orders;
declare continue handler for sqlstate '02000' set done=1;
create table if not exists ordertotals

open ordernumbers;\
repeat
fetch ordernumbers into o;
call ordertotal(0,1,t);-- 调用计算总金额的存储过程
insert into ordertotals(order_num,total) values(o,t);
until done 
end repeat;

```

此存储过程不返回数据，但它能够创建和填充另一个表ordertotals

> 在这个例子中，我们增加了另一个名为 t 的变量（存储每个订单的合计）。此存储过程还在运行中创建了一个新表（如果它不存在的话），名为 ordertotals 。这个表将保存存储过程生成的结果。 FETCH像以前一样取每个 order_num ，然后用 CALL 执行另一个存储过程（我们在前一章中创建）来计算每个订单的带税的合计（结果存储到 t ）。最后，用 INSERT 保存每个订单的订单号和合计。





# 二、触发器

## 1. 什么是触发器

触发器是MySQL响应以下任意语句而自动执行的一条MySQL语句（或位于 BEGIN 和 END 语句之间的一组语句）：其他MySQL语句不支持触发器。

- DELETE ；
- INSERT ；
- UPDATE 。

> 仅支持表 只有表才支持触发器，视图不支持（临时表也不支持）。



## 2. 创建触发器

在创建触发器时，需要给出4条信息：

- 唯一的触发器名；
-  触发器关联的表；
-  触发器应该响应的活动（ DELETE 、 INSERT 或 UPDATE ）；
-  触发器何时执行（处理之前或之后）

> 保持每个数据库的触发器名唯一 在MySQL 5中，触发器名必须在每个表中唯一，但不是在每个数据库中唯一。这表示同一数据库中的两个表可具有相同名字的触发器。这在其他每个数据库触发器名必须唯一的DBMS中是不允许的，而且以后的MySQL版本很可能会使命名规则更为严格。因此，现在最好是在数据库范围内使用唯一的触发器名

 关键字：==CREATE TRIGGER==

![image-20200805132255069](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200805132256.png)

<font color=red>注意</font>

触发器按每个表每个事件每次地定义，每个表每个事件每次只允许

一个触发器。因此，每个表最多支持6个触发器（每条 INSERT 、 UPDATE和 DELETE 的之前和之后）。单一触发器不能与多个事件或多个表关联，所以，如果你需要一个对 INSERT 和 UPDATE 操作执行的触发器，则应该定义两个触发器。

![image-20200805132504640](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200805135021.png)



## 3. 删除触发器

触发器不能更新或覆盖。为了修改一个触发器，必须先删除它，然后再重新创建。

```sql
drop trigger newproduct;
```





## 4. insert触发器

- 在 INSERT 触发器代码内，可引用一个名为 ==NEW 的虚拟表==，访问被插入的行；

- 在 BEFORE INSERT 触发器中， NEW 中的值也可以被更新（允许更改被插入的值）；
-  对于 AUTO_INCREMENT 列， NEW 在 INSERT 执行之前包含 0 ，在 INSERT执行之后包含新的自动生成值。

下面举一个例子（一个实际有用的例子）。 AUTO_INCREMENT 列具有MySQL自动赋予的值。第21章建议了几种确定新生成值的方法，但下面是一种更好的方法：

```sql
crteate trigger neworder after insert on orders
for each row select New.order_num;
```

> 此代码创建一个名为 neworder 的触发器，它按照 AFTER INSERT ON orders 执行。在插入一个新订单到 orders 表时，MySQL生成一个新订单号并保存到 order_num 中。
>
> 触发器从 NEW. order_num 取得这个值并返回它。此触发器必须按照 AFTER INSERT 执行，因为在 BEFOREINSERT 语句执行之前，新 order_num 还没有生成。
>
> 对于 orders 的每次插入使用这个触发器将总是返回新的订单号。

![image-20200805133247027](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200805133249.png)

BEFORE 或 AFTER ？ 通常，将 BEFORE 用于数据验证和净化（目的是保证插入表中的数据确实是需要的数据）。本提示也适用于 UPDATE 触发器。





## 5. delete 触发器



在使用该触发器之前你需要知道如下几点：

- 在 DELETE 触发器代码内，你可以引用一个名为 OLD 的虚拟表，访问被删除的行；

- OLD 中的值全都是只读的，不能更新

下面的例子演示使用 OLD 保存将要被删除的行到一个存档表中：

```sql
create trigger deleteorder before delete on orders
for each row
begin
	insert into archive_orders(order_num,order_data,cust_id)
	values(OLD.order_num,OLD.order_data,OLD.cust_id);
end;
```

> 在任意订单被删除前将执行此触发器。它使用一条 INSERT 语句将 OLD 中的值（要被删除的订单）保存到一个名为 archive_orders 的存档表中（为实际使用这个例子，你需要用与 orders 相同的列创建一个名为 archive_orders 的表）。

使用 BEFORE DELETE 触发器的优点（相对于 AFTER DELETE 触发器来说）为，如果由于某种原因，订单不能存档， DELETE 本身将被放弃。

![image-20200805133836459](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200805133838.png)

## 6. UPDATE触发器

在 UPDATE 触发器代码中，你可以引用一个名为 OLD 的虚拟表访问以前（ UPDATE 语句前）的值，引用一个名为 NEW 的虚拟表访问更新以后的值；

- 在 BEFORE UPDATE 触发器中， NEW 中的值可能也被更新（允许更改
  将要用于 UPDATE 语句中的值）；
- OLD 中的值全都是只读的，不能更新。





请看下一个例子，保证修改的字段值都是大写的

何数据净化都需要在 UPDATE 语句之前进行

```sql
create trigger updatavender before update on vender
for each row set NEW.vend_state = Upper(NEW.vend_state);
```



## 7. 注意事项

- 应该用触发器来保证数据的一致性（大小写、格式等）。在触发器中执行这种类型的处理的优点是它总是进行这种处理，而且是透明地进行，与客户机应用无关。
-   触发器的一种非常有意义的使用是创建审计跟踪。使用触发器，把更改（如果需要，甚至还有之前和之后的状态）记录到另一个表非常容易。
-  遗憾的是，MySQL触发器中不支持 CALL 语句。这表示不能从触发器内调用存储过程。所需的存储过程代码需要复制到触发器内。

