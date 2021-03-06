# 1-安装

## 相关信息

MySQL本身实际上只是一个SQL接口，它的内部还包含了多种数据引擎，常用的包括：

- InnoDB：由Innobase Oy公司开发的一款支持事务的数据库引擎，2006年被Oracle收购；
- MyISAM：MySQL早期集成的默认数据库引擎，不支持事务。

对用户而言，切换浏览器引擎不影响浏览器界面，切换MySQL引擎不影响自己写的应用程序使用MySQL的接口。

使用MySQL时，不同的表还可以使用不同的数据库引擎。如果你不知道应该采用哪种引擎，记住总是选择***InnoDB***就好了。

**MariaDB：**由MySQL的创始人创建的一个开源分支版本，使用XtraDB引擎。

**Aurora：**由Amazon改进的一个MySQL版本，专门提供给在AWS托管MySQL用户，号称5倍的性能提升。

**PolarDB：**由Alibaba改进的一个MySQL版本，专门提供给在阿里云托管的MySQL用户，号称6倍的性能提升。

MySQL官方版本又分了好几个版本：

- Community Edition：社区开源版本，免费；
- Standard Edition：标准版；
- Enterprise Edition：企业版；
- Cluster Carrier Grade Edition：集群版。

以上版本的功能依次递增，价格也依次递增。不过，功能增加的主要是监控、集群等管理功能，对于基本的SQL功能是完全一样的。

Debian和Ubuntu用户可以简单地通过命令`apt-get install mysql-server`安装最新的MySQL版本。

MySQL安装后会自动在后台运行。为了验证MySQL安装是否正确，我们需要通过`mysql`这个命令行程序来连接MySQL服务器。

在命令提示符下( **root** )输入`mysql -u root -p`，(默认没密码)然后输入口令，如果一切正确，就会连接到MySQL服务器，同时提示符变为`mysql>`。

输入`exit`退出MySQL命令行。注意，MySQL服务器仍在后台运行。

# 2-理论知识

**关系数据库**是建立在**关系模型**上的。而关系模型**本质上就是若干个存储数据的二维表**

表的每一行称为**记录（Record）**，记录是一个逻辑意义上的数据。

表的每一列称为**字段（Column）**，同一个表的每一行记录都拥有相同的若干字段。

字段定义了数据类型（整型、浮点型、字符串、日期等），以及是否允许为`NULL`。注意`NULL`表示字段数据不存在。**通常情况下，字段应该避免允许为`NULL`**。不允许为`NULL`可以简化查询条件，加快查询速度，也利于应用程序读取数据后无需判断是否为`NULL`。

关系数据库的表和表之间需要建立**“一对多”**，**“多对一”**和**“一对一”**的关系，这样才能够按照应用程序的逻辑来组织和存储数据。

班级表

| 班级ID | 名称       | 班主任ID |
| ------ | ---------- | -------- |
| 201    | 二年级一班 | A1       |
| 202    | 二年级二班 | A2       |

学生表

| 学生ID | 姓名 | 班级ID | 性别 | 年龄 |
| ------ | ---- | ------ | ---- | ---- |
| 1      | 小黄 | 201    | W    | 9    |
| 2      | 小郑 | 202    | M    | 10   |

教师表

| 班主任ID | 名称   | 年龄 |
| -------- | ------ | ---- |
| A1       | 李老师 | 30   |
| A2       | 张老师 | 31   |

一个班级总是对应一个教师，班级表和教师表就是“一对一”关系。

每一行对应着一个班级，而一个班级对应着多个学生，所以班级表和学生表的关系就是“一对多”：

在关系数据库中，关系是通过**主键**和**外键**来维护的。

## 主键



每一条记录都包含若干定义好的字段。同一个表的所有记录都有相同的字段定义。

对于关系表，有个很重要的约束，就是**任意两条记录不能重复**。通过某个字段唯一区分出不同的记录，这个字段被称为**主键**。

有可能重名，同年月，同性别，这些都不具备唯一性，所以要选择或创造一个唯一的字段，作为主键。

选取主键的一个基本原则是：**不使用任何业务相关的字段作为主键。**手机号，身份证都有可能会出现变动，可能会造成后续的问题

