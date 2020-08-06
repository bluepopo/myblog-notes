# 第十八章、全文本搜索



## 1. 全文本搜索的使用

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200803132344.png" alt="image-20200803120731865" style="zoom:100%;" />

这些列中有一个名为 note_text 的列，为了进行全文本搜索，MySQL根据子句 FULLTEXT(note_text) 的指示对它进行索引。这里的
FULLTEXT 索引单个列，如果需要也可以指定多个列。

在定义之后，MySQL自动维护该索引。在增加、更新或删除行时，索引随之自动更新。
可以在创建表时指定 FULLTEXT ，或者在稍后指定（在这种情况下所有已有数据必须立即索引）。

> 不要在导入数据时使用 FULLTEXT 更新索引要花时间，虽然不是很多，但毕竟要花时间。如果正在导入数据到一个新表，
> 此时不应该启用 FULLTEXT 索引。应该首先导入所有数据，然后再修改表，定义 FULLTEXT 。这样有助于更快地导入数据（而
> 且使索引数据的总时间小于在导入每行时分别进行索引所需的总时间）。



## 2. 进行全文本搜索

在索引之后，使用两个函数 Match() 和 Against() 执行全文本搜索，
其中 Match() 指定被搜索的列， Against() 指定要使用的搜索表达式

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200803132350.png" alt="image-20200803121217460" style="zoom:100%;" />

分析：

此 SELECT 语句检索单个列 note_text 。由于 WHERE 子句，一个全文本搜索被执行。 Match(note_text) 指示MySQL针对指定的
列进行搜索， Against('rabbit') 指定词 rabbit 作为搜索文本。由于有两行包含词 rabbit ，这两个行被返回。

> 使用完整的 Match() 说明 传递给 Match() 的值必须与FULLTEXT() 定义中的相同。如果指定多个列，则必须列出它们（而且次序正确）。
>
> 搜索不区分大小写 除非使用 BINARY 方式（本章中没有介绍），否则全文本搜索不区分大小写。

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200803121613.png" alt="image-20200803121612661" style="zoom:100%;" />

这条 SELECT 语句同样检索出两行，但次序不同（虽然并不总是出现这种情况）。

## 3. 全文本搜索和LIKE 的对比

上述两条 SELECT 语句都不包含 ORDER BY 子句。后者（使用 LIKE ）以不特别有用的顺序返回数据。

前者（使用全文本搜索）返回以文本匹配的良好程度排序的数据。两个行都包含词 rabbit ，但包含词 rabbit 作为第3个词的行的等级比作为第20个词的行高。

这很重要。全文本搜索的一个重要部分就是对结果排序。具有较高等级的行先返回（因为这些行很可能是你真正想要的行）。



## 4. 全文本搜索的排序

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200803122208.png" alt="image-20200803122207322" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200803122325.png" alt="image-20200803122324220" style="zoom:100%;" />

这里，在 SELECT 而不是 WHERE 子句中使用 Match() 和 Against() 。这使所有行都被返回（因为没有 WHERE 子句）。 Match() 和 Against()用来建立一个计算列（别名为 rank ），此列包含全文本搜索计算出的等级值。

等级由MySQL根据行中词的数目、唯一词的数目、整个索引中词的总数以及包含该词的行的数目计算出来。

正如所见，不包含词 rabbit 的行等级为0（因此不被前一例子中的 WHERE 子句选择）。确实包含词 rabbit的两个行每行都有一个等级值，文本中词靠前的行的等级值比词靠后的行的等级值高。

<font color=red>这个例子有助于说明全文本搜索如何排除行（排除那些等级为0的行），如何排序结果（按等级以降序排序）。</font>



> 排序多个搜索项 如果指定多个搜索项，则包含多数匹配词的那些行将具有比包含较少词（或仅有一个匹配）的那些行高的等级值

正如所见，全文本搜索提供了简单 LIKE 搜索不能提供的功能。而且，由于数据是索引的，全文本搜索还相当快。





## 5. 使用查询扩展

查询扩展用来设法放宽所返回的全文本搜索结果的范围。考虑下面的情况。

你想找出所有提到 anvils 的注释。只有一个注释包含词 anvils ，但你还想找出可能与你的搜索有关的所有其他行，即使它们不包含anvils。

