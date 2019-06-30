# Mysq操作指令 07

## 1.MySQL中GROUP BY 子句有关的常见错误P96
### 1.1 在SELECT子句中书写多余的列
`SELECT`子句中使用`COUNT`这样的聚合函数时，其中仅能存在以下3种元素：
-->常数                         #如1.2.3……
-->聚合函数                     #COUNT; AVG; SUM; MAX; MIN函数P81
-->GROUP BY 子句中指定的列名
错误示例：
```
mysql> SELECT name, pprice, COUNT(*) FROM pro
    -> GROUP BY pprice;
ERROR 1055 (42000): Expression #1 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'shop.pro.name' which is
not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by
```
### 1.2 GROUP BY中写列的“别名”
上述错误是因为MySQL 的命令执行顺序为 FROM-->WHERE-->GROUP BY-->SELECT，而SELECT才为列添加别名，可是却在GROUP BY 后面执行。

### 1.3 WHERE子句中使用“聚合函数”
```
mysql> SELECT type, COUNT(*) FROM pro
    -> WHERE COUNT(*) = 2
    -> GROUP BY type;
ERROR 1111 (HY000): Invalid use of group function
mysql> SELECT type, COUNT(*) FROM pro
    -> GROUP BY type;
+--------------+----------+
| type         | COUNT(*) |
+--------------+----------+
| 衣服         |        2 |
| 办公用品     |        2 |
| 厨房用具     |        4 |
+--------------+----------+
3 rows in set (0.00 sec)
```

