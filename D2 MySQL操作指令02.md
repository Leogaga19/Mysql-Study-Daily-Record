## MySQL操作指令02

### 1.显示数据库中的全部表
命令：```mysql> SHOW TABLES;```  示例：

```
mysql> show tables;
+---------------+
| Tables_in_abc |
+---------------+
| gtb1          |
+---------------+
1 row in set (0.21 sec)
```

### 2.新建数据表
命令： ```CREATE TABLE table_name (column1_name column1_限制条件, column2_name column2_限制条件,……, PRIMARY KEY( column1_name ))```

创建数据表需要明确以下条件：1.表名称；2.字段名称；3.字段限制条件；4.明确主键。示例：
```
mysql> CREATE TABLE gtb2(
    -> g2_id INT NOT NULL AUTO_INCREMENT,
    -> g2_name CHAR(50) NOT NULL,
    -> g2_class CHAR(50) NOT NULL,
    -> g2_gender CHAR(1) DEFAULT 'M',
    -> PRIMARY KEY ( g2_id )
    -> )ENGINE=InnoDB DEFAULT CHARSET=utf8;
Query OK, 0 rows affected, 1 warning (1.07 sec)

mysql> show tables;
+---------------+
| Tables_in_abc |
+---------------+
| gtb1          |
| gtb2          |
+---------------+
2 rows in set (0.00 sec)
```
上面在数据库```abc```中新创建```gtb2```数据表，分析如下：

1.如果你不想字段为 NULL 可以设置字段的属性为 NOT NULL，在操作数据库时如果输入该字段的数据为NULL ，就会报错；

2.AUTO_INCREMENT定义列为自增的属性，一般用于主键，数值会自动加1，这样就避免未来主键冲突；

3.PRIMARY KEY关键字用于定义列为主键。 您可以使用多列来定义主键及联合主键，列间以逗号分隔；

4.ENGINE 设置存储引擎，存储引擎：决定了数据如何存储以及如何访问，还有事务如何处理

MySQL允许对每个表使用不同的存储引擎，如果在create table语句中没有指定存储引擎，则使用默认的存储引擎。
```
mysql> show engines;    #查询所有支持的存储引擎

mysql> CREATE TABLE sexes(sex char(1) NOT NULL) ENGINE = INNODB; 
```
注意：存储引擎是个重点，后面我们详细讲解。CHARSET 设置编码

### 3.显示数据表结构
命令```mysql> DESCRIBE table_name;``` 示例：
```
mysql> DESCRIBE gtb2;
+-----------+----------+------+-----+---------+----------------+
| Field     | Type     | Null | Key | Default | Extra          |
+-----------+----------+------+-----+---------+----------------+
| g2_id     | int(11)  | NO   | PRI | NULL    | auto_increment |
| g2_name   | char(50) | NO   |     | NULL    |                |
| g2_class  | char(50) | NO   |     | NULL    |                |
| g2_gender | char(1)  | YES  |     | M       |                |
+-----------+----------+------+-----+---------+----------------+
4 rows in set (0.00 sec)
```

### 4.删除数据表
命令：```DROP TABLE table_name;```

注意：删除数据表和数据表一定要谨慎，删除后所有数据就消失！示例：
```
mysql> DROP TABLE gtbl;
Query OK, 0 rows affected (0.8 sec)
mysql>
```

### 5.MySQL数据表中插入数据
命令：```INSERT INTO table_name (column1_name, column2_name,……columnN_name） VALUES ("value1, value2,……valueN)``` 示例：
```
mysql> INSERT INTO gtb2 (g2_id, g2_name, g2_class, g2_gender)
    -> VALUES
    -> ('0', '小明', '五年级一班', 'F');
Query OK, 1 row affected (0.64 sec)

mysql> INSERT INTO gtb2
    -> (g2_id, g2_name, g2_class, g2_gender)
    -> VALUES
    -> ('0', '张飞', '五年级1班', 'M');
Query OK, 1 row affected (0.16 sec)

mysql> INSERT INTO gtb2
    -> (g2_id, g2_name, g2_class, g2_gender)
    -> VALUES
    -> ('0', '关羽', '五年级2班', 'M');
Query OK, 1 row affected (0.24 sec)
………………
mysql> INSERT INTO gtb2
    -> (g2_name, g2_class, g2_gender)
    -> VALUES
    -> ( '小红帽', '五年级2班', 'F');  #此处不输入g2_id，因为该字段在创建表时已设为AUTO_INCREMENT(自动增加)属性。 故其会自动递增，可以不去设置。实际上，大部分上述情况时该字段都省略。
Query OK, 1 row affected (0.30 sec)

mysql> INSERT INTO gtb2
    -> (g2_name, g2_class, g2_gender)   
    -> VALUES
    -> ( '大乔', '五年级1班', default);   #此处g2_gender字段用DEFAULT来代替，因该字段在创建表时设置为DEFAULT 'M'故此其将显示默认设置。
Query OK, 1 row affected (0.40 sec)

mysql> SELECT * FROM gtb2;    #读取数据表！使用星号（*）来代替其他字段，SELECT语句会返回表的所有字段数据。
+-------+-----------+-----------------+-----------+
| g2_id | g2_name   | g2_class        | g2_gender |
+-------+-----------+-----------------+-----------+
|     1 | 小明      | 五年级一班      | F         |
|     2 | 小刚      | 五年级2班       | F         |
|     3 | 小芳      | 五年级3班       |           |
|     4 | 小红      | 五年级2班       | M         |
|     5 | 刘备      | 五年级1班       | M         |
|     6 | 张飞      | 五年级1班       | M         |
|     7 | 关羽      | 五年级2班       | M         |
|     8 | 曹操      | 五年级3班       | M         |
|     9 | 貂蝉      | 五年级3班       | F         |
|    10 | 洛神      | 五年级3班       | F         |
|    11 | 小红帽    | 五年级2班       | F         |
|    12 | 大乔      | 五年级1班       | M         |
+-------+-----------+-----------------+-----------+
12 rows in set (0.00 sec)
```