作为主键最好是完全业务无关的字段，我们一般把这个字段命名为`id`。常见的可作为`id`字段的类型有：

1. **自增整数类型**：数据库会在插入数据时自动为每一条记录分配一个自增整数，这样我们就完全不用担心主键重复，也不用自己预先生成主键；
2. **全局唯一GUID类型**：使用一种全局唯一的字符串作为主键，类似`8f55d96b-8acc-4636-8cb8-76bf8abc2f57`。GUID算法通过网卡MAC地址、时间戳和随机数保证任意计算机在任意时间生成的字符串都是不同的，大部分编程语言都内置了GUID算法，可以自己预算出主键。

对于大部分应用来说，通常自增类型的主键就能满足需求。我们在`students`表中定义的主键也是`BIGINT NOT NULL AUTO_INCREMENT`类型。

*如果使用INT自增类型，那么当一张表的记录数超过2147483647（约21亿）时，会达到上限而出错。使用BIGINT自增类型则可以最多约922亿亿条记录。*

关系数据库实际上还**允许通过多个字段唯一标识记录**，即两个或更多的字段都设置为主键，这种主键被称为**联合主键**。没有必要的情况下，我们**尽量不使用**联合主键，因为它给关系表带来了**复杂度的上升**。

**总结**：主键是关系表中记录的唯一标识。主键的选取非常重要：主键不要带有业务含义，而应该使用BIGINT自增或者GUID类型。主键也不应该允许`NULL`



## 外键

students表

| id   | class_id | name | other columns... |
| ---- | -------- | ---- | ---------------- |
| 1    | 1        | 小明 | ...              |
| 2    | 2        | 小红 | ...              |
| 5    | 2        | 小白 | ...              |

在`students`表中，通过`class_id`的字段，可以把数据与另一张表关联起来，这种列称为`外键`。

外键并不是通过列名实现的，而是通过定义外键约束实现的：

```mysql
ALTER TABLE students
ADD CONSTRAINT fk_class_id
FOREIGN KEY (class_id)
REFERENCES classes (id);
```

其中，外键约束的名称`fk_class_id`可以任意，`FOREIGN KEY (class_id)`指定了`class_id`作为外键，`REFERENCES classes (id)`指定了这个外键将关联到`classes`表的`id`列（即`classes`表的主键）。

通过定义外键约束，关系数据库可以保证无法插入无效的数据。即如果`classes`表不存在`id=99`的记录，`students`表就无法插入`class_id=99`的记录。

**由于外键约束会降低数据库的性能，大部分互联网应用程序为了追求速度，并不设置外键约束，而是仅靠应用程序自身来保证逻辑的正确性。**这种情况下，`class_id`仅仅是一个普通的列，只是它起到了外键的作用而已。

要删除一个外键约束，也是通过`ALTER TABLE`实现的：

```mysql
ALTER TABLE students
DROP FOREIGN KEY fk_class_id;
```

注意：删除外键约束并没有删除外键这一列。删除列是通过`DROP COLUMN ...`实现的。

### 多对多

**多对多**关系实际上是通过两个一对多关系实现的，即**通过一个中间表，关联两个一对多关系**，就形成了多对多关系：

`teachers`表：

| id   | name   |
| :--- | :----- |
| 1    | 张老师 |
| 2    | 王老师 |
| 3    | 李老师 |
| 4    | 赵老师 |

`classes`表：

| id   | name |
| :--- | :--- |
| 1    | 一班 |
| 2    | 二班 |

中间表`teacher_class`关联两个一对多关系：

| id   | teacher_id | class_id |
| :--- | :--------- | :------- |
| 1    | 1          | 1        |
| 2    | 1          | 2        |
| 3    | 2          | 1        |
| 4    | 2          | 2        |
| 5    | 3          | 1        |

 通过中间表`teacher_class`可知`teachers`到`classes`的关系：

- `id=1`的张老师对应`id=1,2`的一班和二班；
- `id=2`的王老师对应`id=1,2`的一班和二班；

同理可知`classes`到`teachers`的关系：

