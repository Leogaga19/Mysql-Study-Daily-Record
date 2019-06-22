#Mysql的密码设置
成功安置Mysql以后，以管理员身份运行cmd,进入到Mysql的bin路径，输入mysql -u root -p 启动本地Mysql，如果安装成功则会出现Enter password：代码如下：```
```
F:\mysql-8.0.16>cd bin

F:\mysql-8.0.16\bin>mysql -u root -p
Enter password:***********
```
上面输入的是初始密码，成功进入后出现：Welcome to the MySQL monitor.  …… Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.的欢迎词
然后光标变成mysql>
用use mysql;来进入。特别提示：在Mysql中命令写完后必须用;来结尾，否则无法运行。
如果是第一次进入，则会出现以下提示：
```
mysql> use mysql;
ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
```
这是Mysql提醒你用Alter user 语句来修改密码，具体做法如下:
