# MySQL操作指令04

## 1.MySQL删除表记录DELETE
**命令：`DELETE FROM tb_name [WHERE 条件];`**  
1.当没有`[WHERE 条件]`时，将直接删除`tb_name`数据表；  
2.`[WHERE 条件]`用于删除指定记录，善用`WHERE`是必备技能；  
3.`[WHERE 条件]`的操作符很多如>，<，=,<>等。  
示例：
```
mysql> SELECT * FROM gtb2;
+-------+-----------+---------------+-----------+
| g2_id | g2_name   | g2_class      | g2_gender |
+-------+-----------+---------------+-----------+
|     1 | 孔明      | 六年级1班     | m         |
|     2 | 小刚      | 五年级2班     | M         |
|     3 | 小芳      | 五年级3班     | F         |
|     4 | 小红      | 五年级2班     | F         |
|     5 | 刘备      | 五年级1班     | M         |
|     6 | 张飞      | 五年级1班     | M         |
|     7 | 关羽      | 五年级2班     | M         |
|     8 | 曹操      | 五年级3班     | M         |
|     9 | 貂蝉啊    | 五年级3班     | F         |
|    10 | 洛神      | 五年级3班     | F         |
|    11 | 小红帽    | 五年级2班     | F         |
|    12 | 大乔      | 五年级1班     | F         |
+-------+-----------+---------------+-----------+
12 rows in set (0.00 sec)

mysql> DELETE FROM gtb2
    -> WHERE g2_class = '五年级3班' AND g2_gender='M';
Query OK, 1 row affected (0.40 sec)

mysql> SELECT * FROM gtb2;
+-------+-----------+---------------+-----------+
| g2_id | g2_name   | g2_class      | g2_gender |
+-------+-----------+---------------+-----------+
|     1 | 孔明      | 六年级1班     | m         |
|     2 | 小刚      | 五年级2班     | M         |
|     3 | 小芳      | 五年级3班     | F         |
|     4 | 小红      | 五年级2班     | F         |
|     5 | 刘备      | 五年级1班     | M         |
|     6 | 张飞      | 五年级1班     | M         |
|     7 | 关羽      | 五年级2班     | M         |
|     9 | 貂蝉啊    | 五年级3班     | F         |
|    10 | 洛神      | 五年级3班     | F         |
|    11 | 小红帽    | 五年级2班     | F         |
|    12 | 大乔      | 五年级1班     | F         |
+-------+-----------+---------------+-----------+
11 rows in set (0.00 sec)
```
## 2.MySQL的LIKE模糊查询
[LIKE的模糊查询参考文章](https://www.runoob.com/mysql/mysql-like-clause.html)  
SQL的`SELECT`命令可以在MySQL中查询数据，但是通常需要查询模糊数据，比如查询姓‘李’的用户、名字里面含有‘龙’的用户等。上面的场景仅用`WHERE`很难实现，通过`LIKE`即可轻松搞定。
**`SELECT`命令并不区分大小写，可以用`BINARY`来约束区分大小写**
**命令**
```
SELECT col1_name FROM tb_name
WHERE col2_name LIKE 'valueA' [AND(OR)] col3_name LIKE 'valueB';
```
**其实上面的命令中`LIKE`的作用就是`=`，`LIKE`模糊查询的命令如下：**  
![image](https://github.com/Leogaga19/Mysql-Study-Daily-Record/blob/master/Photos/LIKE%E6%A8%A1%E7%B3%8A%E6%9F%A5%E8%AF%A2%E6%96%B9%E6%B3%95.PNG)  
当然，可以使用`NOT LIKE`来实现反向查询。
`LIKE`模糊查询用法示例：  
```
mysql> SELECT * FROM gtb2
    -> WHERE g2_name LIKE '小_';
+-------+---------+---------------+-----------+
| g2_id | g2_name | g2_class      | g2_gender |
+-------+---------+---------------+-----------+
|     2 | 小刚    | 五年级2班     | M         |
|     3 | 小芳    | 五年级3班     | F         |
|     4 | 小红    | 五年级2班     | F         |
+-------+---------+---------------+-----------+
3 rows in set (0.00 sec)

mysql> SELECT * FROM gtb2
    -> WHERE g2_class LIKE '五年级%' AND g2_gender='F';
+-------+-----------+---------------+-----------+
| g2_id | g2_name   | g2_class      | g2_gender |
+-------+-----------+---------------+-----------+
|     3 | 小芳      | 五年级3班     | F         |
|     4 | 小红      | 五年级2班     | F         |
|     9 | 貂蝉啊    | 五年级3班     | F         |
|    10 | 洛神      | 五年级3班     | F         |
|    11 | 小红帽    | 五年级2班     | F         |
|    12 | 大乔      | 五年级1班     | F         |
+-------+-----------+---------------+-----------+
6 rows in set (0.00 sec)
```

## 3.UNION命令
MySQL UNION 操作符用于连接两个以上的`SELECT`语句的结果组合到一个结果集合内，默认会删除重复的数据。如果要显示全部结果使用`ALL`命令。
命令如下：
```
SELECT col1_name FROM tb1_name
UNION [ALL/DISTINCT]
SELECT col2_name FROM tb2_name
```
**示例：**
```
mysql> SELECT * FROM shz;
+----+-----------+---------------+------+
| id | name      | class         | gen  |
+----+-----------+---------------+------+
|  1 | 宋江      | 五年级3班     | M    |
|  2 | 吴用      | 五年级3班     | M    |
|  3 | 武松      | 五年级3班     | M    |
|  4 | 林冲      | 五年级3班     | M    |
|  5 | 卢俊义    | 五年级3班     | M    |
|  6 | 杜十娘    | 五年级3班     | F    |
|  7 | 潘金莲    | 五年级3班     | F    |
|  8 | 金瓶儿    | 六年级1班     | F    |
|  9 | 鲁智深    | 六年级1班     | M    |
| 10 | 石迁      | 六年级1班     | M    |
| 11 | 杨志      | 六年级1班     | M    |
| 12 | 柴进      | 六年级1班     | M    |
| 13 | 花荣      | 五年级3班     | M    |
| 14 | 李俊      | 六年级3班     | M    |
| 15 | 戴宗      | 六年级2班     | M    |
| 16 | 扈三娘    | 六年级2班     | F    |
+----+-----------+---------------+------+
16 rows in set (0.00 sec)
mysql> SELECT g2_gender FROM gtb2             #此处没有BINARY
    -> UNION
    -> SELECT gen FROM shz;
+-----------+
| g2_gender |
+-----------+
| m         |
| F         |
+-----------+
2 rows in set (0.20 sec)

mysql> SELECT BINARY g2_gender FROM gtb2      #此处有BINARY，即在查询是区分字母大小写
    -> UNION
    -> SELECT gen FROM shz;
+------------------+
| BINARY g2_gender |
+------------------+
| m                |
| M                |
| F                |
+------------------+
3 rows in set (0.00 sec)
mysql> SELECT g2_class FROM gtb2
    -> UNION                        #此处无ALL
    -> SELECT class FROM shz;
+---------------+
| g2_class      |
+---------------+
| 六年级1班     |
| 五年级2班     |
| 五年级3班     |
| 五年级1班     |
| 六年级3班     |
| 六年级2班     |
+---------------+
6 rows in set (0.00 sec)             #无ALL 显示6条记录

mysql> SELECT g2_class FROM gtb2
    -> UNION ALL                     #此处有ALL
    -> SELECT class FROM shz;
+---------------+
| g2_class      |
+---------------+
| 六年级1班     |
| 五年级2班     |
|……            |
| 六年级1班     |
| 五年级3班     |
| 六年级3班     |
| 六年级2班     |
| 六年级2班     |
+---------------+
27 rows in set (0.00 sec)            #有ALL 显示27条记录
```