- `id=1`的一班对应`id=1,2,3`的张老师、王老师和李老师；
- `id=2`的二班对应`id=1,2,4`的张老师、王老师和赵老师；

因此，通过中间表，我们就定义了一个“多对多”关系。

### 一对一

一对一关系是指，一个表的记录对应到另一个表的唯一一个记录。

例如，`students`表的每个学生可以有自己的联系方式，如果把联系方式存入另一个表`contacts`，我们就可以得到一个“一对一”关系：

既然是一对一关系，那为啥不给`students`表增加一个`mobile`列，这样就能合二为一了？

如果业务允许，完全可以把两个表合为一个表。但是，有些时候，如果某个学生没有手机号，那么，`contacts`表就不存在对应的记录。

实际上，一对一关系准确地说，是`contacts`表一对一对应`students`表。

还有**一些应用会把一个大表拆成两个一对一的表，目的是把经常读取和不经常读取的字段分开，以获得更高的性能。**例如，把一个大的用户表分拆为用户基本信息表`user_info`和用户详细信息表`user_profiles`，大部分时候，只需要查询`user_info`表，并不需要查询`user_profiles`表，这样就提高了查询速度。

### 小结

关系数据库通过外键可以实现一对多、多对多和一对一的关系。外键既可以通过数据库来约束，也可以不设置约束，仅依靠应用程序的逻辑来保证。

## 索引

索引是关系数据库中对某一列或多个列的值进行预排序的数据结构。通过使用索引，可以让数据库系统不必扫描整个表，而是直接定位到符合条件的记录，这样就大大加快了查询速度。

例如，对于`students`表：

| id   | class_id | name | gender | score |
| :--- | :------- | :--- | :----- | :---- |
| 1    | 1        | 小明 | M      | 90    |
| 2    | 1        | 小红 | F      | 95    |
| 3    | 1        | 小军 | M      | 88    |

如果要经常根据`score`列进行查询，就可以对`score`列创建索引：

```sql
ALTER TABLE students
ADD INDEX idx_score (score);
```

索引的效率取决于索引列的值是否散列，即该列的值如果越互不相同，那么索引效率越高。对于大量相同值的列无效，如性别。

### 唯一索引

在设计关系数据表的时候，有些看似唯一的列（身份证），由于具有业务含义，不能作为主键；对于某一列要求具有唯一性约束：

例如，我们假设`students`表的`name`不能重复：

**方法1:**

```sql
ALTER TABLE students
ADD UNIQUE INDEX uni_name (name);
```

通过`UNIQUE`关键字我们就添加了一个唯一索引。

**方法2:**

也可以只对某一列添加一个唯一约束而不创建唯一索引：

```sql
ALTER TABLE students
ADD CONSTRAINT uni_name UNIQUE (name);
```

这种情况下，`name`列没有索引，但仍然具有唯一性保证。

### 总结：

索引就是为了加快查询速度的；

通过创建唯一索引，可以保证某一列的值具有唯一性。

数据库索引对于用户和应用程序来说都是透明的。

# 3-查询

## 普通查询

要查询数据库表的数据，我们使用如下的SQL语句：

```sql
SELECT * FROM <表名>
```

使用`SELECT * FROM students`时，`SELECT`是关键字，表示将要执行一个查询，`*`表示“所有列”，`FROM`表示将要从哪个表查询，本例中是`students`表。

该SQL将查询出`students`表的所有数据。注意：**查询结果也是一个二维表，它包含列名和每一行的数据。**

`SELECT`语句其实并不要求一定要有`FROM`子句。我们来试试下面的`SELECT`语句：`select 100+200`     结果是`300`

不带`FROM`子句的`SELECT`语句有一个有用的用途，就是用来判断当前到数据库的连接是否有效。许多检测工具会执行一条`SELECT 1;`来测试数据库连接。

## 条件查询

SELECT语句可以通过`WHERE`条件来设定查询条件，查询结果是满足查询条件的记录。

```sql
SELECT * FROM <表名> WHERE <条件表达式>
```

例如，要指定条件“分数在80分或以上的学生”，写成`WHERE`条件就是：

