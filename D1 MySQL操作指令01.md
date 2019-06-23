## MySQL操作指令
学习教程为[RUNOOB](https://www.runoob.com/mysql/mysql-connection.html)，操作系统Win10-64位，MySQL版本8.0.16 MySQL Community Server。

### 1.连接本地MySQL
以管理员身份打开cmd，进入到MySQL安置目录的bin目录。连接本地Mysql服务器实例：
```
F:\mysql-8.0.16\bin> mysql -u root -p
Enter password: **********
```
登录成功后出现以下内容：
```
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 8.0.16 MySQL Community Server - GPL

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```
然后光标前面出现mysql>_命令提示窗口，接下来即可执行任何SQL命令。

上述实例中，使用root 用户登录MySQL服务器，root用户拥有最高权限。同时，也可用其他的MySQL用户登录，前提是那个用户的权限足够。

### 2.连接远程主机上的MySQL
登录语法```mysql -h(主机IP) -u(用户名) -p(用户密码)```

设远程主机IP：192.168.10.1，用户名guest1，密码123abc。登录命令为：

```mysql -h192.168.10.1 -uguest1 -p123abc```

### 3.退出MySQL

退出mysql>_命令提示窗口可以使用exit或quit命令：
```
mysql> quit
Bye
```
或者
```
mysql> exit
Bye
```

### 4.MySQL创建数据库
MySQL服务启动后，使用```CREATE```命令创建数据库，语法：
```CREATE DATABASE name-of-database;```
例如创建名为abc1的数据库：
```
mysql> CREATE DATABASE abc1;
Query OK, 1 row affected (0.38 sec)
```
Query OK即提示创建成功！
当然为了保证新创建的数据库与现存数据库名称不冲突，则可以使用```CREATE DATABASE IF NOT EXISTS abc1 DEFAULT CHARSET=utf8;```创建数据库，
上述语法的意义：

1.如果abc1数据库不存在则创建该数据库，否则停止创建；
2.如果能创建则abc1数据库的编码集为utf8。

示例：
```
mysql> CREATE DATABASE IF NOT EXISTS abc1 DEFAULT CHARSET=utf8;
Query OK, 1 row affected, 2 warnings (0.24 sec)
```

### 5.MySQL删除数据库
使用普通用户登录MySQL服务器，可能需要特定权限来创建or删除MySQL数据库。但是如果用root用户登录，则拥有最高权限。

删除数据库时，务必特别特别谨慎！！！因为一旦执行删除命令，数据库将消失。

语法：```DROP DATABASE <数据库名>;```
示例：
```
mysql> DROP DATABASE abc1;
Query OK, 0 rows affected (0.36 sec)
```

### 6.MySQL选择数据库
成功启动MySQL服务后，可能有N个数据库，所以需要选择目标数据库，如果知道目标数据库名称，在mysql>_ 命令提示窗口使用```use <目标数据库名称>```命令来打开目标数据库，如下：
```
mysql> use abc;
Database changed
mysql>
```

当然，可以使用```SHOW DATABASES;```来显示所有数据库，如下：
```
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| abc                |
| abc2               |
| emp                |
| employees          |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
8 rows in set (0.00 sec)

mysql>
```

### 7.MySQL切换数据库
在mysql>_ 命令提示窗口使用```use <新目标数据库名称>```命令来切换目标数据库，如下：
```
mysql> use abc2;
Database changed
mysql>
```

