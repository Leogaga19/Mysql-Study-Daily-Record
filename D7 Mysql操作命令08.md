# 标量子查询
**标量子查询**“标量”就是“单一”的意思。其限制是：必须并且仅能返回1行1列的结果，也就是返回单一值的子查询。  
应用：**标量子查询**可以应用在任何使用**单一值**的地方，就是说**可以使用常数或列明的地方，无论是SELECT子句，GROUP BY子句，HAVING子句，还是ORDER BY子句
几乎所有的地方都可以使用。  
问题：查询按商品种类计算出的销售单价高于全部商品单价的商品种类。  
解答如下：
```
mysql> SELECT type, AVG(sprice)
    -> FROM pro
    -> GROUP BY type
    -> HAVING AVG(sprice) > (SELECT AVG(sprice) FROM pro);
+--------------+-------------+
| type         | AVG(sprice) |
+--------------+-------------+
| 衣服         |   2500.0000 |
| 厨房用具     |   2795.0000 |
+--------------+-------------+
2 rows in set (0.19 sec)
```

# 关联子查询
问题：参照pro表格，创建包含id,name,type,sprice和不同种类商品平均价格（average price of TPYE products）的视图。
**pro表格数据如下：**
```
mysql> SELECT * FROM pro;
+------+------------+--------------+--------+--------+------------+
| id   | name       | type         | sprice | pprice | rd         |
+------+------------+--------------+--------+--------+------------+
| 0001 | T恤衫      | 衣服         |   1000 |    500 | 2009-09-20 |
| 0002 | 打孔器     | 办公用品     |    500 |    320 | 2009-09-11 |
| 0003 | 运动T恤    | 衣服         |   4000 |   2800 | NULL       |
| 0004 | 菜刀       | 厨房用具     |   3000 |   2800 | 2009-09-20 |
| 0005 | 高压锅     | 厨房用具     |   6800 |   5000 | 2009-01-15 |
| 0006 | 叉子       | 厨房用具     |    500 |   NULL | 2009-09-20 |
| 0007 | 擦菜板     | 厨房用具     |    880 |    790 | 2008-04-28 |
| 0008 | 圆珠笔     | 办公用品     |    100 |   NULL | 2009-11-11 |
+------+------------+--------------+--------+--------+------------+
8 rows in set (0.00 sec)
```
则解答如下：
```
mysql> CREATE VIEW apbt
    -> AS
    -> SELECT id, name, type, sprice,
    -> (SELECT AVG(sprice) FROM  pro             #关联子查询
    -> AS P2                                         
    -> WHERE P1.type = P2.type                   #WHERE语句非常关键，限定了P1和P2表格中的商品种类一致，是的AVG函数按照商品种类进行均值计算
    -> GROUP BY type) AS asp                     #此处的GROUP BY type也可以省略，查询结果一致！
    -> FROM pro
    -> AS P1;
Query OK, 0 rows affected (0.19 sec)

mysql> SELECT * FROM apbt;
+------+------------+--------------+--------+-----------+
| id   | name       | type         | sprice | asp       |
+------+------------+--------------+--------+-----------+
| 0001 | T恤衫      | 衣服         |   1000 | 2500.0000 |
| 0002 | 打孔器     | 办公用品     |    500 |  300.0000 |
| 0003 | 运动T恤    | 衣服         |   4000 | 2500.0000 |
| 0004 | 菜刀       | 厨房用具     |   3000 | 2795.0000 |
| 0005 | 高压锅     | 厨房用具     |   6800 | 2795.0000 |
| 0006 | 叉子       | 厨房用具     |    500 | 2795.0000 |
| 0007 | 擦菜板     | 厨房用具     |    880 | 2795.0000 |
| 0008 | 圆珠笔     | 办公用品     |    100 |  300.0000 |
+------+------------+--------------+--------+-----------+
8 rows in set (0.00 sec)
```