```sql
SELECT * FROM students WHERE score >= 80
```

条件表达式可以用`<条件1> AND <条件2>`表达满足条件1并且满足条件2:

```sql
SELECT * FROM students WHERE score >= 80 AND gender = 'M';
```

`<条件1> OR <条件2>`，表示满足条件1或者满足条件2:

```sql
SELECT * FROM students WHERE score >= 80 OR gender = 'M';
```

条件`NOT class_id = 2`其实等价于`class_id <> 2`，因此，`NOT`查询不是很常用。

要组合三个或者更多的条件，就需要用小括号`()`表示如何进行条件运算。

```sql
SELECT * FROM students WHERE (score < 80 OR score > 90) AND gender = 'M';
```

如果不加括号，条件运算按照`NOT`、`AND`、`OR`的优先级进行，即`NOT`优先级最高，其次是`AND`，最后是`OR`。加上括号可以改变优先级。

**常用的条件表达式**

| 条件                 | 表达式举例1     | 表达式举例2      | 说明                                              |
| :------------------- | :-------------- | :--------------- | :------------------------------------------------ |
| 使用=判断相等        | score = 80      | name = 'abc'     | 字符串需要用单引号括起来                          |
| 使用>判断大于        | score > 80      | name > 'abc'     | 字符串比较根据ASCII码，中文字符比较根据数据库设置 |
| 使用>=判断大于或相等 | score >= 80     | name >= 'abc'    |                                                   |
| 使用<判断小于        | score < 80      | name <= 'abc'    |                                                   |
| 使用<=判断小于或相等 | score <= 80     | name <= 'abc'    |                                                   |
| **使用<>判断不相等** | score <> 80     | name <> 'abc'    |                                                   |
| **使用LIKE判断相似** | name LIKE 'ab%' | name LIKE '%bc%' | %表示任意字符，例如'ab%'将匹配'ab'，'abc'，'abcd' |

查询分数在60分(含)～90分(含)

```sql
SELECT * FROM students WHERE score >= 60 AND score <= 90
SELECT * FROM students  WHERE score BETWEEN 60 AND 90
```

## 投影查询

让结果集仅包含指定列。这种操作称为**投影查询**

```sql
SELECT 列1, 列2, 列3 FROM ...
SELECT id, score, name FROM students;
```

## 排序

查询结果集通常是按照`id`排序的，也就是主键排序，`ORDER BY`子句按其他列排序，默认高到低，加上`DESC`表示“倒序”，如果`score`列有相同的数据，要进一步排序，可以继续添加列名。

```sql
SELECT id, name, gender, score 
FROM students 
ORDER BY score DESC, gender;
```

默认的排序规则是`ASC`：“升序”，即从小到大。`ASC`可以省略，即`ORDER BY score ASC`和`ORDER BY score`效果一样。如果有`WHERE`子句，那么`ORDER BY`子句要放到`WHERE`子句后面。

```sql
select id,name,gender,score from students
where class_id = 1
order by score desc ,gender;
```

## 分页查询

分页实际上就是从结果集中“截取”出第M~N条记录。这个查询可以通过`LIMIT <M> OFFSET <N>`子句实现.

- `LIMIT`总是设定为`pageSize`；
- `OFFSET`计算公式为`pageSize * (pageIndex - 1)`。
- `OFFSET`超过了查询的最大数量并不会报错，而是得到一个空的结果集。

```sql
select id,name,gender,score from students
order by score desc
limit 5 offset 0;       # 每页5条第1页
```

## 聚合查询

对于统计总数、平均数这类计算，SQL提供了专门的聚合函数，使用聚合函数进行查询，就是聚合查询，它可以快速获得结果。

```sql
SELECT COUNT(*) FROM students;		#统计个数，COUNT(*)和COUNT(id)实际上是一样的效果。
SELECT COUNT(*) num FROM students;	#统计并设别名num
```

除了`COUNT()`函数外，SQL还提供了如下聚合函数：

