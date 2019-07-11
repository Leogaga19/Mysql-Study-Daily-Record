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

