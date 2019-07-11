# 日期函数 
参考《SQL基础教程（第2版）》P190
## 1.当前日期CURRENT_DATE
**语法：`CURRENT_DATE` 示例：**
```
mysql> SELECT CURRENT_DATE;
+--------------+
| CURRENT_DATE |
+--------------+
| 2019-07-11   |
+--------------+
1 row in set (0.00 sec)
```

## 2.当前时间CURRENT_TIME
**语法：`CURRENT_TIME` 示例：**
```
mysql> SELECT CURRENT_TIME;
+--------------+
| CURRENT_TIME |
+--------------+
| 22:38:55     |
+--------------+
1 row in set (0.00 sec)
```
## 3.当前日期和时间CURRENT_TIMESTAMP函数
**语法：`CURRENT_TIMESTAMP` 示例：**
```
mysql> SELECT CURRENT_TIMESTAMP;
+---------------------+
| CURRENT_TIMESTAMP   |
+---------------------+
| 2019-07-11 22:43:20 |
+---------------------+
1 row in set (0.00 sec)
```

## 4.截取日期元素
**语法：`EXTRACT(日期元素, FROM 日期)` 示例：**  
截取出日期数据中的一部分，例如“年” “月” “小时” “秒”等，该函数的返回值并不是日期类型而是数值类型。
```
mysql> SELECT CURRENT_TIMESTAMP,
    -> EXTRACT(YEAR FROM CURRENT_TIMESTAMP) AS year,
    -> EXTRACT(MONTH FROM CURRENT_TIMESTAMP) AS month,
    -> EXTRACT(DAY FROM CURRENT_TIMESTAMP) AS day,
    -> EXTRACT(HOUR FROM CURRENT_TIMESTAMP) AS hour,
    -> EXTRACT(MINUTE FROM CURRENT_TIMESTAMP) AS min,
    -> EXTRACT(SECOND FROM CURRENT_TIMESTAMP) AS sec;
+---------------------+------+-------+------+------+------+------+
| CURRENT_TIMESTAMP   | year | month | day  | hour | min  | sec  |
+---------------------+------+-------+------+------+------+------+
| 2019-07-11 22:52:03 | 2019 |     7 |   11 |   22 |   52 |    3 |
+---------------------+------+-------+------+------+------+------+
1 row in set (0.00 sec)
```

# 转换函数
两层意思：1.数据类型的转换即**类型转换**，称为`CAST`；2.指的是值的转换。

## 1.数据类型的转换`CAST`函数
**语法：`CAST(转换前的值 AS 想要转换的数据类型)`**  
应用场景：插入与表中数据类型不匹配的数据，或者在进行运算时由于数据类型不一致发生了错误，又或者是进行自动类型转换会造成处理速度低下。
这些时候都需要事前进行数据类型转换。  
示例：
```
mysql> SELECT CAST('0001' AS SIGNED INT) AS int_col;
+---------+
| int_col |
+---------+
|       1 |           #将‘0001’字符转换为整数型数值
+---------+
1 row in set (0.00 sec)

mysql> SELECT CAST('2019-7-17' AS DATE) AS date_col;
+------------+
| date_col   | 
+------------+
| 2019-07-17 |       #将'2019-7-17'字符转换为整数型数值
+------------+
1 row in set (0.00 sec)
```
## 2.COALESCE函数
**01-语法`COALESCE((数据1，数据2，数据3……)`查询结果：返回参数中从左到右的第一个非NULL数据**  
`select coalesce(a,b,c); `如果a==null,则选择b；如果b==null,则选择c；如果a!=null,则选择a；如果a b c 都为null ，则返回为null（没意义）
示例：
```
mysql> SELECT
    -> COALESCE(NULL, 1) AS C1,
    -> COALESCE(NULL, 'test', NULL) AS C2,
    -> COALESCE(NULL, NULL, '2019-7-17') AS C3;
+------+------+-----------+
| C1   | C2   | C3        |
+------+------+-----------+
|    1 | test | 2019-7-17 |
+------+------+-----------+
1 row in set (0.00 sec)
```
**02-语法`COALESCE(数据列, NULL的替换数据)`查询结果：将该数据列内的NULL数据替换为指定数据**
示例：
```
mysql> SELECT s2 FROM SS;
+--------+
| s2     |
+--------+
| rt     |
| def    |
| 太郎   |
| NULL   |
| xyz    |
| NULL   |
| NULL   |
| NULL   |
| abc    |
| abc    |
| i      |
+--------+
11 rows in set (0.00 sec)

mysql> SELECT COALESCE(s2, 'change_s') FROM SS; 
+--------------------------+
| COALESCE(s2, 'change_s') |          
+--------------------------+
| rt                       |
| def                      |
| 太郎                     |
| change_s                 |       #用'change_s'替换“NULL”
| xyz                      |
| change_s                 |
| change_s                 |
| change_s                 |
| abc                      |
| abc                      |
| i                        |
+--------------------------+
11 rows in set (0.00 sec)
```