| 函数    | 说明                                                   |
| :------ | :----------------------------------------------------- |
| SUM     | 计算某一列的合计值，该列必须为数值类型                 |
| AVG     | 计算某一列的平均值，该列必须为数值类型                 |
| MAX     | 计算某一列的最大值                                     |
| MIN     | 计算某一列的最小值                                     |
| COUNT   | 统计总个数                                             |
| ROUND   | 四舍五入一个正数或者负数，结果为一定长度的值           |
| CEILING | 返回最小的整数，使这个整数大于或等于指定数的数值运算。 |
| FLOOR   | 返回最大整数，使这个整数小于或等于指定数的数值运算。   |

注意，`MAX()`和`MIN()`函数并不限于数值类型。如果是字符类型，`MAX()`和`MIN()`会返回排序最后和排序最前的字符。

要特别注意：如果聚合查询的`WHERE`条件没有匹配到任何行，`COUNT()`会返回0，而`SUM()`、`AVG()`、`MAX()`和`MIN()`会返回`NULL`：



## 多表查询

SELECT查询不但可以从一张表查询数据，还可以从多张表同时查询数据。查询多张表的语法是：`SELECT * FROM <表1> <表2>`。

这种一次查询两个表的数据，查询的结果也是一个二维表，它是`students`表和`classes`表的“乘积”，即`students`表的每一行与`classes`表的每一行都两两拼在一起返回。结果集的列数是`students`表和`classes`表的列数之和，行数是`students`表和`classes`表的行数之积。使用多表查询可以获取M x N行记录；这种多表查询又称笛卡尔查询。多表查询的结果集可能非常巨大，要小心使用。

| id   | class_id | name | gender | score | id   | name |
| :--- | :------- | :--- | :----- | :---- | :--- | :--- |
| 1    | 1        | 小明 | M      | 90    | 1    | 一班 |
| 1    | 1        | 小明 | M      | 90    | 2    | 二班 |
| 1    | 1        | 小明 | M      | 90    | 3    | 三班 |
| 1    | 1        | 小明 | M      | 90    | 4    | 四班 |
| 2    | 1        | 小红 | F      | 95    | 1    | 一班 |
| 2    | 1        | 小红 | F      | 95    | 2    | 二班 |
| 2    | 1        | 小红 | F      | 95    | 3    | 三班 |
| 2    | 1        | 小红 | F      | 95    | 4    | 四班 |

其中：students表 和 class表 都有id,列名重复，可以用区别名的方式解决。

```sql
SELECT
    students.id sid,
	classes.id cid,
    classes.name cname
FROM students, classes;
```

注意，多表查询时，要使用`表名.列名`这样的方式来引用列和设置别名，这样就避免了结果集的列名重复问题。但是，用`表名.列名`这种方式列举两个表的所有列实在是很麻烦，所以SQL还允许给表设置一个别名，让我们在投影查询中引用起来稍微简洁一点：

```sql
SELECT
    s.id sid,
    c.id cid,
    c.name cname
FROM students s, classes c;
```

注意到`FROM`子句给表设置别名的语法是`FROM <表名1> <别名1>, <表名2> <别名2>`。

## 连接查询

连接查询是另一种类型的多表查询。连接查询对多个表进行JOIN运算，简单地说，就是先确定一个主表作为结果集，然后，把其他表的行有选择性地“连接”在主表结果集上。

```sql
SELECT s.id, s.name, s.class_id, s.gender, s.score FROM students s;

SELECT s.id, s.name, s.class_id, c.name class_name, s.gender, s.score FROM students s
INNER JOIN classes c ON s.class_id = c.id;		#将class表中的数据接到students表上
```

注意INNER JOIN查询的写法是：

1. 先确定主表，仍然使用`FROM <表1>`的语法；
2. 再确定需要连接的表，使用`INNER JOIN <表2>`的语法；
3. 然后确定连接条件，使用`ON <条件...>`，这里的条件是`s.class_id = c.id`，表示`students`表的`class_id`列与`classes`表的`id`列相同的行需要连接；
4. 可选：加上`WHERE`子句、`ORDER BY`等子句。



有RIGHT OUTER JOIN，就有LEFT OUTER JOIN，以及FULL OUTER JOIN。它们的区别是：

