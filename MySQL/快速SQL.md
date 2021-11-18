通过SQL的相关命令记忆内容

```sql
SELECT - 从数据库中提取数据
UPDATE - 更新数据库中的数据
DELETE - 从数据库中删除数据
INSERT INTO - 向数据库中插入新数据
CREATE DATABASE - 创建新数据库
ALTER DATABASE - 修改数据库
CREATE TABLE - 创建新表
ALTER TABLE - 变更（改变）数据库表
DROP TABLE - 删除表
CREATE INDEX - 创建索引（搜索键）
DROP INDEX - 删除索引
```

```sql
use RUNOOB; 		#命令用于选择数据库。
set names utf8; 	#命令用于设置使用的字符集。
SELECT * FROM Websites; 	#读取数据表的信息。
```

- SQL 对大小写不敏感：SELECT 与 select 是相同的。
- 分号是在数据库系统中分隔每条 SQL 语句的标准方法，这样就可以在对服务器的相同请求中执行一条以上的 SQL 语句。

## 创建

```sql
CREATE DATABASE my_db;		#数据库表可以通过 CREATE TABLE 语句来添加。

CREATE TABLE table_name(column_name1 data_type(size), column_name2 data_type(size), column_name3 data_type(size), .... );

CREATE TABLE Persons ( PersonID int, LastName varchar(255), FirstName varchar(255), Address varchar(255), City varchar(255) );
```

## 查询

```sql
#查询数据表的全部列信息。
SELECT * FROM Websites; 	
#加条件
SELECT * FROM students WHERE (score < 80 OR score > 90) AND gender = 'M';
SELECT * FROM students WHERE score >= 60 AND score <= 90;
SELECT * FROM students  WHERE score BETWEEN 60 AND 90;
```

如果不加括号，条件运算按照`NOT`、`AND`、`OR`的优先级进行，即`NOT`优先级最高，其次是`AND`，最后是`OR`。加上括号可以改变优先级。

```sql
#查询指定列信息，加排序
select id,name,gender,score from students where class_id = 1 order by score desc ,gender;
```

默认的排序规则是`ASC`：“升序”，即从小到大。加上`DESC`表示“倒序”，如果`score`列有相同的数据，要进一步排序，可以继续添加列名。`ASC`可以省略，即`ORDER BY score ASC`和`ORDER BY score`效果一样。如果有`WHERE`子句，那么`ORDER BY`子句要放到`WHERE`子句后面。

```sql
select id,name,gender,score from students order by score desc limit 5 offset 0;       # 每页5条第1页
```

```sql
SELECT COUNT(*) FROM students;		#统计个数，COUNT(*)和COUNT(id)实际上是一样的效果。
SELECT DISTINCT country FROM Websites;		#去重查询
SELECT COUNT(*) num FROM students;	#统计并设别名num
SELECT AVG(score) average FROM students WHERE gender = 'M';
```

除了`COUNT()`函数外，SQL还提供了如下聚合函数：

| 函数    | 说明                                                   |
| ------- | ------------------------------------------------------ |
| SUM     | 计算某一列的合计值，该列必须为数值类型                 |
| AVG     | 计算某一列的平均值，该列必须为数值类型                 |
| MAX     | 计算某一列的最大值                                     |
| MIN     | 计算某一列的最小值                                     |
| COUNT   | 统计总个数                                             |
| ROUND   | 四舍五入一个正数或者负数，结果为一定长度的值           |
| CEILING | 返回最小的整数，使这个整数大于或等于指定数的数值运算。 |
| FLOOR   | 返回最大整数，使这个整数小于或等于指定数的数值运算。   |

```sql
#各个班级的学生人数：
SELECT class_id, COUNT(*) num FROM students GROUP BY class_id;
```

`GROUP BY`子句指定了按`class_id`分组，因此，执行该`SELECT`语句时，会把`class_id`相同的列先分组，再分别计算

```sql
SELECT students.id sid, classes.id cid, classes.name cname FROM students, classes;
SELECT s.id sid, c.id cid, c.name cname FROM students s, classes c;
```

注意，多表查询时，要使用`表名.列名`这样的方式来引用列和设置别名，这样就避免了结果集的列名重复问题。

```sql
SELECT s.id, s.name, s.class_id, c.name class_name, s.gender, s.score FROM students s INNER JOIN classes c ON s.class_id = c.id;		#将class表中的数据接到students表上
```

注意INNER JOIN查询的写法是：

1. 先确定主表，仍然使用`FROM <表1>`的语法；
2. 再确定需要连接的表，使用`INNER JOIN <表2>`的语法；
3. 然后确定连接条件，使用`ON <条件...>`，这里的条件是`s.class_id = c.id`，表示`students`表的`class_id`列与`classes`表的`id`列相同的行需要连接；
4. 可选：加上`WHERE`子句、`ORDER BY`等子句。

## 插入

```sql
INSERT INTO <表名> (字段1, 字段2, ...) VALUES (值1, 值2, ...);

INSERT INTO students (class_id, name, gender, score) VALUES
  (1, '大宝', 'M', 87),
  (2, '二宝', 'M', 81);
```

## 更新

```sql
UPDATE <表名> SET 字段1=值1, 字段2=值2, ... WHERE ...;

-- SET score=score+10就是给当前行的score字段的值加上了10。
UPDATE students SET score=score+10 WHERE score<80;
```

使用`UPDATE`，我们就可以一次更新表中的一条或多条记录。