- [python 基础](#python-基础)
  - [输入输出](#输入输出)
    - [print()函数](#print函数)
    - [格式化输出](#格式化输出)
  - [文件操作](#文件操作)
    - [读取文件](#读取文件)
    - [file对象的方法](#file对象的方法)
    - [json](#json)
    - [CSV](#csv)
    - [EXCEL](#excel)
  - [字符串](#字符串)
    - [切片slice](#切片slice)
    - [查找 修改 判断](#查找-修改-判断)
  - [列表](#列表)
    - [查\增\删\改](#查增删改)
    - [复制(赋值,浅拷贝,深拷贝)](#复制赋值浅拷贝深拷贝)
    - [序列转换](#序列转换)
  - [字典](#字典)
    - [增/删/改/查](#增删改查)
  - [集合](#集合)
    - [增加 删除](#增加-删除)
  - [推导式](#推导式)
  - [生成器和迭代器](#生成器和迭代器)
  - [函数](#函数)
    - [lambda 匿名函数](#lambda-匿名函数)
    - [filter() 过滤](#filter-过滤)
    - [map() 映射](#map-映射)
    - [functools.reduce() 累积](#functoolsreduce-累积)
    - [函数的说明文档](#函数的说明文档)
    - [可变参数 *agrs/**kwargs](#可变参数-agrskwargs)
  - [闭包和装饰器](#闭包和装饰器)
  - [DATABASE数据库操作](#database数据库操作)
    - [连接-获取游标-执行-获取数据-关闭](#连接-获取游标-执行-获取数据-关闭)
    - [SQL语法:](#sql语法)
  - [时间处理](#时间处理)
    - [time 模块](#time-模块)
    - [datetime 模块](#datetime-模块)
  - [正则表达](#正则表达)
    - [匹配字符窜](#匹配字符窜)
    - [替换字符窜](#替换字符窜)
    - [查找字符窜](#查找字符窜)

<br>
<br>

# python 基础  
## 输入输出
### print()函数   
`print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)`
* sep:Specify how to separate the objects, if there is more than one.默认为空格
* flush:输出是否被缓存通常决定于 file，但如果 flush 关键字参数为 True，流会被强制刷新。  

### 格式化输出  
* %形式
  * 整形
    * %02d 十进制,占2个占位符,右对齐, 用0补齐
    * <左对齐, >右对齐(默认), ^中间对齐  
  * 浮点：
    * %f 保留小数点后面六位有效数字
    * %.3f，保留3位小数位  
  * 字符窜：
    * %10s 10个占位符右对齐
    * %-10s 10个占位符左对齐
    * %.2s——截取2位字符串, 例如:`print("hello,%10.3s!"%a)`  10个占位符右对齐，取a中的前3个字符。  
* format形式  
    * 可以指定顺序  
      * 例如:`print("{0},{1}".format(a,b))`
    * 格式调整  
      * 例如:`print("{0},{1:.2s}".format(a,b))` 对b只取前两个字符
      * <左对齐(默认), >右对齐, ^中间对齐. 例如: `print(f'{a:*^11s}')` 11个占位符居中对齐*补齐空位
    * 可在推导式中使用
      * 例如: `list = ["this is {}.".format(i) for i in range(5)]`
    * 简写为f""形式
      * 例如:`print(f"{a.capitalize()} and {b:-^10s} for {c:.2f}")` a首字母大写,b用10个占位符居中对齐空位用-填充,c浮点数取小数点后2位
<br>

---------------
## 文件操作
### 读取文件
* `open(filepath, mode, encoding='utf-8')` 返回一个file对象.记得关闭file.close()
  * b:二进制打开
  * +: 可读可写
  * r: 读
  * rb+:以二进制格式打开一个文件用于读写。一般用于非文本文件如图片等。
  * w:写,原内容被删除. 文件夹不存在这创建新文件
  * a:指针放最后,追加内容.文件夹不存在这创建新文件
* `with open(filepath, mode) as file:` 不用关闭了
### file对象的方法
* f.read(size) 全部读取,返回str, 包含\n
* f.readline() 每次读一行到\n为止,返回str,包含\n
* f.readlines() 一行一行读,返回list.每一行都有换行自带\n，最后一行没有换行不带\n
* f.seek(offset,whence)  移动文件读取指针到指定位置. 
  * offset偏移量 
  * whence起始位置. 0开头，1当前位置，2文件结尾
  
### json
读取json
* `with open(filename,'r',encoding='utf-8') as f: json_data = json.load(f)`  json读取后的数据类型是字典
  
写入json
```py
with open (filename,'w',encoding='utf-8') as f:
    json.dump(data,f,ensure_ascii=False)  # 如果包含中文的话,取消默认ascii编码
```
### CSV
1. 引入CSV模块
   * `csv.reader(csvfile, dialect='excel', delimiter=',')`
   * `csv.writer(csvfile, dialect='excel', **fmtparams)`

2. 引入pandas模块
   * `pd.read_csv((filepath, sep=',', header=0, names=None, index_col=None, usecols=None, dtype=None, skiprows=None, nrows=None,encoding=None) ` 读取csv
   * `df.to_csv(filepath, index=True, encoding=None)`  写入csv 设置index=False 不要把索引写入

### EXCEL
* `pandas.read_excel(filepath, sheet_name=0, header=0, names=None, index_col=None, usecols=None, dtype=None,skiprows=None, nrows=None)` 读取Excel
*  `DataFrame.to_excel(excel_writer, sheet_name='Sheet1', columns=None, header=True, index=True, index_label=None, startrow=0, startcol=0, encoding=None)` 写入Excel

------------------------------ 
## 字符串
不可变有序序列
### 切片slice
`str[start:end:step]`
* `s[::2]` 从头取到尾,步长为2
* `s[1::2]` 从1号位置取到尾,步长为2
* `s[::-1]`  从头取到尾,步长为-1, 倒着取
* `s[:-1]` 从头取到倒数第一个位置,不包含倒数第一个
### 查找 修改 判断
* 查找
  * str.find()/str.index()
    * `str.find(str, beg=0, end=len(string))` 返回查询子字符串开始的位置下标.查询失败返回-1
    * `str.index(str, beg=0, end=len(string))`  返回位置下标,查询失败报错
  * str.count()
    * `str.count(sub, start= 0,end=len(string))`  计字符串里某个子字符出现的次数
* 修改
字符串是不可变类型,修改字符串会改变id内存地址. 需要新变量接收
  * `str.replace(old, new, max)`   max:替换次数,默认全部替换
  * `str.split(str="", num=string.count(str))`  num:分隔次数, 默认是分隔符出现的次数
  * `str.join(sequence)`序列中的元素以指定的字符连接生成一个新的字符串. 
    * str为连接符, 一般为'.','-'
    * seq可以为字符串组成的列表/元祖. 例如字符串序列(s1,s2,s3)
  * `str.strip([chars])` 移除字符串头尾指定的字符（默认为空格或换行符）或字符序列. 同理rstrip() lstrip()
  * `str.lower()`/`str.upper()`/`str.capitalize()` 改变大小写
* 判断 
  * `str.startswith(substr, beg=0,end=len(string))` 同理endswith. 检查字符串是否是以指定子字符串开头
  * `sub_str in str` 返回布尔值

-------------
## 列表
可变有序序列
### 查\增\删\改
* 查找:
  * `list.index(x, start, end)` 返回下标位置,找不到报错
  * `list.count(obj)` 统计某个元素在列表中出现的次数
  * `len(list)` 列表长度
* 增加 
原列表会变化, 因为list属于可变数据类型, 修改列表不会改变id内存地址, 操作无返回值
  * `list.append(obj)` 末尾追加单个数据
  * `list.extend(seq)` 末尾一次性追加另一个序列中的多个值
  * `list.insert(index, obj)` 将指定对象插入列表的指定位置
* 删除
  * `del list` 删除整个列表
  * `del list[index]`  删除列表中的指定数据
  * `list.pop([index=-1])` 移除列表中的一个元素（默认最后一个元素），并且返回该元素的值
  * `list.remove(obj)` 移除列表中某个值的第一个匹配项
* 修改
  直接在原数据上修改,没有返回值
  * `list[index] = new_value` 
  * `list.reverse()` 反向列表中元素
  * `list.sort( key=None, reverse=False)` 排序，如果指定参数，则使用比较函数指定的比较函数
    * 例如: key=lambda x:x[0] 
### 复制(赋值,浅拷贝,深拷贝)
* `list2 = list1` 直接赋值. 其实就是对象的引用（别名）,修改list1也会自动修改list2
* `list2 = list1.copy()` 浅拷贝, 等同于`list[:]`, 拷贝父对象，不会拷贝对象的内部的子对象
* `list2 = copy.deepcopy(list1)` 深拷贝. 需要引入 copy 模块的 deepcopy 方法

### 序列转换
字符串,列表,元组,集合,字典统称为序列
* `list(seq)` 将序列转为列表
* `tuple(seq)`  将序列转为元组  
* `str(obj)` 将对象转换为字符串
* `set(seq)` 集合将自动去重

-------------
## 字典
可变无序序列, 如果迭代的话,默认迭代的是keys
d = {} 或者 d = dict() 创建空字典, 字典是可变数据类型  
`len(dict)` 计算字典元素个数，即键的总数。
### 增/删/改/查
* 增加 删除 修改
  * `dict[key] = value` 如果键存在则修改对应的值，如果键不存在新增这个键和值
  * `del dict[key]`  删除对应的键和值
  * `dict.fromkeys(seq, value)` 创建一个新字典，以序列 seq 中元素做字典的键，value 为字典所有键对应的初始值
  * 
* 查询
  * `dict.get(key, default=None)` 返回指定键的值,如果键不存在，返回默认值
  * `dict.values()` 字典中所有的值,返回一个迭代器，可以使用 list() 来转换为列表`list(d.values())`
  * `dict.keys()` 所有的键值,返回迭代器,，可以使用 list() 来转换为列表
  * `dict.items()` 返回迭代器, 以列表返回可遍历的(键, 值) 元组数组

------
## 集合
s = set() 或者{}但是创建空集合必需用set()，因为{}创建的是空字典
集合的特点: 自动去除重复数据. 集合是可变无序序列
### 增加 删除
* `set.add(elmt)` 给集合添加元素，如果添加的元素在集合中已存在，则不执行任何操作
* `set.update(seq)`  可以添加新的元素或集合到当前集合中，如果添加的元素在集合中已存在，不添加
* `set.remove(item)` 移除集合中的指定元素, item不存在则报错.`set. discard()`不报错
* `set.pop()` 随机移除一个元素

-----------------

## 推导式
1. 列表推导式
   * 使用[]生成列表, 例如 生成间隔5分钟的时间列表序列 `["%02d:%02d"%(h,m) for h in range(0, 24) for m in range(0, 60, 5)]`
   * 使用()生成生成器generator, 例如 `multiples = (i for i in range(30) if i%3 == 0)`
2. 字典推导式
   * { key_expr: value_expr for value in collection if condition }  
   *  例如:快速对调key和value
    ```py
    dic_old = {'a': 10, 'b': 34}
    dic_new = {v: k for k, v in dic_old.items()}
    ```
    * 将两个等长列表生成键-值 对应的字典
    ```py
    l1 = ['a','b','c']
    l2 = [1,2,3]
    d = {l1[i]:l2[i] for i in range(len(l1))}
    ```
<br>

## 生成器和迭代器
生成器(generator)都是迭代器(Iterator)对象.可以一边循环一边计算, generator保存的是算法, 比列表节省空间
* `next(g)`  函数获得generator的下一个返回值.直到计算到最后一个元素，没有更多的元素时，抛出StopIteration的错误。
* generator是可迭代对象 `for i in g: print(i)`
* list、dict、str虽然是Iterable(可迭代), 但不是Iterator(迭代器), 不能被next()调用


1. g = iter(Iterable)  把list dict str等Iterable变成Iterator
2. 推导式生成器: g = (列表推导式) 例如:  `g = (x * x for x in range(10))`
3. 函数生成器(generator function): 在每次调用next()的时候执行，遇到yield语句执行后返回，再次执行时从上次返回的yield语句处继续执行。
    ```py
    # 例1:生成无限序列
    def infinite_sequence():
        num = 0
        while True:
            yield num
            num += 1

    infinite_sequence()  # 不再是一个普通函数,而是一个generator object

    
    for i in infinite_sequence():
        print(i, end=" ") # 打印无限序列
        time.sleep(1)
    
    # 斐波那契数列(Fibonacci)
    def fib(max):
        n, a, b = 0, 0, 1
        while n < max:
            yield b
            a, b = b, a + b
            n = n + 1

    for i in fib(6):
        print(i)  
    ```

----------------

## 函数
### lambda 匿名函数
`lambda arguments : expression` 形参:表达式, 返回表达式的结果
* 带条件语句的lambda, 例如:`print((lambda a,b : a if a>b else b)(1,2))`
* 列表中的字典顺序排序, 例如:`list.sort( key=lambda x : x['名'], reverse = True)`
### filter() 过滤
`filter(function, iterable)`过滤序列，过滤掉不符合条件的元素，返回由符合条件元素组成的新列表的地址
* 当第1个参数是函数的时候, 序列的每个元素作为参数传递给函数进行判断,然后返回 True(1) 或 False(0)，最后将返回 True 的元素放到新列表中。
* 当第1个参数是None时，直接将第二个参数中为True(1)的值筛选出来
* 返回:filter地址, 需要list(filter())转化
### map() 映射
`map(function, iterable)` 第一个参数function以参数序列中的每一个元素调用函数，返回包含函数返回值的新列表地址。
* 例如:`list(map(lambda x: x ** 2, [1, 2, 3, 4, 5]))` 返回:`[1, 4, 9, 16, 25]`
### functools.reduce() 累积
`from functools import reduce` 引入包
`reduce(function, iterable)` 用传给函数function(有两个参数)先对集合中的第1 2个元素进行操作，得到的结果再与第三个数据用function函数运算，最后得到一个结果进行返回
* 例如:`reduce(lambda x,y:x+y, [1,2,3,4])` 等同于`sum([1,2,3,4])`
### 函数的说明文档
设置说明文档
```py
def my_func():
  """help document"""
  return True
```
调用说明:`help(my_func)`
### 可变参数 *agrs/**kwargs
1. 位置可变参数（接收所有的位置参数，返回一个元组）
```py
def func(*args):
        print(args)
func('a',1)  # 返回元组('a',1)
```
2. 关键字可变参数（接收所有关键字，返回一个字典）
```py
def func2(**kwargs):
        print(kwargs)
func2(a=1, b=2)  # 返回字典{'a': 1, 'b': 2}
```

----------------------

## 闭包和装饰器
* 闭包(closure)
  内部函数引用了外部函数的局部变量，那么内部函数就被认为是闭包.
  外部函数的返回值:内部函数名.
  ```py
  def my_sum(*args):  # 传入位置可变参数
      def sub_my_sum():
          s = 0
          for i in args:
              s += i
          return s
      return sub_my_sum  # 不能加括号，加括号表示调用. 返回的是函数名字而不是调用这个函数

  f = my_sum(1,3,5,7)
  f           # <function sub_my_sum()>
  f.__name__   # sub_my_sum
  print(f())   # 调用f函数, 即调用sub_my_sum, 打印结果1+3+5+7 16
  ```
* 装饰器（Decorator）
  在代码运行期间动态增加功能的方式，称之为“装饰器”.本质上，decorator就是一个返回函数的高阶函数。
  ```py
  # 生成调用函数的logging日志
  def log(func):
      @functools.wraps(func)  # 调用模块functools中的wraps函数, 把原始函数的__name__等属性复制到wrapper()函数中
      def wrapper(*args, **kwargs):
          print(f'正在调用函数:{func.__name__}' )   # 函数对象有一个__name__属性，可以拿到函数的名字
          t1 = datetime.datetime.now()
          func(*args, **kwargs)  # 这里要是不调用函数的话,需要再return func(*args, **kwargs)调用
          t2 = datetime.datetime.now()
          print(f'运行花费{(t2-t1).microseconds}毫秒.')
          # return func(*args, **kwargs)
      return wrapper

  @log  # 语法糖. 等同于 now = log(now)
  def now():
      t = datetime.datetime.now()
      time.sleep(0.1)
      print(f'{t.year}年{t.month}月{t.day}日{t.hour}时{t.minute}分{t.second}秒')

  now() 

  now.__name__  # 不加@functools.wraps 输出:wrapper. 加@functools.wraps,输出:now
  ```
  
-------------------
 
## DATABASE数据库操作
import sqlite3  
### 连接-获取游标-执行-获取数据-关闭
* 连接  
  conn = sqlite3.connect(db_name)  

  设置row_factory，对查询到的数据，通过字段名获取列数据  
  conn.row_factory = sqlite3.Row   
  Tips：需要设置在conn.cursor()语句之前，若是不设置该语句，只能通过索引 0,1,2...来获取内容。

* 获取游标  
conn.cursor()  
  
* 用于执行SQL语句  
CRUD操作  
cursor.execute("sql_str;")  
cursor.executemany(sql_str) 批量操作  
  
* 获取数据  
获取单条记录：fetchone()  
获取多条记录：fetchall()  
获取前n条数据：fetchmany(n)  
  
* 提交和关闭操作  
提交操作：conn.commit()  
关闭连接：conn.close()

### SQL语法:
* 查看数据库里有几张表
```py
cur = conn.cursor()
cur.execute("SELECT name FROM sqlite_master WHERE type='table';")
table_data = cur.fetchall()
print(table_data)
```

* Create  
    * CREATE TABLE  table_name(column1,column2,column3);  
    `cur.execute('CREATE TABLE student(ID INT, NAME TEXT, AGE INT, SCORE FLOAT);')`
    * INSERT INTO table_name(column1,column2,column3,...) VALUES(value1,value2,value3,...);  
    `cur.execute("INSERT INTO student VALUES (1,'Lucy',13,89);")`
    * INSERT INTO table_name VALUES(?,?,?,?);
    ```py
    students = (
    (4,'LiLei',11,93),
    (5,'Bobll',12,85),
    (6,'July',13,98)
    )
    ```
    `cur.executemany("INSERT INTO student VALUES(?,?,?,?);",students)`
* Read  
SELECT column_name,column_name FROM table_name ;
`cur.execute("SELECT * FROM student")`
* Update  
UPDATE table_name SET column1=value1,column2=value2,... WHERE some_column=some_value;    
`cur.execute("UPDATE student SET SCORE=99 WHERE ID=3;")`

* Delete  
DELETE FROM table_name WHERE some_column=some_value;
`cur.execute("DELETE FROM student WHERE ID=5;")`

* 获取字段信息  
获取字段信息 PRAGMA [database.]index_info( index_name );  
index_info Pragma 返回关于数据库索引的信息。  
(id, name, type, notnull, default_value, primary_key)   
`cur.execute("PRAGMA table_info(employee)").fetchall()`

* INNER JOIN  
SELECT column1, column2 FROM table_1 INNER JOIN table_2 ON column_x = column_y  
`cur.execute("SELECT emp_id, name, dept FROM employee INNER JOIN department ON employee.id = department.emp_id;").fetchall()`

* LEFT OUTER JOIN  
SELECT column1, column2 FROM table_1 LEFT OUTER JOIN table_2 ON column_x = column_y    
`cur.execute("SELECT emp_id, name, dept FROM employee LEFT OUTER JOIN department ON employee.id = department.emp_id;").fetchall()`

<br>

-----------

## 时间处理
### time 模块  
import time  
* time.ctime(sec) 把时间戳格式化为 例如"Mon Mar  1 21:34:43 2021"的字符串
* time.localtime(sec) 把时间戳格式化为本地时间, 返回时间元祖time.struct_time
    * 包含属性:tm_year tm_mon tm_mday tm_yday tm_wday tm_hour tm_min tm_sec
* time.strftime(format,t)  格式化日期
    * %a:星期简称  %A:星期完整名称 %w:星期(0-6)
    * %b: 月份简称 %B: 月份完整名称
    * %d: 月内的天  %j: 年内的天
    * %H %M %S: 时分秒
    * %Y: 四位数年份  %y: 两位数年份
* time.strptime(str, format)  把一个时间字符串解析为时间元组time.struct_time
* time.time()  返回当前时间戳
    
### datetime 模块
import datetime   
也可以直接从<datetime模块>引入<datetime类>,之后只要写一遍datetime:from datetime import datetime   

* t = datetime.datetime(2021,3,20)
    * type(t): <class 'datetime.datetime'>
    * 属性:year, month, day, hour, minute, second, microsecond, tzinfo
    * timestamp(): 获得时间戳浮点数
    * strftime(fmt): 将datetime格式转化成字符串
        * 例如: datetime.datetime.now().strftime('%y-%m-%d %H:%M')
* datetime.datetime.fromtimestamp(时间戳) 从时间戳转换为datetime格式
* datetime.datetime.strptime(str, fmt)  从字符串转换为datetime格式
    * 例如:datetime.datetime.strptime('2015-6-1 18:19:59', '%Y-%m-%d %H:%M:%S')

<br>

-----------
## 正则表达
### 匹配字符窜
`re.findall(pattern,str,mode)`  匹配str中的跟指定值有关的值,返回列表
* \[^\] 除了[]中的内容
* \d    :所有数字,相当于\[0-9\]
* \D    :排除所有数字,相当于\[^0-9\]
* \w    :匹配字母/数字/中文/下划线, 相当于\[a-zA-Z0-9_\]
* \W    :匹配特殊符号如 $ \t \n 空格
* \s    :匹配空白字符(包括空格、换行符、制表符等)，相当于 \[\t\n\r\f\v\]
* \S    :与\s相反，相当于 \[^\t\n\r\f\v\]

* \*   :匹配*之前的字符0次或无限次  *? 非贪婪
* \+   :匹配+之前的字符1次或无限次  +? 非贪婪
* ?    :匹配?之前的字符0次或1次  ?? 非贪婪
* .    :匹配任意单个字符,除了\n

* {n,m}  匹配最少n个,最多m个{}之前的字符  默认贪婪模式
* (xyz)  组, 匹配xyz字符窜, xyz是and关系,\[xyz\]中是or关系

* ^ :从字符窜开始位置匹配
* $ :从字符窜结尾位置匹配

匹配模式:
* re.I 忽略大小写 re.IGNORECASE
* re.S .变成任意匹配模式,包括换行符
* re.I | re.S  匹配模式之间用|隔开

### 替换字符窜
`re.sub(pattern, repl, string, count=0)`
* count 默认0 ,替换无限次
* repl 可以是函数

### 查找字符窜
`re.match(pattern, string)`   
* 从字符串的第一个字符**开始**匹配，如果未匹配到返回None，匹配到则返回一个<匹配对象>
* r.group() 返回找到的结果
* r.span() 返回一个元组表示匹配位置（开始，结束）   

`re.search(pattern, string)`   
* 是搜索整个字符串然后第一个匹配到指定的字符则返回一个匹配对象，未匹配到则返回None.需要通过group()来获取匹配到的值
