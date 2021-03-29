# MYSQL
- [MYSQL](#mysql)
  - [基础查询语句](#基础查询语句)
    - [基本查询/去重/连接](#基本查询去重连接)
    - [条件查询](#条件查询)
    - [排序查询](#排序查询)
  - [使用函数处理数据](#使用函数处理数据)
    - [常用文本函数](#常用文本函数)
    - [数学函数](#数学函数)
    - [日期函数](#日期函数)
    - [流程控制函数](#流程控制函数)
    - [聚合函数](#聚合函数)
    - [分组聚合函数GROUP BY](#分组聚合函数group-by)
  - [总结:SELECT语法结构和运算顺序](#总结select语法结构和运算顺序)
  - [多表合并](#多表合并)
  - [连接查询](#连接查询)
    - [多表配合](#多表配合)
    - [内连接](#内连接)
    - [左连接](#左连接)
    - [全外连接](#全外连接)
    - [交叉连接](#交叉连接)
  - [子查询](#子查询)
    - [单行子查询](#单行子查询)
    - [多行子查询](#多行子查询)
  - [增/删/改](#增删改)
    - [创建 CREATE](#创建-create)
    - [增加 INSERT INTO](#增加-insert-into)
    - [修改 UPDATE...SET...](#修改-updateset)
    - [删除 DELETE](#删除-delete)
  - [SQL in Python](#sql-in-python)

## 基础查询语句
### 基本查询/去重/连接
* `select 查询列表 from 表名;` 查询列表可以是：表中的字段、常量值、表达式、函数
* `select 原名 as 别名 from 表名;` 重命名列表名
* `SELECT DISTINCT 字段 FROM 表名;` 查询去重的字段
* `SELECT CONCAT(字段1,字段2,…) AS 别名 FROM 表名;` 字段连接
  * 字段中的值有可能为空时: `SELECT CONCAT(字段1, IFNULL(字段2,为空时默认值),…) AS 别名 FROM 表名;` 不然有一个空值,返回的就是NULL
### 条件查询
`select 查询列表 from 表名 where 筛选条件;`
* `LIKE '通配条件';`
  * %: 代表任意多个字符,含0个
  * _: 任何单个字符
  * \: 转义符 例如: `LIKE '_\_%'; ` 第二个字符为下划线
  * [ ]:字符列中的任何单一字符
  * \[^\]:不在字符列中的任何单一字符
* `BETWEEN 值1 AND 值2;` 包含本身的两个值之间(含值1和值2). 否定:NOT BETWEEN
* `IN(条件1,条件2,...)` 指定条件范围. 例如: `SELECT * FROM '销售表' WHERE 店号 IN(1,3,7);`
* `IS NULL;` 判断是否为空值,不能用 列表名=NULL 来判断,或者可以用安全等于 `<=>`. 
  * 否定: `IS NOT NULL;`
  * 例如: `SELECT * FROM 销售表 WHERE 销售数量 IS NOT NULL;`
  * 处理NULL: `ISNULL(列名,0)` 将这列中含NULL的都处理为0(可以改) 或者 `IFNULL(列名,0)`
  
### 排序查询
`SELECT 字段名 FROM 表名 ORDER BY 字段名1 DESC 字段名2 DESC` 不写DESC就默认ASC(升序)
* `ORDER BY LENGTH(字段名)` 按字段名这一列中每一个的长度排序. 
  * 例如:`SELECT * FROM 商品表 ORDER BY LENGTH(商品名称); `
  * 在UTF8字符集中，一个英文字母点1个字节，一个汉字占3个字节。
* `INSTR（str, substr）` 查找一个字符串在另一个字符串中的位置,若不存在，则返回0。可以用作中文排序.
  * 例如: `SELECT * FROM '测试' ORDER BY INSTR('五月,四月,三月,二月,一月',月份)`  将月份这一列按5,4,3,2,1月排序
* ORDER BY 还支持按相对位置进行排序. 例如: `SELECT 商品名称,进价,售价 FROM '商品表' ORDER BY 2,3` 先按 进价 升序,再按 售价 倒序


## 使用函数处理数据
`SELECT 函数名(实参列表) FROM 表名` 如果函数名的括号里有字段名，就要加表名
### 常用文本函数
* `LEFT(字符串或字段，长度)`  字符串左边的几个字符
* `LENGTH(字符串或字段)`    返回字符串长度. 注意这里指的是字节, 其他函数值得都是字符. 汉字占3个字节.
* `CONCAT(字段1,字段2,…) ` 字段连接或字符串连接
* `LOWER(字段)` 转小写 `UPPER(字段)` 转大写
* `TRIM(' ' FROM 字段名或字符串)`  默认去掉空格. TRIM(字段名或字符串) 同理LTRIM() RTRIM()
* `SUBSTR(字符或字段,起始位置,截取几个字符)` 字符截取. 索引从1开始!
* `INSTR(字符串，子串)` 返回子串第一次出现的索引，如果找不到返回0
* `LPAD(字符串，字符位数，'占位符号') `  左填充 同理RPAD()
* `REPLACE(字符串，替换谁，换成什么)`  字符串替换
### 数学函数
* `round(数)` 四舍五入
* `ceil(数)`  向上取整 ceil() 向下取整
* `truncate(数,截取小数点后几位)` 例如: `truncate(7.30118,1)` 返回7.3
* `mod(被除数,除数)` 取余数
### 日期函数 
* `NOW()`  返回当前系统日期
* `CURDATE()`  返回当前系统日期，不包含时间
* `CURTIME()`  返回当前时间，不包含日期
*  获取指定的部分，年、月、日、小时、分钟、秒. 例如: YEAR(日期字段) Month(日期) Day(now()) DATE(日期)提取日期
*  `DATE_ADD()` 给日期添加指定的时间间隔
*  `DATE_SUB()` 从日期减去指定的时间间隔
*  `DATEDIFF()` 返回两个日期之间的天数
*  `str_to_date(str, 日期格式)`  将日期格式的字符转换成指定日期格式 例如:`str_to_date('3-30-2020','%m-%d-%Y')`
*  `DATE_FORMAT(日期, 日期格式)` 将日期格式转成字符 例如: `DATE_FORMAT('2020/3/30','%Y年%m月%d日')`
   * %Y 四位年份
   * %y 2位数年份
   * %m 月份
   * %c 月份
   * %d 日 
   * %H 24小时制
   * %h 12小时制
   * %i 分钟
   * %s 秒
* MySQL 使用下列数据类型在数据库中存储日期或日期/时间值：
  * DATE - 格式 YYYY-MM-DD
  * DATETIME - 格式: YYYY-MM-DD HH:MM:SS
  * TIMESTAMP - 格式: YYYY-MM-DD HH:MM:SS
  * YEAR - 格式 YYYY 或 YY
  
### 流程控制函数
* `if(表达式成立，返回值，否则返回值)`  例如:`SELECT * ,IF(销售数量>200,'优秀','一般') AS 评价 FROM 销售表`
* case 要判断的字段或表达式
  when 常量1/条件1 then 要显示的值1或语句1;
  when 常量2/条件2 then 要显示的值2或语句2;
  ….
  else 要显示的值n或语句n;
  end
例如:
```SQL
SELECT `商品名称`,`进价`,`售价`,  # 注意CASE前要有逗号
CASE `大类编码`
  WHEN 01 THEN 售价*1.1
  WHEN 02 THEN 售价*1.2
  WHEN 03 THEN 售价*1.05
  ELSE 售价
  END 
  AS 新售价
FROM 商品表;
```
### 聚合函数
聚合函数是对一组数据（一列或多列）进行处理，返回单个结果
* SUM（总和）、MAX（最大值），MIN（最小值），AVG（平均值）以及COUNT（计数不包含NULL值）
  * 例如: `SELECT SUM(销售数量) AS 求和,AVG(销售数量) AS 求平均,MAX(销售数量) AS 最大值,MIN(销售数量) AS 最小值,COUNT(销售数量) AS 计数 FROM 销售表;`
* Count(distinct 字段名) 计算字段非重复值的个数, 例如:`SELECT Count(DISTINCT 店号) FROM 销售表` 返回12, 有12家店
* Count(*) 计算总行数, 全空行 不计入. 例如:`SELECT Count(*) FROM 销售表` 返回1031, 即有1031条非全空的数据

### 分组聚合函数GROUP BY
语法:
SELECT  字段名，聚合函数(字段名) AS 定义的字段名
FROM 表名
WHERE 条件
GROUP BY 字段名或分组表达式, 字段名2
ORDER BY 字段名
LIMIT 10;   # 先排序,再取前几

例如:
```SQL
SELECT 店号, SUM(销售数量) AS 销售总量 FROM 销售表
WHERE 日期 BETWEEN '2020-01-05' AND '2020-01-06'
GROUP BY 店号
ORDER BY 销售总量;
```
HAVING与WHERE子句：
* HAVING在GROUP BY后使用，WHERE在GROUP BY前使用
* 作用对象不同:WHERE只作用于表中已有的字段，而HAVING作用于GROUP BY子句的分组结果
* 计算对象不同:WHERE计算指定字段的每条记录。HAVING是用于组的计算
例如:
```SQL
SELECT 店号,SUM(销售数量) AS 销售总量 FROM 销售表
WHERE 日期 BETWEEN '2020-01-05' AND '2020-01-06'
GROUP BY 店号
HAVING 销售总量 > 7000;
```

## 总结:SELECT语法结构和运算顺序
SELECT [DISTINCT] 字段名 FROM 表名
[WHERE] 条件筛选
[GROUP BY] 分组
[HAVING] 分组筛选
[ORDER BY] 排序
[LIMIT] 名次或分页

---------------------

## 多表合并
UNION ALL 不去重
UNION 去重

*  SELECT语句拥有相同的列数，而且字段的排放顺序相同
   *  SELECT * FROM 表名1 UNION ALL SELECT * FROM 表名2;
* 如果两张原始表的顺序不一致，你需要手动在SELECT后面写字段强行把他顺序变成一致。
  * 例如: `SELECT 姓名,语文,数学,英语  FROM 1班 UNION ALL SELECT 姓名,语文,NULL,英语 FROM 2班`

## 连接查询
### 多表配合
语法:
SELECT 字段名 FROM 表1,表2 WHERE 表1.字段名=表2.字段名
```SQL
SELECT a.店号,店名,商品编码,销售数量
FROM `销售表`a,`店铺表`b   # 相当于`销售表`AS a 取一个别名
WHERE b.`店号`=a.`店号`;
```
* 凡是表里没有这个字段的时候（你自造的字段）必需用HAVING，不能使用WHERE
* 凡是字段名在FROM表中是唯一的，字段名前可以省略表名

### 内连接
语法:
SELECT 字段名1, 字段名2
FROM 表1
INNER JOIN 表2
ON 表1.字段名 = 表2.字段名

例如:
```SQL
SELECT a.店号, 店名, SUM(销售数量) AS 销售总量
FROM `销售表` a
INNER JOIN `店铺表` b
ON a.店号 = b.店号
WHERE a.店号 in (1,3,7)
GROUP BY 店名
```
注意:如果有一个店号，在商品表里有，销售表里没有，或者在销售表里有，在商品表里没有，就V不过来，因为取的是同时符合条件的。

### 左连接
语法：
SELECT 字段名 FROM 表1 LEFT JOIN 表2 ON 连接条件 

例如:
查询毛利润总和前3的店铺, 显示店铺名称, 总毛利润
```SQL
SELECT 店名, SUM(销售数量*(售价-进价)) AS 毛利润
FROM `销售表` a
LEFT JOIN `店铺表` b ON a.店号 = b.店号
LEFT JOIN `商品表` c ON a.商品编码 = c.商品编码
GROUP BY 店名
ORDER BY 毛利润 DESC
LIMIT 3
```
### 全外连接
SQL Server 中支持全外连接 FULL JOIN +ON，但是暂时MySQL不支持，但是可以通过左连接 UNION 右连接的方式办到
 > SELECT * FROM 表a LEFT JOIN 表b ON 表a.师傅编号 = 表b.序号 
 UNION
 SELECT * FROM 表a RIGHT JOIN 表b ON 表a.师傅编号 = 表b.序号;

### 交叉连接
SELECT 字段名1,字段名2 FROM 表a CROSS JOIN 表b
CROSS JOIN 可以省略, 即
SELECT 字段名1,字段名2 FROM 表a, 表b
加上WHERE 就变成内连接了:
 SELECT 字段名1,字段名2 FROM 表a, 表b WHERE 表a.字段名=表b.字段名

-------------
 ## 子查询
 ### 单行子查询
 子查询结果是一个值,也就是说，子语句Select后面只能有一个字段名
 例如查询店铺平均销量数量大于3号店平均销量的店:
 ```SQL
SELECT 店号, AVG(销售数量) AS 平均销量 
FROM 销售表 
GROUP BY 店号
HAVING 平均销量 > (SELECT AVG(销售数量) AS 平均销量 FROM 销售表 WHERE 店号 IN(3))
```
### 多行子查询
* IN(子查询语句) 等于其中任意一个/ NOT IN() 不等于其中的任意
  * 例如: 查询'倚天'相关人员的考试成绩.子查询: 在'花名册'这个表中筛出'源'='倚天' 的 姓名. 因为返回值是多行,所以用IN. 主查询: 成绩表
    `SELECT * FROM 成绩表 WHERE 姓名 IN (SELECT 姓名 FROM 花名册 WHERE 源='倚天');`
* ANY(子查询语句) (大于小于)其中任意一个. 同SOME(). 可以用MAX(), MIN()代替
* ALL(子查询语句) (大于小于)其中所有的. 可以用MAX MIN代替
* SELECT后面的子查询
* FROM 后面子查询,放表几行几列都没关系,但是要给新的表别名.
* EXISTS 相关子查询.先执行主查询某个字段的值，根据子查询进行过滤.因为子查询涉及到了主查询的字段，所以叫相关子查询。EXISTS (子查询) 返回结果是：1或0.
  * 例如先筛选出 成绩表 里的所有列,然后按照 子查询筛选符合条件的: `SELECT * FROM 成绩表 WHERE EXISTS (SELECT 姓名 FROM 花名册 WHERE 成绩表.姓名=姓名 AND 源='倚天')`

## 增/删/改
### 创建 CREATE
* CREATE DATABASE 数据库名字;
* CREATE TABLE 表名称
(
列名称1 数据类型,
列名称2 数据类型,
列名称3 数据类型,
....
);
* 有以下数据类型:
  * 整形:
    * integer(size) 4 byte around 4 billion all positive
    * int(size) 4 byte
    * smallint(size)  2 byte
    * tinyint(size)
  * 浮点型: 
    * "size" 规定数字的最大位数。"d" 规定小数点右侧的最大位数。
    * decimal(size,d)
    * numeric(size,d)
    * FLOAT(size,d)
    * DOUBLE(size,d)
  * 字符串:
    * char(size) 容纳固定长度的字符串
    * varchar(size) 容纳可变长度的字符串,做大255
    * text 存放最大长度为 65,535 个字符的字符串。
  * 日期:
    * date(yyyymmdd) 格式：YYYY-MM-DD
    * datetime() 格式：YYYY-MM-DD HH:MM:SS
    * time() 格式：HH:MM:SS 
    * year() 如果2位格式所允许的值70到69，表示从1970到2069。
* 有以下约束(constraints)
  可以在创建表时规定约束（通过 CREATE TABLE 语句），或者在表创建之后也可以（通过 ALTER TABLE 语句）。
  * NOT NULL
  * UNIQUE: 注意MYSQL在申明完所有列的最后创建 `UNIQUE (列名)`, SQL等可以在申明列的时候直接加上UNIQUE
  * PRIMARY KEY: 约束唯一标识数据库表中的每条记录。默认NOT NULL和UNIQUE. MYSQL在最后申明.`PRIMARY KEY (列名)`
  * FOREIGN KEY: 外键.一个表中的 FOREIGN KEY 指向另一个表中的 PRIMARY KEY。MYSQL在最后申明 `FOREIGN KEY (列名) REFERENCES 指向的表名(列名)`
  * CHECK: 限制列中的值的范围。MYSQL在最后申明, 例如: `CHECK (Id_P>0)` ID必须大于0
  * DEFAULT:向列中插入默认值。直接在申明的时候协商即可. `DEFAULT '默认值'`
  * AUTO_INCREMENT: MySQL 使用 AUTO_INCREMENT 关键字来执行 auto-increment 任务。默认地，AUTO_INCREMENT 的开始值是 1，每条新记录递增 1。
  
* CREATE INDEX index_name ON table_name (column_name);在常常被搜索的列（以及表）上面创建索引。
  * ALTER TABLE 创建之后修改表
  * ALTER TABLE table_name
  ADD column_name datatype; 增加列
  * ALTER TABLE table_name 
  DROP COLUMN column_name; 删除列.某些数据库系统不允许这种在数据库表中删除列的方式 (DROP COLUMN column_name)。
  * ALTER TABLE table_name
  ALTER COLUMN column_name datatype; 改变数据类型
  * ALTER TABLE table_name
  ADD/DROP 约束条件;
  * ALTER TABLE Persons AUTO_INCREMENT=100; 要让 AUTO_INCREMENT 序列以其他的值起始

### 增加 INSERT INTO
* 方法1: 字段名和值一一对应 `INSERT INTO 表名(字段名1,字段名2…) VALUES(值1,值2….);`
* 方法2: 字段省略，但是值的位置必需和表中的字段位置一样，顺序不能颠倒 `INSERT INTO 表名 VALUES(值1,值2….);`
* 方法三: SET `INSERT INTO 表名 SET 字段名1=值1，字段名2=值2，`不支持插入多行,不支持子查询
* 插入多行: `INSERT INTO 表名 VALUES (值1,值2….),(值1,值2….),(值1,值2….)`
* 添加子查询: 例如: `INSERT INTO 销售表 SELECT 日期,店号,商品编码,销售数量 FORM 销售表2 WHERE 销售数量>300；`
* 备份表: `INSERT INTO 新表名 SELECT 列名 FROM 表名`

### 修改 UPDATE...SET...
* 修改单表数据
先锁定表，然后锁定字段，最后筛选
  ```SQL
  update 表名
  set 字段名1=新值1，字段名2=新值2，……
  where 筛选条件;
  ```
* 修改多表数据
  ```SQL
  update 表1 别名
  inner/left/right join 表2  别名
  ON 连接条件
  set 字段名1=值1，字段名2=值2，…..
  where 筛选条件;
  ```
  例如, 把孙悟空的师傅改为唐僧:
    ```SQL
    UPDATE 表a
    INNER JOIN 表b
    on 师傅编号=序号
    set 师傅='唐僧'
    WHERE 徒弟='孙悟空';
    ```
### 删除 DELETE
* 单表删除: `delete from 表名 where 筛选条件` 例如删除所有电话结尾为4的记录.`DELETE FROM 单表删除 WHERE 电话 LIKE '%4'`
* 多表删除:
  ```SQL
  delete 表1的别名，表2的别名       
  from 表1 别名
  inner/left/right join 别名 on 连接条件
  where 筛选条件;
  ```
  例如, 删去孙悟空(表a中的 徒弟)的师傅(表b 中的师傅)
  ```SQL
  DELETE b
  from 表b b
  INNER JOIN 表a a
  ON 序号=师傅编号
  WHERE 徒弟= '孙悟空';
  ```
* 删除整表: 
* `DROP TABLE 表名` 删除表（表的结构、属性以及索引也会被删除）
*  `truncate table 表名;  `  不能加where.仅仅需要除去表内的数据

## SQL in Python
import pymysql
* 连接: conn = pymysql.connect(host='localhost',user='root',password='o09',database='test')
* 获得游标: cursor = conn.cursor()
* 查询版本号:cursor.execute("SELECT VERSION()")
* 执行SQL语句  
  * CRUD操作  
  * cursor.execute("sql_str;")  
  * cursor.executemany("sql_str;", 元祖变量) 批量操作. 占位符为 `%s` 注意和sqlite不同. 每一个元祖对应一句sql语句  
  * try: ... except: ... 回滚
    ```py
    try: 
      cursor.execute("sql_str;")
      conn.commit()  
    except: 
      conn.rollback()  出错时回滚
    ```
* 获取数据  
  * 获取单条记录：cursor.fetchone()  
  * 获取多条记录：cursor.fetchall()  
  * 获取前n条数据：cursor.fetchmany(n)  
* 提交和关闭操作  
  * 关闭游标: cursor.close()
  * 提交操作：conn.commit()  
  * 关闭连接：conn.close()

实例:
```py
# MYSQL
import pymysql
# 连接
conn = pymysql.connect(host='localhost',user='root',password='o09',database='test')
cursor = conn.cursor()

# 查询版本号
cursor.execute("SELECT VERSION()")
result = cursor.fetchone()
print(result)

# 创建表
sql = """
    CREATE TABLE books(
        id int NOT NULL AUTO_INCREMENT,
        name varchar(50) NOT NULL,
        price decimal(10,2) DEFAULT NULL,
        public_time date DEFAULT NULL,
        PRIMARY KEY(id)
    )
"""
cursor.execute('DROP TABLE IF EXISTS books')  
cursor.execute(sql)

# 添加数据
sql = "INSERT INTO books VALUES(%s,%s,%s,%s)"
data = [('1','哆啦A梦','10.99','2021/03/05'),
        ('2','小王子','5','2019/01/01'),
        ('10','孙悟空','100','2000/01/03'),
        ('3','猪八戒','88','1999/01/03')]
try:
    cursor.executemany(sql, data)
    conn.commit() 
except:
    print("添加出错啦")
    conn.rollback()

# 删除数据
sql = "DELETE FROM books WHERE id=%s"
data = [(3),(10)]
try:
    cursor.executemany(sql, data)
    conn.commit() 
except:
    print("删除出错啦")
    conn.rollback()

# 修改数据
sql = "UPDATE books SET price=%s WHERE id=%s"
data = [(100,1),(6.99,2)]
try:
    cursor.executemany(sql, data)
    conn.commit() 
except:
    print("修改出错啦")
    conn.rollback()

# 查看数据
sql = "SELECT * FROM books WHERE id=1"
try:
    cursor.execute(sql)
    result =  cursor.fetchall()
    for i in result:
        print(i)
        ID = i[0]
        name = i[1]
        price = i[2]
        date = i[3]
        print(f'id={ID}, 书:{name}, 价格{price}元, 出版时间:{date}')
    conn.commit() 
except:
    print("查看出错啦")
    conn.rollback()

# 关闭
cursor.close() 
conn.close()
```