INNER JOIN只返回同时存在于两张表的行数据，由于`students`表的`class_id`包含1，2，3，`classes`表的`id`包含1，2，3，4，所以，INNER JOIN根据条件`s.class_id = c.id`返回的结果集仅包含1，2，3。

RIGHT OUTER JOIN返回右表都存在的行。如果某一行仅在右表存在，那么结果集就会以`NULL`填充剩下的字段。

LEFT OUTER JOIN则返回左表都存在的行。如果我们给students表增加一行，并添加class_id=5，由于classes表并不存在id=5的行，所以，LEFT OUTER JOIN的结果会增加一行，对应的`class_name`是`NULL`：

对于这么多种JOIN查询，到底什么使用应该用哪种呢？其实我们用图来表示结果集就一目了然了。

假设查询语句是：

```
SELECT ... FROM tableA ??? JOIN tableB ON tableA.column1 = tableB.column2;
```

我们把tableA看作左表，把tableB看成右表，那么INNER JOIN是选出两张表都存在的记录：

![inner-join](https://www.liaoxuefeng.com/files/attachments/1246892164662976/l)

LEFT OUTER JOIN是选出左表存在的记录：

![left-outer-join](https://www.liaoxuefeng.com/files/attachments/1246893588481376/l)

RIGHT OUTER JOIN是选出右表存在的记录：

![right-outer-join](https://www.liaoxuefeng.com/files/attachments/1246893609222688/l)

FULL OUTER JOIN则是选出左右表都存在的记录：

![full-outer-join](https://www.liaoxuefeng.com/files/attachments/1246893632359424/l)

# 4-修改

关系数据库的基本操作就是增删改查，即CRUD：Create、Retrieve、Update、Delete。，而对于增、删、改，对应的SQL语句分别是：

- INSERT：插入新记录；
- UPDATE：更新已有记录；
- DELETE：删除已有记录。

## INSERT

`INSERT`语句的基本语法是：

```sql
INSERT INTO <表名> (字段1, 字段2, ...) VALUES (值1, 值2, ...);

INSERT INTO students (class_id, name, gender, score) VALUES
  (1, '大宝', 'M', 87),
  (2, '二宝', 'M', 81);
```

## UPDATE

`UPDATE`语句的基本语法是：

```sql
UPDATE <表名> SET 字段1=值1, 字段2=值2, ... WHERE ...;
-- SET score=score+10就是给当前行的score字段的值加上了10。
UPDATE students SET score=score+10 WHERE score<80;
```

使用`UPDATE`，我们就可以一次更新表中的一条或多条记录。

## DELETE

`DELETE`语句的基本语法是：

```sql
DELETE FROM <表名> WHERE ...;
```

```sql
-- 删除id=1的记录
DELETE FROM students WHERE id=1;
```

注意到`DELETE`语句的`WHERE`条件也是用来筛选需要删除的行，因此和`UPDATE`类似，`DELETE`语句也可以一次删除多条记录：

如果`WHERE`条件没有匹配到任何记录，`DELETE`语句不会报错，也不会有任何记录被删除。

最后，要特别小心的是，和`UPDATE`类似，不带`WHERE`条件的`DELETE`语句会删除整个表的数据：

# 5-MySQL

## 管理MySQL

打开命令提示符，输入命令`mysql -u root -p`，提示输入口令。填入MySQL的root口令，如果正确，就连上了MySQL Server，同时提示符变为`mysql>`：

```ascii
┌────────────────────────────────────────────────────────┐
│Command Prompt                                    - □ x │
├────────────────────────────────────────────────────────┤
│Microsoft Windows [Version 10.0.0]                      │
│(c) 2015 Microsoft Corporation. All rights reserved.    │
│                                                        │
│C:\> mysql -u root -p                                   │
│Enter password: ******                                  │
│                                                        │
│Server version: 5.7                                     │
│Copyright (c) 2000, 2018, ...                           │
│Type 'help;' or '\h' for help.                          │
│                                                        │
│mysql>                                                  │
│                                                        │
└────────────────────────────────────────────────────────┘
```

输入`exit`断开与MySQL Server的连接并返回到命令提示符。

MySQL Client和MySQL Server的关系如下：

```ascii
┌──────────────┐  SQL   ┌──────────────┐
│ MySQL Client │───────>│ MySQL Server │
└──────────────┘  TCP   └──────────────┘
```

在MySQL Client中输入的SQL语句通过TCP连接发送到MySQL Server。默认端口号是3306，即如果发送到本机MySQL Server，地址就是`127.0.0.1:3306`。

也可以只安装MySQL Client，然后连接到远程MySQL Server。假设远程MySQL Server的IP地址是`10.0.1.99`，那么就使用`-h`指定IP或域名：

```bash
mysql -h 10.0.1.99 -u root -p
```

命令行程序`mysql`实际上是MySQL客户端，真正的MySQL服务器程序是`mysqld`，在后台运行。

### 数据库

在一个运行MySQL的服务器上，实际上可以创建多个数据库（Database）。要列出所有数据库，使用命令：

```mysql
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| shici              |
| sys                |
| test               |
| school             |
+--------------------+
```

其中，`information_schema`、`mysql`、`performance_schema`和`sys`是系统库，不要去改动它们。其他的是用户创建的数据库。

要创建一个新数据库，使用命令：

```mysql
mysql> CREATE DATABASE test;
Query OK, 1 row affected (0.01 sec)
```

要删除一个数据库，使用命令：

```mysql
mysql> DROP DATABASE test;
Query OK, 0 rows affected (0.01 sec)
```

注意：删除一个数据库将导致该数据库的所有表全部被删除。

对一个数据库进行操作时，要首先将其切换为当前数据库：

```mysql
mysql> USE test;
Database changed
```

### 表

列出当前数据库的所有表，使用命令：

```mysql
mysql> SHOW TABLES;
+---------------------+
| Tables_in_test      |
+---------------------+
| classes             |
| statistics          |
| students            |
| students_of_class1  |
+---------------------+
```

要查看一个表的结构，使用命令：

```mysq
mysql> DESC students;
+----------+--------------+------+-----+---------+----------------+
| Field    | Type         | Null | Key | Default | Extra          |
+----------+--------------+------+-----+---------+----------------+
| id       | bigint(20)   | NO   | PRI | NULL    | auto_increment |
| class_id | bigint(20)   | NO   |     | NULL    |                |
| name     | varchar(100) | NO   |     | NULL    |                |
| gender   | varchar(1)   | NO   |     | NULL    |                |
| score    | int(11)      | NO   |     | NULL    |                |
+----------+--------------+------+-----+---------+----------------+
5 rows in set (0.00 sec)
```

```mysql
mysql> SHOW CREATE TABLE students;
+----------+-------------------------------------------------------+
| students | CREATE TABLE `students` (                             |
|          |   `id` bigint(20) NOT NULL AUTO_INCREMENT,            |
|          |   `class_id` bigint(20) NOT NULL,                     |
|          |   `name` varchar(100) NOT NULL,                       |
|          |   `gender` varchar(1) NOT NULL,                       |
|          |   `score` int(11) NOT NULL,                           |
|          |   PRIMARY KEY (`id`)                                  |
|          | ) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 |
+----------+-------------------------------------------------------+
1 row in set (0.00 sec)
```

创建表使用`CREATE TABLE`语句，而删除表使用`DROP TABLE`语句：

```mysql
mysql> DROP TABLE students;
Query OK, 0 rows affected (0.01 sec)
```

修改表就比较复杂。如果要给`students`表新增一列`birth`，使用：

```mysql
ALTER TABLE students ADD COLUMN birth VARCHAR(10) NOT NULL;
```

要修改`birth`列，例如把列名改为`birthday`，类型改为`VARCHAR(20)`：

```mysql
ALTER TABLE students CHANGE COLUMN birth birthday VARCHAR(20) NOT NULL;
```

要删除列，使用：

```mysql
ALTER TABLE students DROP COLUMN birthday;
```

### 退出MySQL

使用`EXIT`命令退出MySQL：

```mysql
mysql> EXIT
Bye
```

注意`EXIT`仅仅断开了客户端和服务器的连接，MySQL服务器仍然继续运行。