## MySQL操作指令03

### 1.MySQL数据表数据查询
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

### 2.MySQL数据表WHERE条件数据查询
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

使用主键来作为 WHERE 子句的条件查询非常快速。  
**MySQL 的 WHERE 子句的字符串比较是不区分大小写的。 你可以使用 BINARY 关键字来设定 WHERE 子句的字符串比较是区分大小写的。** 
```
mysql> SELECT * FROM gtb1
    -> WHERE BINARY g_author='lwy';
+------+---------+----------+-----------------+
| g_id | g_title | g_author | submission_date |
+------+---------+----------+-----------------+
|    9 | 自学    | lwy      | 2019-06-23      |
+------+---------+----------+-----------------+
1 row in set (0.00 sec)

mysql> SELECT * FROM gtb1
    -> WHERE g_author='lwy';
+------+---------+----------+-----------------+
| g_id | g_title | g_author | submission_date |
+------+---------+----------+-----------------+
|    3 | 练习    | LWY      | 2019-06-22      |
|    9 | 自学    | lwy      | 2019-06-23      |
+------+---------+----------+-----------------+
2 rows in set (0.00 sec)
```

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

### 2.MySQL更新数据UPDATE
```
ysql> UPDATE gtb2 SET
    -> g2_gender = 'M';
Query OK, 12 rows affected (0.33 sec)
Rows matched: 12  Changed: 12  Warnings: 0

mysql> SELECT * FROM gtb2;
+-------+-----------+-----------------+-----------+
| g2_id | g2_name   | g2_class        | g2_gender |
+-------+-----------+-----------------+-----------+
|     1 | 小明      | 五年级一班      | M         |
|     2 | 小刚      | 五年级2班       | M         |
|     3 | 小芳      | 五年级3班       | M         |
|     4 | 小红      | 五年级2班       | M         |
|     5 | 刘备      | 五年级1班       | M         |
|     6 | 张飞      | 五年级1班       | M         |
|     7 | 关羽      | 五年级2班       | M         |
|     8 | 曹操      | 五年级3班       | M         |
|     9 | 貂蝉      | 五年级3班       | M         |
|    10 | 洛神      | 五年级3班       | M         |
|    11 | 小红帽    | 五年级2班       | M         |
|    12 | 大乔      | 五年级1班       | M         |
+-------+-----------+-----------------+-----------+
12 rows in set (0.00 sec)

mysql> UPDATE gtb2 SET
    -> g2_gender = CASE g2_id
    -> WHEN 1 THEN 'F'
    -> WHEN 2 THEN 'F'
    -> END
    -> WHERE g2_id IN( 1, 2 );
Query OK, 2 rows affected (0.30 sec)
Rows matched: 2  Changed: 2  Warnings: 0

mysql> SELECT * FROM gtb2;
+-------+-----------+-----------------+-----------+
| g2_id | g2_name   | g2_class        | g2_gender |
+-------+-----------+-----------------+-----------+
|     1 | 小明      | 五年级一班      | F         |
|     2 | 小刚      | 五年级2班       | F         |
|     3 | 小芳      | 五年级3班       | M         |
|     4 | 小红      | 五年级2班       | M         |
|     5 | 刘备      | 五年级1班       | M         |
|     6 | 张飞      | 五年级1班       | M         |
|     7 | 关羽      | 五年级2班       | M         |
|     8 | 曹操      | 五年级3班       | M         |
|     9 | 貂蝉      | 五年级3班       | M         |
|    10 | 洛神      | 五年级3班       | M         |
|    11 | 小红帽    | 五年级2班       | M         |
|    12 | 大乔      | 五年级1班       | M         |
+-------+-----------+-----------------+-----------+
12 rows in set (0.00 sec)
```

