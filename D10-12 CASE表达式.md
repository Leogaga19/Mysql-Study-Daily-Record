 # 搜索CASE表达式
   
   WHEN 子句中的“< 求值表达式 >”就是类似“列 = 值”这样，返回值为**真值（TRUE/FALSE/UNKNOWN）的表达式**。可将其看作使用 =、 !=逻辑符号。 		
	 **语法：**
 ```
 CASE
    WHEN <求值表达式> THEN <表达式>
    WHEN <求值表达式> THEN <表达式>
    WHEN <求值表达式> THEN <表达式>
    ……
    ELSE <表达式>
 END
```
 