# 字符串函数
MySQL中创建以下表，练习字符串函数
```
mysql> CREATE TABLE SS
    -> (s1 VARCHAR(40),
    ->  s2 VARCHAR(40),
    ->  s3 VARCHAR(40));
Query OK, 0 rows affected (1.01 sec)

mysql> INSERT INTO SS VALUES ('opx', 'rt', NULL);
Query OK, 1 row affected (0.35 sec)
mysql> INSERT INTO SS VALUES ('abc', 'def', NULL);
Query OK, 1 row affected (0.30 sec)

mysql> INSERT INTO SS VALUES ('山田', '太郎' ,'是我');
Query OK, 1 row affected (0.14 sec)

mysql> INSERT INTO SS VALUES ('aaa', NULL, NULL);
Query OK, 1 row affected (0.17 sec)

mysql> INSERT INTO SS VALUES (NULL, 'xyz', NULL);
Query OK, 1 row affected (0.14 sec)

mysql> INSERT INTO SS VALUES ('@!#$%', NULL, NULL);
Query OK, 1 row affected (0.38 sec)

mysql> INSERT INTO SS VALUES ('ABC', NULL, NULL);
Query OK, 1 row affected (0.12 sec)

mysql> INSERT INTO SS VALUES ('aBC', NULL, NULL);
Query OK, 1 row affected (0.14 sec)

mysql> INSERT INTO SS VALUES ('abc太郎', 'abc',  'ABC');
Query OK, 1 row affected (0.42 sec)

mysql> INSERT INTO SS VALUES ('abcdefabd', 'abc', 'ABC');
Query OK, 1 row affected (0.12 sec)

mysql> INSERT INTO SS VALUES ('micmic', 'i',  'I');
Query OK, 1 row affected (0.15 sec)

mysql> SELECT * FROM SS;
+-----------+--------+--------+
| s1        | s2     | s3     |
+-----------+--------+--------+
| opx       | rt     | NULL   |
| abc       | def    | NULL   |
| 山田      | 太郎   | 是我    |
| aaa       | NULL   | NULL   |
| NULL      | xyz    | NULL   |
| @!#$%     | NULL   | NULL   |
| ABC       | NULL   | NULL   |
| aBC       | NULL   | NULL   |
| abc太郎   | abc    | ABC    |
| abcdefabd | abc    | ABC    |
| micmic    | i      | I      |
+-----------+--------+--------+
11 rows in set (0.00 sec)
```
## 字符串拼接
**CONCAT 字符串拼接**  
**CONCAT_WS    指定字符串分割拼接字符串拼接**
### 1.语句：`CONCAT(str1,str2…)`    ###
解释：将str1和str2……字符串拼接起来
示例：
```
mysql>  SELECT  CONCAT('s1', 's2') AS s_cs;
+------+
| s_cs |
+------+
| s1s2 |
+------+
1 row in set (0.00 sec)

mysql>  SELECT s1, s2, CONCAT(s1, s2) AS s_cs FROM ss;
+-----------+--------+--------------+
| s1        | s2     | s_cs         |
+-----------+--------+--------------+
| opx       | rt     | opxrt        |
| abc       | def    | abcdef       |
| 山田      | 太郎   | 山田太郎     |
| aaa       | NULL   | NULL         |
| NULL      | xyz    | NULL         |
| @!#$%     | NULL   | NULL         |
| ABC       | NULL   | NULL         |
| aBC       | NULL   | NULL         |
| abc太郎   | abc    | abc太郎abc   |
| abcdefabd | abc    | abcdefabdabc |
| micmic    | i      | micmici      |
+-----------+--------+--------------+
11 rows in set (0.00 sec)
```

