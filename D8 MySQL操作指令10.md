# MySQL之谓词
参考《MySQL基础教程（第2版）》P201章节内容
*谓词*是**函数**的一种，是需要满足特定条件的函数，该条件就是返回值是**真值**。对通常的函数来说，返回值有可能是**数字、字符串或者日期等**，但是
谓词的返回值全都是**真值（TRUE/FALSE/UNKNOWN）**。这也是谓词和函数的最大区别。

### 学习用表
```
mysql> CREATE TABLE SL
    -> (sc VARCHAR(6) NOT NULL,
    -> PRIMARY KEY (sc));
Query OK, 0 rows affected (0.94 sec)

mysql> INSERT INTO SL VALUES ('abcddd');
Query OK, 1 row affected (0.30 sec)

mysql> INSERT INTO SL VALUES ('dddabc');
Query OK, 1 row affected (0.14 sec)

mysql> INSERT INTO SL VALUES ('abdddc');
Query OK, 1 row affected (0.11 sec)

mysql> INSERT INTO SL VALUES ('abcdd');
Query OK, 1 row affected (0.15 sec)

mysql> INSERT INTO SL VALUES ('ddabc');
Query OK, 1 row affected (0.12 sec)

mysql> INSERT INTO SL VALUES ('abddc');
Query OK, 1 row affected (0.15 sec)

mysql> SELECT * FROM SL;
+--------+
| sc     |
+--------+
| abcdd  |
| abcddd |
| abddc  |
| abdddc |
| ddabc  |
| dddabc |
+--------+
6 rows in set (0.00 sec)
```

## LIKE-字符串的部分一致查询
LIKE 谓词是模糊查询，主要用于**字符串的部分一致查询**。  
**更多知识请参考“D3 MySQL操作指令04”文章**
示例：
```
mysql> SELECT * FROM SL
    -> WHERE sc LIKE 'ddd%';
+--------+
| sc     |
+--------+
| dddabc |
+--------+
1 row in set (0.20 sec)

mysql> SELECT * FROM SL
    -> WHERE sc LIKE '%ddd%';
+--------+
| sc     |
+--------+
| abcddd |
| abdddc |
| dddabc |
+--------+
3 rows in set (0.00 sec)

mysql> SELECT * FROM SL
    -> WHERE sc LIKE '%ddd';
+--------+
| sc     |
+--------+
| abcddd |
+--------+
1 row in set (0.00 sec)
```
