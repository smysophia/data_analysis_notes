# 目录
- [目录](#目录)
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
    - [sorted() 排序](#sorted-排序)
    - [函数的说明文档](#函数的说明文档)
    - [可变参数 *agrs/**kwargs](#可变参数-agrskwargs)
  - [闭包和装饰器](#闭包和装饰器)
  - [面向对象](#面向对象)
    - [类和实例](#类和实例)
    - [访问限制](#访问限制)
    - [继承](#继承)
      - [多态](#多态)
    - [获取对象的属性](#获取对象的属性)
    - [魔法函数(Magic Methods)](#魔法函数magic-methods)
  - [异常处理和存储数据](#异常处理和存储数据)
    - [错误处理](#错误处理)
    - [常见的错误类型和继承关系](#常见的错误类型和继承关系)
    - [抛出错误](#抛出错误)
    - [调试](#调试)
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
    * 每一个元素必须是字符串. 例如:`y = ('1','2','3')  '.'.join(y)`  y=(1,2,3) 会报错
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
    * 注意和sorted()的区别(参见[sorted()函数](#sorted-排序)) 
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
### sorted() 排序
`sorted(iterable, cmp=None, key=None, reverse=False)`对所有可迭代的对象进行排序操作。
* cmp :比较的函数，这个具有两个参数，参数的值都是从可迭代对象中取出. 例如: 
  ```py
  L=[('b',2),('a',1),('c',3),('d',4)]
  sorted(L, cmp=lambda x,y:cmp(x[1],y[1]))
  ```
* key: 主要是用来进行比较的元素，只有一个参数，具体的函数的参数就是取自于可迭代对象中，指定可迭代对象中的一个元素来进行排序。例如: `sorted(L, key=lambda x:x[1])`

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

## 面向对象 
OOP(Object Oriented Programming)
### 类和实例
* __init__(self)
    __init__()这个函数会在对象初始化的时候调用, 第一个参数必须为self,表示创建实例的本身.  
    在创建实例的时候，把一些我们认为必须绑定的属性强制填写进去。  
    有了__init__方法，在创建实例的时候，就不能传入空的参数了，必须传入与__init__方法匹配的参数，但self不需要传   
  ```py
  # 类
  class Student(object):
      # 类的属性, 即实例变量(和对象关系在一起的变量叫实例变量)
      def __init__(self, name, score): 
          self.name = name
          self.score =  score
      # 类的方法 即实例方法
      def grade(self):
          if self.score >= 90:
              return 'A'
          elif self.score >= 60:
              return 'B'
          else:
              return 'C'
  # 实例
  student1 = Student('minyan',100)  # 创建对象,一个student的实例
  print(student1)  # <__main__.Student at 0x2544cb12eb8> 变量student1指向类Student的实例
  # 实例的属性
  student1.name
  student1.score
  # 调用实例方法
  student1.grade()
  ```
* __str__()
  当使用print输出对象的时候，默认打印对象的内存地址。  
  如果类定义了__str__方法，那么就会打印在这个方法中的return的数据  
  ```py
  class Student(object):
      def __init__(self, name, score): 
          self.__name = name
          self.__score =  score

      def __str__(self):
          return f'学生{self.__name}, 考了{self.__score}分'

  student1 = Student('minyan', 90)
  print(student1)   # 打印__str__()的返回值, 不设置的话就会打印对象student1的地址
  ```
### 访问限制
* 不设限制:外部代码可以自由地修改实例的属性. 例如:`student1.score = 60`
* 私有变量:实例的变量名如果以__开头，就变成了一个私有变量（private），只有内部可以访问，外部不能访问,确保了外部代码不能随意修改对象内部的属性值.  
  * `student1.__name`会报错'Student' object has no attribute '__name'
  * `student1.__name = 'aha'`  把外部的.__name属性重新定义了.但实际上这个__name变量和class内部的__name变量不是一个变量！
  * `student1._Student__score = 60` 强制访问类的私有属性(不推荐用)
* 为私有变量的访问和修改增加方法`get_变量名`和`set_变量名`. 好处:在方法中，可以对参数做检查，避免传入无效的参数
  ```py
  class Student(object):
      def __init__(self, name, score): 
          self.__name = name  # 变成私有变量
          self.__score =  score

      # 给Student类的私有属性增加get的方法, 允许外部访问属性
      def get_name(self):
          return self.__name
      def get_score(self):
          return self.__score

      # 给Student类的私有属性增加set方法.在方法中，可以对参数做检查，避免传入无效的参数：
      def set_score(self, score):
          if 0 <= score <= 100:
              self.__score = score
          else:
              raise ValueError('bad score')

  student1 = Student('minyan',100)
  student1.set_score(90)
  student1.get_score()
  student1.set_score(-1)  # traceback: ValueError: bad score
  ```

### 继承
子类 继承父类, 获得父类的所有属性和方法. 子类可以重写父类的属性和方法.当子类和父类具有同名属性和方法时,默认使用子类的同名属性和方法。
* `类名.__mro__` 查看类的继承顺序
```py
# 父类
class Animal(object):
    def __init__(self):
        self.food = 'food'
        self.__sound = 'Wow'  # 父类的私有属性, 不能被子类继承
    
    def eat(self):
        print(f'Animal is eating {self.food}')

    def run(self):
        print('Animal is running')

# 子类
class Dog(Animal):
    def __init__(self,food):
        self.food = food
    
    def eat(self,food):
        self.__init__(food) # 如果是先调用了父类的属性和方法，父类属性会覆盖子类属性，故在调用前，先调用自己子类的初始化。
        print(f'Dog is eating {self.food}')

    # 实现即调用父类又调用子类
    def animal_eat(self):
        Animal.__init__(self)  # 放入父类的属性和方法
        Animal.eat(self)

dog =  Dog('meat') # 实例化子类对象
dog.food   # 子类属性, 返回: 'meat'
# dog.__sound  # 报错, 子类不能继承父类的私有属性
dog.animal_eat()  # 调用父类同名方法, 返回: Animal is eating food
dog.food  # 调用完父类方法后,子类属性改变. 返回父类属性:'food'
dog.eat('meat')  # 调用子类同名方法, 同时重新传入参数,初始化子类属性, 返回: Dog is eating meat
dog.run() # 继承父类方法, 返回:Animal is running

Dog.__mro__  # 查看Dog类的继承顺序
```

#### 多态
多态：调用不同子类对象的相同父类方法。
步骤：
（1）定义父类，并提供公共方法
（2）定义子类，并重写父类方法
（3）传递子类对象给调用者，可以看到不同子类执行效果不同

当我们定义一个class的时候，我们实际上就定义了一种数据类型.。
* isinstance(变量,数据类型) 判断一个变量是否是某个类型.例如:`isinstance(dog, Animal)`返回True `isinstance(dog, Dog)`返回True
* 在继承关系中，如果一个实例的数据类型是某个子类，那它的数据类型也可以被看做是父类。实际上，任何依赖例如Animal这种数据类型,作为参数的函数或者方法都可以不加修改地正常运行，原因就在于多态。
  ```py
  class Animal(object):  
      def eat(self):  # 1.定义父类,父类公共方法
          print('Animal is eating food')

  class Dog(Animal):
      def eat(self): # 2.定义子类,并重写父类方法
          print('Dog is eating meat')    

  def eat_twice(Animal):  # 调用者调用了父类这个数据类型
      Animal.eat()
      Animal.eat()

  eat_twice(Animal())
  eat_twice(Dog())  # 3. 传递子类对象给调用者
  ```

* 动态语言:对于Python这样的动态语言来说，则不一定需要传入Animal类型。我们只需要保证传入的对象有一个run()方法就可以了, 例如:
  ```py
  class Hurry(object): # 是个基类,跟Animal类没关系
      def eat():   # 只要有eat这个方法就行
          print('hurry up!')

  eat_twice(Hurry)  # 输出'hurry up!','hurry up!'
  ```

### 获取对象的属性
* type(obj),返回对应的Class类型. 如果一个变量指向函数或者类，也可以用type()判断.例如`type(abs)`返回:builtin_function_or_method
  * `type(dog)` 返回:`__main__.Dog`
* types模块:判断一个对象是否是函数. 不能直接用`type(函数名) == function`会报错
  * type(fc) == types.FunctionType
  * type(abs)==types.BuiltinFunctionType
  * type(lambda x: x)==types.LambdaType
  *  type((x for x in range(10)))==types.GeneratorType
* isinstance(obj, type) 还可以判断一个变量是否是某些类型中的一种, 例如`isinstance([1, 2, 3], (list, tuple))`
* dir(obj) 获得一个对象的所有属性和方法.返回一个包含字符串的list
  * `dir(dog)` 返回列表:
    * ['_Animal__sound'(父类的私有属性), 
    * 'animal_eat'(父类同名方法),
    * 'eat'(子类重写的同名方法),
    * 'food'(子类重写的属性),
    * 'run'(从父类继承的方法)] 以及一些魔法函数
  * hasattr(obj,'attr') obj有属性(或方法)'attr'吗?返回布尔.例如:`hasattr(dog, 'food')`返回True
  * getattr(obj,'attr',default) 获取obj的'attr'属性(或方法). 例如:`getattr(dog, 'food')` 返回:'meat'.
    * 如果不设置默认值,当试图获取不存在的属性，会报错AttributeError
    * 当获取方法时, 返回这个实例方法的指针.
    * 可以将这个获得的指针复制给变量. 例如:
    ```py
    getattr(dog,'run')  # 返回:<bound method Animal.run of <__main__.Dog object at 0x00>>

    fn = getattr(dog,'run') # fn指向dog.run
    fn()  # 等价与dog.run(). 即调用fn,等同于调用dog.run
    ```
  * setattr(obj,'attr',value)  为obj设置一个属性. 例如:`setattr(dog,'age',10)` 之后`dog.age`就返回10了
  
### 魔法函数(Magic Methods)
允许在类中自定义函数,，并绑定到类的特殊方法中。
1. `__init__` : 在对象初始化的时候调用
2. `__str__` : print(obj) 等同于 `obj.__str__`
3. `__len__` : 如果调用len()函数试图获取一个对象的长度，实际上，在len()函数内部，它自动去调用该对象的__len__()方法.所以`len(obj)`和`obj.__len__()`是等价的
4. `__add__` 可以重新定义+和operator模块里的operator.add()
5. `__lt__` less than当比较两个实例时自动调用.会改变比较符号和sorted()
    ```py
    class String(object):
        def __init__(self, a):
            self.lenght = len(a)
            self.name = a
        def __add__(self,b): # 定义为长度相加
            return self.lenght + b.lenght 
        def __lt__(self,b): # 定义为长度比较
            return self.lenght < b.lenght  # 返回布尔值
        
    a = String('hellooooo')
    b =String('world')
    print(type(a)) # a,b的数据类型为String

    # __add__ 改变'+'的用法
    print('自定义+:',a+b) # 14
    print('字符串+:','hellooooo' + 'world') # helloooooworld
    print(a.__add__(b)) # 等同于a+b

    # __add__ 将影响operator模块功能的工作方式
    import operator
    print('自定义add: ', operator.add(a,b) )  # 14
    print('operator默认的add: ',operator.add('hellooooo','world')) # helloooooworld

    # __lt__ 改变比较符'>'或 '<'的用法和排序sorted()
    print('自定义的字符串比较: ',a > b)  # True
    print('默认字符串比较: ','hellooooo' >  'world') # False
    print('自定义排序: ',[item.name for item in sorted([a,b])])  # 直接sorted([a,b])返回的是对象
    print('默认的字符串排序: ',sorted(['hellooooo','world']))
    ```

-------------------
## 异常处理和存储数据
### 错误处理
`try...except...else....finally....`
```py
try:
    r = 10 / 0
    print('result:', r)
except Exception as error_info:   # 捕获错误
    print(error_info)
finally:  # 无论处不出错都会执行
    print('finally...')
print('END')
```
### 常见的错误类型和继承关系
* Exception
  * AssertionError 断言语句失败
  * StopIteration 迭代耗尽
  * ArithmeticError 算法错误
    * OverflowError  溢出,数值运算超出最大限制
    *  ZeroDivisionError 除0
  * AttributeError 尝试访问未知的对象属性
  * EOFError 用户输入文件末尾标志
  * ImportError 导入模块失败
  * LookupError 查找错误
    * IndexError 索引超出序列的范围
    * KeyError 字典中查找一个不存在的关键字
  * NameError 尝试访问一个不存在的变量
  * OSError 操作系统错误
    * FileNotFoundError 文件不存在
  * SyntaxError 语法错误
    * IndentationError 缩进错误
  * TypeError 不同类型间的无效操作
  * ValueError 传入无效的参数
    * UnicodeError  编码相关错误

### 抛出错误  

用`raise`语句抛出一个错误的实例
```py
def not_one(s):
    n = int(s)
    if n == 1:
        raise ValueError('value one is invalid!!!') # 抛出无效参数错误,并写上错误信息
    return n

def foo(s):
    try:
        not_one(s)
    except ValueError as err_info:
        print(err_info)  # 打印错误信息

foo(1) # 返回:value one is invalid!!!
foo('aaa') # 返回:invalid literal for int()
```
自定义错误类型.
首先根据需要，可以定义一个错误的class，选择好继承关系，然后，用raise语句抛出一个错误的实例
```py
# 定义一个错误类型,继承自Exception
class Password(Exception):
    def __init__(self, lenght, min_len):
        self.lenght = lenght
        self.min_len = min_len
    # 设置抛出异常的描述信息:
    def __str__(self):
        return f'你输入的密码长度为{self.lenght}, 不能少于{self.min_len}'
#使用异常
def password_error():
    try:
        password = input('请输入密码: ')
        lenght = len(password)
        if lenght < 5:
            raise Password(lenght, 5)  # 抛出错误的实例
    # 捕获异常
    except Exception as err_info:
        print(err_info)  # 打印异常的描述信息
    else:
        print('密码设置完成')
```
### 调试
* 断言 assert
凡是用print()来辅助查看的地方，都可以用断言（assert）来替代.assert的意思是，表达式应该是True.如果断言失败，assert语句本身就会抛出AssertionError.启动Python解释器时可以用-O参数来关闭assert
  ```py
  def foo(s):
      n = int(s)
      assert n != 0, 'n is zero!'  # assert 表达式, 出错信息
      return 10 / n

  foo(0)  # 抛出错误AssertionError: n is zero!
  foo(5) # 正确. 返回运算结果2.0
  ```
* 日志 logging
  和assert比，logging不会抛出错误，而且可以输出到文件.


------------


## DATABASE数据库操作
import sqlite3  
### 连接-获取游标-执行-获取数据-关闭
* 连接  
  conn = sqlite3.connect(db_name)  

  设置row_factory，对查询到的数据，通过字段名获取列数据  
  conn.row_factory = sqlite3.Row   
  Tips：需要设置在conn.cursor()语句之前，若是不设置该语句，只能通过索引 0,1,2...来获取内容。

* 获取游标  
cursor = conn.cursor()  
  
* 游标用于执行SQL语句  
CRUD操作  
cursor.execute("sql_str;")  
cursor.executemany(sql_str) 批量操作  
  
* 获取数据  
获取单条记录：fetchone()  
获取多条记录：fetchall()  
获取前n条数据：fetchmany(n)  
  
* 提交和关闭操作  
关闭游标: cursor.close()
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

* Create 创建数据
创建表 :
  * CREATE TABLE  table_name(column1,column2,column3);  
  `cur.execute('CREATE TABLE student(ID INT, NAME TEXT, AGE INT, SCORE FLOAT);')`
添加数据:
  * 添加一条数据:
    * INSERT INTO table_name(column1,column2,column3,...) VALUES(value1,value2,value3,...);  
    `cur.execute("INSERT INTO student VALUES (1,'Lucy',13,89);")`
  * 添加多条数据:
    * INSERT INTO table_name VALUES(?,?,?,?); 之后再写要放入的值
    ```py
    # 每一项必须是元祖,哪怕只有一个值
    students = (
    (4,'LiLei',11,93),
    (5,'Bobll',12,85),
    (6,'July',13,98)
    )
    ```
    `cur.executemany("INSERT INTO student VALUES(?,?,?,?);",students)`

* Read 查询数据
SELECT column_name,column_name FROM table_name ;
`cur.execute("SELECT * FROM student")`
  * INNER JOIN  内连接
  SELECT column1, column2 FROM table_1 INNER JOIN table_2 ON column_x = column_y  
  `cur.execute("SELECT emp_id, name, dept FROM employee INNER JOIN department ON employee.id = department.emp_id;").fetchall()`
  * LEFT OUTER JOIN  左外连接
  SELECT column1, column2 FROM table_1 LEFT OUTER JOIN table_2 ON column_x = column_y    
  `cur.execute("SELECT emp_id, name, dept FROM employee LEFT OUTER JOIN department ON employee.id = department.emp_id;").fetchall()`

* Update 更新数据
UPDATE table_name SET column1=value1,column2=value2,... WHERE some_column=some_value;    
`cur.execute("UPDATE student SET SCORE=99 WHERE ID=3;")`

* Delete 删除数据  
DELETE FROM table_name WHERE some_column=some_value;
`cur.execute("DELETE FROM student WHERE ID=5;")`

* 获取字段信息  
获取字段信息 PRAGMA [database.]index_info( index_name );  
index_info Pragma 返回关于数据库索引的信息。  
(id, name, type, notnull, default_value, primary_key)   
`cur.execute("PRAGMA table_info(student)").fetchall()`

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
