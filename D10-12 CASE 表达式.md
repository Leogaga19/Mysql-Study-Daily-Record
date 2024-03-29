# CASE表达式
CASE 表达式是在区分情况时使用的，这种情况的区分在编程中通常**称为（条件）分支。**  
参考[mysql中case使用](https://www.cnblogs.com/kacha886/p/9162696.html)  

## 搜索CASE表达式
WHEN 子句中的“< 求值表达式 >”就是类似“列 = 值”这样，返回值为真值（TRUE/FALSE/UNKNOWN）的表达式。  
**在`SELECT`语句中使用`CASE`时，前必须有‘，’逗号分隔，否则将报错！！**  
语法：
```
CASE
    WHEN <求值表达式_1> THEN <表达式>
    WHEN <求值表达式_2> THEN <表达式>
    WHEN <求值表达式_3> THEN <表达式>
    ……
    ELSE <表达式>
END
```
示例：
```
mysql> SELECT *,
    -> (CASE
    ->  WHEN age>12 THEN '青春期'
    ->  WHEN age<=12 THEN '幼儿组'
    ->  END) AS 'GROUP'                        #此处，AS 可以省略！ END 也可省略，默认为END 
    -> FROM shz;
+----+-----------+---------------+------+------+-----------+
| id | name      | class         | gen  | age  | GROUP     |
+----+-----------+---------------+------+------+-----------+
|  1 | 宋江      | 五年级3班     | M    |   14 | 青春期    |
|  2 | 吴用      | 五年级3班     | M    |   10 | 幼儿组    |
|  3 | 武松      | 五年级3班     | M    |   12 | 幼儿组    |
|  4 | 林冲      | 五年级3班     | M    |   11 | 幼儿组    |
|  5 | 卢俊义    | 五年级3班     | M    |   16 | 青春期    |
|  6 | 杜十娘    | 五年级3班     | F    |   16 | 青春期    |
|  7 | 潘金莲    | 五年级3班     | F    |   15 | 青春期    |
|  8 | 金瓶儿    | 六年级1班     | F    |   13 | 青春期    |
|  9 | 鲁智深    | 六年级1班     | M    |   14 | 青春期    |
| 10 | 石迁      | 六年级1班     | M    |   11 | 幼儿组    |
| 11 | 杨志      | 六年级1班     | M    |   12 | 幼儿组    |
| 12 | 柴进      | 六年级1班     | M    |   15 | 青春期    |
| 13 | 花荣      | 五年级3班     | M    |   11 | 幼儿组    |
| 14 | 李俊      | 六年级3班     | M    |   12 | 幼儿组    |
| 15 | 戴宗      | 六年级2班     | M    |   15 | 青春期    |
| 16 | 扈三娘    | 六年级2班     | F    |   16 | 青春期    |
+----+-----------+---------------+------+------+-----------+
16 rows in set (0.00 sec)

mysql> SELECT *,
    -> (CASE id
    ->          WHEN '000A' THEN 'A组'
    ->          WHEN '000B' THEN 'B组'
    ->          WHEN '000C' THEN 'C组'
    ->          WHEN '000D' THEN 'D组'
    ->  END) AS '分组'
    -> FROM sp;
+------+-----------+------+-----+--------+
| id   | name      | pid  | qy  | 分组   |
+------+-----------+------+-----+--------+
| 000A | 东京      | 0001 |  30 | A组    |
| 000A | 东京      | 0002 |  50 | A组    |
| 000A | 东京      | 0003 |  15 | A组    |
| 000B | 名古屋    | 0002 |  30 | B组    |
| 000B | 名古屋    | 0003 | 120 | B组    |
| 000B | 名古屋    | 0004 |  20 | B组    |
| 000B | 名古屋    | 0006 |  10 | B组    |
| 000B | 名古屋    | 0007 |  40 | B组    |
| 000C | 大阪      | 0003 |  20 | C组    |
| 000C | 大阪      | 0004 |  50 | C组    |
| 000C | 大阪      | 0006 |  90 | C组    |
| 000C | 大阪      | 0007 |  70 | C组    |
| 000D | 福冈      | 0001 | 100 | D组    |
+------+-----------+------+-----+--------+
13 rows in set (0.00 sec)

mysql> SELECT name, type,
    -> (CASE type
    ->          WHEN '衣服' THEN CONCAT('A:', type)
    ->          WHEN '办公用品' THEN CONCAT('B:', type)
    ->          WHEN '厨房用具' THEN CONCAT('C:', type)
    -> END) AS 'GROUP'
    -> FROM pro;
+------------+--------------+----------------+
| name       | type         | GROUP          |
+------------+--------------+----------------+
| T恤衫      | 衣服         | A:衣服         |
| 打孔器     | 办公用品     | B:办公用品     |
| 运动T恤    | 衣服         | A:衣服         |
| 菜刀       | 厨房用具     | C:厨房用具     |
| 高压锅     | 厨房用具     | C:厨房用具     |
| 叉子       | 厨房用具     | C:厨房用具     |
| 擦菜板     | 厨房用具     | C:厨房用具     |
| 圆珠笔     | 办公用品     | B:办公用品     |
+------------+--------------+----------------+
8 rows in set (0.00 sec)
```
**场景：对shop.pro表格中的三类上面销售单价进行汇总**  
两种实现方式：`CASE` 和 `GROUP BY`
```
mysql> SELECT SUM(CASE WHEN type='衣服'
    ->                  THEN sprice ELSE 0 END) AS ssp_clothes,
    ->        SUM(CASE WHEN type='办公用品'
    ->                  THEN sprice ELSE 0 END) AS ssp_office,
    ->        SUM(CASE WHEN type='厨房用具'
    ->                  THEN sprice ELSE 0 END) AS ssp_kitchen
    -> FROM pro;
+-------------+------------+-------------+
| ssp_clothes | ssp_office | ssp_kitchen |
+-------------+------------+-------------+
|        5000 |        600 |       11180 |
+-------------+------------+-------------+
1 row in set (0.00 sec)

mysql> SELECT type, SUM(sprice) AS sum_spric FROM pro
    -> GROUP BY type;
+--------------+-----------+
| type         | sum_spric |
+--------------+-----------+
| 衣服         |      5000 |
| 办公用品     |       600 |
| 厨房用具     |     11180 |
+--------------+-----------+
3 rows in set (0.00 sec)

mysql> SELECT
    -> SUM(CASE WHEN sprice <= 1000 THEN 1 ELSE 0 END) AS low,
    -> SUM(CASE WHEN sprice BETWEEN 1001 AND 3000 THEN 1 ELSE 0 END) AS mid,
    -> SUM(CASE WHEN sprice >= 3001 THEN 1 ELSE 0 END) AS high
    -> FROM pro;
+------+------+------+
| low  | mid  | high |
+------+------+------+
|    5 |    1 |    2 |
+------+------+------+
1 row in set (0.00 sec)

```
