## MySQL数据类型
参考资料[MySql数据类型](https://www.runoob.com/mysql/mysql-data-types.html)

特别摘录以下讯息：

### 数据类型的属性
![image](https://github.com/Leogaga19/Mysql-Study-Daily-Record/blob/master/Photos/%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B%E7%9A%84%E5%B1%9E%E6%80%A7.PNG)
 
### 数值类型

![数值类型](https://github.com/Leogaga19/Mysql-Study-Daily-Record/blob/master/Photos/%E6%95%B0%E5%80%BC%E7%B1%BB%E5%9E%8B.PNG)

*浮点型（float和double）

![image](https://github.com/Leogaga19/Mysql-Study-Daily-Record/blob/master/Photos/%E6%B5%AE%E7%82%B9%E5%9E%8B.PNG)


### 日期和时间类型
![image](https://github.com/Leogaga19/Mysql-Study-Daily-Record/blob/master/Photos/%E6%97%A5%E6%9C%9F%E5%92%8C%E6%97%B6%E9%97%B4%E7%B1%BB%E5%9E%8B.PNG)

如果定义一个字段为TIMESTAMP,这个字段的时间数据会随着其他字段修改时而自动刷新，故可以用此时间类型的字段来存放该条记录最后修改的时间。

### 字符串类型
![image](https://github.com/Leogaga19/Mysql-Study-Daily-Record/blob/master/Photos/%E5%AD%97%E7%AC%A6%E4%B8%B2%E7%B1%BB%E5%9E%8B.PNG)

char 和 varchar：

 1.char(n) 若存入字符数小于n，则以空格补于其后，查询之时再将空格去掉。所以 char 类型存储的字符串末尾不能有空格，varchar 不限于此。
 2.char(n) 固定长度，char(4) 不管是存入几个字符，都将占用 4 个字节，varchar 是存入的实际字符数 +1 个字节（n<=255）或2个字节(n>255)，所以 varchar(4),存入 3 个字符将占用 4 个字节。
 3.char 类型的字符串检索速度要比 varchar 类型的快。
varchar 和 text：

 1.varchar 可指定 n，text 不能指定，内部存储 varchar 是存入的实际字符数 +1 个字节（n<=255）或 2 个字节(n>255)，text 是实际字符数 +2 个字节。
 2.text 类型不能有默认值。
 3.varchar 可直接创建索引，text 创建索引要指定前多少个字符。varchar 查询速度快于 text, 在都创建索引的情况下，text 的索引似乎不起作用。
 


