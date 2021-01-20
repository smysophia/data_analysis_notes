# python 基础  
## 输入输出
print   
`
print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)
`  
flush -- 输出是否被缓存通常决定于 file，但如果 flush 关键字参数为 True，流会被强制刷新。  
```py
import time
print("loading",end="")
for i in range(10):
    print(".",end="",flush=True)
    time.sleep(0.5)
```
格式化输出  
* %形式  
    * 浮点：%.3f，保留3位小数位  
    * 字符窜：%.2s——截取2位字符串  
    * ```py
        a='world'
        print("hello,%10.3s!"%a)
        ```
        10个占位符右对齐，取a中的3个字符。  
        输出结果：hello, &emsp; wor!
* format形式  
    * 指定顺序
         ```py
        a='hello'
        b='world'
        print("{0},{1}".format(a,b))
        ```
    * 不指定顺序
        ```py
        a='world'
        b='hello'
        print("a,{},{}".format(b,a))
        ```
    * 格式调整  `print("{0},{1:.2s}".format(a,b))`
    * 也可以用作列表
    ```py
        list = ["this is {}.\n".format(i) for i in range(10)]
        for i in range(10):
            print(list[i],end="")
    ```
* f 形式
    * ```py
        a='hello'
        b='world'
        c=3.33333
        print(f"i want to say {a.capitalize()} and {b:-^10s} for {c:.2f}")
        # a字符窜built-in method capitalize of str首字母大写，b字符窜中间对齐占10个*占位符，c浮点数取小数点后2位
        ```
    输出结果：i want to say Hello and --world--- for 3.33
<br>
<br>

# 文件操作
## txt
```py
filename = 'notes.txt'
with open(filename,'r', encoding='utf-8') as f:
    print(f.read())
```
含有中文需要用编码utf-8  
<br>   

```py
filename = 'notes.txt'
with open(filename,'r', encoding='utf-8') as f:
    lines = f.readlines()
    print(lines)
    for line in lines:
        print(line, end="")
```
readlines 读为表格，遍历输出。  
  
## json
读取json
```py
import json
filename = 'beijing.json'
with open(filename,'r',encoding='utf-8') as f:
    json_data = json.load(f)
print(type(json_data))  # json读取后的数据类型是 字典
print(json_data.keys()) # 字典内置方法.keys() 返回迭代器
print(type(json_data['features']))  # 读取键'features'所对应的值,返回列表
print(len(json_data['features']))  # 列表长度
for item in json_data['features']: # 遍历列表
    print(item['type'])     # 列表中每一项都是字典,读取字典中的键'type'所对应的值
```
写入文件 
```py
with open (filename,'w',encoding='utf-8') as f:
    json.dump(data,f,ensure_ascii=False)  # 如果包含中文的话,取消默认ascii编码
```
## CSV
```py
import pandas as pd
filename = './lesson_02/examples/files/menu.csv'
df_obj = pd.read_csv(filename)
print(type(df_obj))  # frame.DataFrame
df_obj.head()        # 内置方法,预览前5行
filtered_data = df_obj[  ['Item', 'Calories']  ]   # 读取多列数据,列表 ['Item', 'Calories'],作为键值
filtered_data.to_csv('./lesson_02/examples/files/filtered data.csv', index=False)  # 写入,不要索引
```
## EXCEL
```py
import pandas as pd
filename = './lesson_02/examples/files/happiness.xlsx'
df_obj = pd.read_excel(filename, sheet_name=['2016','2017'])  # 读入多个工作簿
print(type(df_obj)) # 读入多个工作簿时,类型为字典
top5_2016 = df_obj['2016'].head()  # 默认前五
top5_2017 = df_obj['2017'].head()
# 写入单个工作簿
top5_2016.to_excel('./happiness2016.xlsx', index= False)
# 写入多个工作簿
writer = pd.ExcelWriter('./happiness2016_2017.xlsx')
top5_2016.to_excel(writer,'2016')
top5_2017.to_excel(writer,'2017')
writer.save()
```
  
## DATABASE
import sqlite3  
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