这也是查询扩展的一项任务。在使用查询扩展时，MySQL对数据和索引进行两遍扫描来完成搜索：

- 首先，进行一个基本的全文本搜索，找出与搜索条件匹配的所有行；
- 其次，MySQL检查这些匹配行并选择所有有用的词（我们将会简要地解释MySQL如何断定什么有用，什么无用）。
-  再其次，MySQL再次进行全文本搜索，这次不仅使用原来的条件，而且还使用所有有用的词。



利用查询扩展，能找出可能相关的结果，即使它们并不精确包含所查找的词。

> 只用于MySQL版本4.1.1或更高级的版本 查询扩展功能是在MySQL 4.1.1中引入的，因此不能用于之前的版本。





我们看一个例子来更好的理解什么是 查询扩展

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200803123904.png" alt="image-20200803123902890" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200803124007.png" alt="image-20200803124006862" style="zoom:100%;" />

这次返回了7行。第一行包含词 anvils ，因此等级最高。

第二行与 anvils 无关，但因为它包含第一行中的两个词（ customer和 recommend ），所以也被检索出来。

第3行也包含这两个相同词，但它们在文本中的位置更靠后且分开得更远，因此也包含这一行，但等级为第三。第三行确实也没有涉及 anvils （按它们的产品名）。

正如所见，查询扩展极大地增加了返回的行数，但这样做也增加了你实际上并不想要的行的数目。





## 6. 布尔文本搜索

MySQL支持全文本搜索的另外一种形式，称为布尔方式（booleanmode）。以布尔方式，可以提供关于如下内容的细节：

- 要匹配的词；
- 要排斥的词（如果某行包含这个词，则不返回该行，即使它包含其他指定的词也是如此）；

-  排列提示（指定某些词比其他词更重要，更重要的词等级更高）；
- 表达式分组；
-  另外一些内容。

> 即使没有 FULLTEXT 索引也可以使用 布尔方式不同于迄今为止使用的全文本搜索语法的地方在于，即使没有定义
> FULLTEXT 索引，也可以使用它。但这是一种非常缓慢的操作（其性能将随着数据量的增加而降低）。

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200803124403.png" alt="image-20200803124402482" style="zoom:100%;" />

此全文本搜索检索包含词 heavy 的所有行（有两行）。其中使用了关键字 IN BOOLEAN MODE ，但实际上没有指定布尔操作符，因此，其结果与没有指定布尔方式的结果相同。

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200803124450.png" alt="image-20200803124448942" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200803124623.png" alt="image-20200803124622521" style="zoom:100%;" />

这次只返回一行。这一次仍然匹配词 heavy ，但 -rope* 明确地指示MySQL排除包含 rope* （任何以 rope 开始的词，包括ropes ）的行，这就是为什么上一个例子中的第一行被排除的原因。

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200803124719.png" alt="image-20200803124718620" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200803124757.png" alt="image-20200803124756676" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200803125310.png" alt="image-20200803125309341" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200803132412.png" alt="image-20200803125446333" style="zoom:100%;" />

## 7. 全文本搜索的使用说明

在结束本章之前，给出关于全文本搜索的某些重要的说明。

- 在索引全文本数据时，短词被忽略且从索引中排除。短词定义为那些具有3个或3个以下字符的词（如果需要，这个数目可以更改。MySQL带有一个内建的非用词（stopword）列表，这些词在索引全文本数据时总是被忽略。如果需要，可以覆盖这个列表（请参
  阅MySQL文档以了解如何完成此工作）。

- 许多词出现的频率很高，搜索它们没有用处（返回太多的结果）。因此，MySQL规定了一条50%规则，如果一个词出现在50%以上
  的行中，则将它作为一个非用词忽略。50%规则不用于 IN BOOLEANMODE 。

- 如果表中的行数少于3行，则全文本搜索不返回结果（因为每个词或者不出现，或者至少出现在50%的行中）。忽略词中的单引号。例如， don't 索引为 dont 。

- 忽略词中的单引号。例如， don't 索引为 dont 。不具有词分隔符（包括日语和汉语）的语言不能恰当地返回全文本搜索结果。如前所述，仅在 MyISAM 数据库引擎中支持全文本搜索。