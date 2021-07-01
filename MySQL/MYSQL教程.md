# 使用MySQL

## 连接MYSQL

```
mysql -uroot -p
```

用户名`root`与`-u`之间空格可有可无，

`-p`后可直接接密码，但注意**不要有空格**，

`-D`接上数据库名称，如果在sql脚本文件中使用了`use 数据库`，则`-D`选项可以忽略, 手动链接的话需要带上，否则无法访问数据库的信息。

若终端中只能用`sudo mysql -u root -p`才能连接。用`mysql -u root -p`连接就会报`ERROR 1698 (28000): Access denied for user 'root'@'localhost'`错误。

运行

```bash
sudo vim /etc/mysql/my.cnf
```


添加

```
[mysqld]
skip-grant-tables
```


进入mysql后修改密码

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '密码';
```


重新进入，ok。


## 修改密码

```
mysqladmin -u用户名 -p旧密码 password 新密码

mysqladmin -uroot -password 123456
mysqladmin -uroot -p123 password 456789
```



## 导入.sql文件

命令行中：

```
mysql -u root -p < init-test-data.sql
```

mysql中：

```
mysql> source  d:/myprogram/database/db.sql;
```