### 2.语句：`CONCAT_WS(separator,str1,str2…)` separator是指分隔符   ###
示例：
```
mysql> SELECT CONCAT_WS(" ","Hello","word");
+-------------------------------+
| CONCAT_WS(" ","Hello","word") |
+-------------------------------+
| Hello word                    |
+-------------------------------+
1 row in set (0.00 sec)

mysql> SELECT CONCAT_WS(", ","Hello","word");
+--------------------------------+
| CONCAT_WS(", ","Hello","word") |
+--------------------------------+
| Hello, word                    |
+--------------------------------+
1 row in set (0.00 sec)

mysql> SELECT CONCAT_WS(" ","Hello","word", "!");
+------------------------------------+
| CONCAT_WS(" ","Hello","word", "!") |
+------------------------------------+
| Hello word !                       |
+------------------------------------+
1 row in set (0.00 sec)
```

### 字符串长度
语句：`LENGTH(str)`
示例：
```
mysql> SELECT s1, LENGTH(s1) AS "s1长度" FROM SS;
+-----------+----------+
| s1        | s1长度   |
+-----------+----------+
| opx       |        3 |
| abc       |        3 |
| 山田      |        6 |               #1个汉字占3个字符
| aaa       |        3 |
| NULL      |     NULL | 
| @!#$%     |        5 |
| ABC       |        3 |
| aBC       |        3 |
| abc太郎   |        9 |
| abcdefabd |        9 |
| micmic    |        6 |
+-----------+----------+
11 rows in set (0.00 sec)
```

### 字符串大小写转换
**小写转换 `LOWER(str1)` 示例：**
```
mysql> SELECT s1, LOWER(s1) AS low_str FROM SS
    -> WHERE s1 IN ('ABC', 'aBC', 'abc', '山田');
+--------+---------+
| s1     | low_str |
+--------+---------+
| abc    | abc     |
| 山田   | 山田    |
| ABC    | abc     |
| aBC    | abc     |
+--------+---------+
4 rows in set (0.00 sec)
```
**大写转换 `UPPER(str1)` 示例：**
```
mysql> SELECT s1, UPPER(s1) AS up_str FROM SS
    -> WHERE s1 IN ('ABC', 'aBC', 'abc', '山田');
+--------+--------+
| s1     | up_str |
+--------+--------+
| abc    | ABC    |
| 山田   | 山田   |
| ABC    | ABC    |
| aBC    | ABC    |
+--------+--------+
4 rows in set (0.00 sec)
```
### 字符串的替换
**语句：`REPLACE(对象字符串，替换前的字符串，替换后的字符串)` 示例：**
```
mysql> SELECT s1, s2, s3,
    -> REPLACE(s1, s2, s3) AS "替换字符串"
    -> FROM SS;
+-----------+--------+--------+-----------------+
| s1        | s2     | s3     | 替换字符串       |
+-----------+--------+--------+-----------------+
| opx       | rt     | NULL   | NULL            |
| abc       | def    | NULL   | NULL            |
| 山田      | 太郎   | 是我    | 山田            |
| aaa       | NULL   | NULL   | NULL            |
| NULL      | xyz    | NULL   | NULL            |
| @!#$%     | NULL   | NULL   | NULL            |
| ABC       | NULL   | NULL   | NULL            |
| aBC       | NULL   | NULL   | NULL            |
| abc太郎   | abc    | ABC    | ABC太郎          |    #用ABC 替换 abc太郎 中的 abc
| abcdefabd | abc    | ABC    | ABCdefabd       |
| micmic    | i      | I      | mIcmIc          |
+-----------+--------+--------+-----------------+
11 rows in set (0.00 sec)
```
### 字符串的截取SUBSTRING
**语句：`SUBSTRING(对象字符串 FROM 截取的起始位置 FOR 截取的字符数)` 示例：**
```
mysql> SELECT s1,
    -> SUBSTRING(s1 FROM 3 FOR 2) AS "截取2个字符" FROM SS;
+-----------+------------------+
| s1        | 截取2个字符       |
+-----------+------------------+
| opx       | x                |
| abc       | c                |
| 山田      |                  |
| aaa       | a                |
| NULL      | NULL             |
| @!#$%     | #$               |
| ABC       | C                |
| aBC       | C                |
| abc太郎   | c太              |
| abcdefabd | cd               |
| micmic    | cm               |
+-----------+------------------+
11 rows in set (0.00 sec)
```