### 6.MySQL数据表数据查询
命令：
```
SELECT column1_name, column2_name,……, columnN_name 
FROM table1_name, table2_name,……, tableN_name
[WHERE Clause]
[LIMIT N][ OFFSET M]
```
命令说明：1.查询语句中你可以使用一个或者多个表，表之间使用逗号(,)分割，并使用WHERE语句来设定查询条件；  
2.SELECT 命令可以读取一条或者多条记录；  
3.你可以使用星号（*）来代替其他字段，SELECT语句会返回表的所有字段数据；  
4.你可以使用 WHERE 语句来包含任何条件。  
5.你可以使用 LIMIT 属性来设定返回的记录数。  
6.你可以通过OFFSET指定SELECT语句开始查询的数据偏移量。默认情况下偏移量为0。  

示例：
```
mysql> SELECT g2_name FROM gtb2;
+-----------+
| g2_name   |
+-----------+
| 小明      |
| 小刚      |
| 小芳      |
| 小红      |
| 刘备      |
| 张飞      |
| 关羽      |
| 曹操      |
| 貂蝉      |
| 洛神      |
| 小红帽    |
| 大乔      |
+-----------+
12 rows in set (0.00 sec)

mysql> SELECT * FROM gtb2;
+-------+-----------+-----------------+-----------+
| g2_id | g2_name   | g2_class        | g2_gender |
+-------+-----------+-----------------+-----------+
|     1 | 小明      | 五年级一班      | F         |
|     2 | 小刚      | 五年级2班       | F         |
|     3 | 小芳      | 五年级3班       |           |
|     4 | 小红      | 五年级2班       | M         |
|     5 | 刘备      | 五年级1班       | M         |
|     6 | 张飞      | 五年级1班       | M         |
|     7 | 关羽      | 五年级2班       | M         |
|     8 | 曹操      | 五年级3班       | M         |
|     9 | 貂蝉      | 五年级3班       | F         |
|    10 | 洛神      | 五年级3班       | F         |
|    11 | 小红帽    | 五年级2班       | F         |
|    12 | 大乔      | 五年级1班       | M         |
+-------+-----------+-----------------+-----------+
12 rows in set (0.00 sec)
```

### 7.MySQL数据表WHERE条件数据查询
命令：
```
SELECT column1_name, column2_name,……, columnN_name 
FROM table1_name, table2_name,……, tableN_name
[WHERE Clause]
[LIMIT N][ OFFSET M]
```
1. WHERE 子句中指定任何条件，如果条件为字符需要用‘’或者""来框住；  
2. 使用 AND 或者 OR 指定一个或多个条件；  
3. WHERE 子句也可以运用于 SQL 的 DELETE 或者 UPDATE 命令；  
4. WHERE 子句类似于程序语言中的 if 条件，根据 MySQL 表中的字段值来读取指定的数据。  
以下为操作符列表，可用于 WHERE 子句中。下表中实例假定 A 为 10, B 为 20  
![image](https://github.com/Leogaga19/Mysql-Study-Daily-Record/blob/master/Photos/WHERE%E9%80%BB%E8%BE%91%E6%93%8D%E4%BD%9C%E7%AC%A6.PNG)
如果我们想在 MySQL 数据表中读取指定的数据，WHERE 子句是非常有用的。  
使用主键来作为 WHERE 子句的条件查询是非常快速的。  
如果给定的条件在表中没有任何匹配的记录，那么查询不会返回任何数据。
示例：
```
mysql> SELECT * FROM gtb2
    -> WHERE g2_class='五年级2班';
+-------+-----------+---------------+-----------+
| g2_id | g2_name   | g2_class      | g2_gender |
+-------+-----------+---------------+-----------+
|     2 | 小刚      | 五年级2班     | F         |
|     4 | 小红      | 五年级2班     | M         |
|     7 | 关羽      | 五年级2班     | M         |
|    11 | 小红帽    | 五年级2班     | F         |
+-------+-----------+---------------+-----------+
4 rows in set (0.00 sec)

mysql> SELECT g2_name FROM gtb2
    -> WHERE g2_class='五年级2班';
+-----------+
| g2_name   |
+-----------+
| 小刚      |
| 小红      |
| 关羽      |
| 小红帽    |
+-----------+
4 rows in set (0.00 sec)

mysql> SELECT g2_name FROM gtb2
    -> WHERE g2_class='五年级2班' AND g2_gender<>'F';
+---------+
| g2_name |
+---------+
| 小红    |
| 关羽    |
+---------+
2 rows in set (0.00 sec)
```
