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
    -> ( '小红帽', '五年级2班', 'F');  #此处不输入g2_id,因为该字段在创建表时，已设置为```AUTO_INCREMENT(自动增加) ```属性。 故，该字段会自动递增我们可以不去设置。实际上，大部分上述情况时该字段都省略。
Query OK, 1 row affected (0.30 sec)

mysql> INSERT INTO gtb2
    -> (g2_name, g2_class, g2_gender)   
    -> VALUES
    -> ( '大乔', '五年级1班', default);   #此处```g2_gender```字段我们用```DEFAULT```来代替，因为该字段在创建表时，已设置为```DEFAULT 'M'```故此字段讲显示默认设置。
Query OK, 1 row affected (0.40 sec)

mysql> SELECT * FROM gtb2;    #读取数据表！
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





