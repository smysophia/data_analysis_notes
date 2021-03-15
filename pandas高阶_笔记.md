# pandas 高阶
- [pandas 高阶](#pandas-高阶)
  - [层级索引](#层级索引)
    - [DataFrame索引的方法](#dataframe索引的方法)
  - [分组与聚合](#分组与聚合)
    - [分组操作](#分组操作)
      - [自定义分组](#自定义分组)
    - [聚合操作](#聚合操作)
  - [pivot table透视表](#pivot-table透视表)
  - [数据合并与重组](#数据合并与重组)
    - [DataFrame.append()](#dataframeappend)
    - [DataFrame.join()](#dataframejoin)
    - [pandas.concat()](#pandasconcat)
    - [DataFrame.merge()](#dataframemerge)
    - [数据重构stack、unstack](#数据重构stackunstack)
  - [其他设置](#其他设置)


## 层级索引 
*** 
例子数据
`data = pd.read_csv('2016_happiness.csv', usecols=['Country','Region', 'Happiness Rank','Happiness Score'])`
data.head()
```
       Country          Region  Happiness Rank  Happiness Score
0      Denmark  Western Europe               1            7.526
1  Switzerland  Western Europe               2            7.509
2      Iceland  Western Europe               3            7.501
3       Norway  Western Europe               4            7.498
4      Finland  Western Europe               5            7.413
```
*****
### DataFrame索引的方法
• 设置多个索引列 `DataFrame.set_index([‘a’,’b’], inplace=True) ` 
  * 例如:`data.set_index(['Region', 'Country'], inplace=True) ` 

• 外层索引切片 `DataFrame.loc[‘outer_index’]` 
  * 例如:`data.loc['Western Europe']`

• 内层索引切片  `DataFrame.loc[‘out_index’, ‘inner_index’]` 必须写两个参数.
  * 例如:`data.loc['Australia and New Zealand', 'New Zealand'] `

• 交换层级顺序 `DataFrame.swaplevel(i=- 2, j=- 1, axis=0)` 有返回值, 原数据不变
  * i, j: Levels of the indices to be swapped. Can pass level name as string.
  * axis: {0 or ‘index’, 1 or ‘columns’}, default 0

• 层级索引排序 `DataFrame.sort_index(level=0,inplace=False)`, 有返回值, 原数据不变  
  * 默认level=0,即按最外层排序, level可以设置为index的名字,或1(按内层排序)
  * 例如: `data.sort_index(level='Country')`
  
•按值排序, 和DataFrame的方法相同, `DataFrame.sort_values(by)`

•常用于分组操作、透视表的生成等  

<br>

## 分组与聚合
***
### 分组操作
`DataFrame.groupby(by=None, axis=0, level=None, as_index=True, sort=True, dropna=True)`
* 按单列分组，`DataFrame.groupby(‘label’)` 
  * 返回值类型:DataFrameGroupBy
  * 例如   `obj1 = data.groupby('Region')` 
   
* 按多列分组，`DataFrame.groupby([‘label1’, ‘label2’])`->多层dataframe  
  * 例如: `obj2 = data.groupby(['Region', 'Country'])`
  * type:DataFrameGroupBy   
  
* 对GroupBy对象进行分组聚合操作 mean()，sum()，size()，count()  
  `obj1.mean()`  
  `obj1.size()` 
   
#### 自定义分组 
• 方法1：`groupby(自定义函数)` 可以传入自定义的函数进行分组，操作针对的是索引 
  * 首先要设置分组的列为索引列. 例如:`data2 = data.set_index('Happiness Score')` 用Happiness Score来自定义分组
  * 可以用 groupby.size()看一下分组结果. 例如:`data2.groupby(get_score_group).size()` 其中get_score_group为自定义函数

• 方法2：项目中，通常可以先人为构造出一个分组列，然后再进行groupby
  例如:  `data['score group'] = data['Happiness Score'].apply(get_score_group)`

### 聚合操作
`DataFrame.agg(func=None)` Aggregate using one or more operations over the specified axis.
* func: function, str(funciton's name), list or dict
  * 函数: `data.groupby('Region').agg(np.max)`
  * 函数名字: 必须是内置的string function 例如: `data.groupby('Region').agg('max')`
  * 列表: 函数或函数名字的列表. 例如: `data.groupby('Region')['Happiness Score'].agg(['max', min, np.mean])`
  * 字典形式:axis labels -> functions 例如:`data.groupby('Region').agg({'Happiness Score': np.mean, 'Happiness Rank': np.max}`
  * 也可以是自定义函数. 例如:`data.groupby('Region').agg(lambda x:x.max() - x.min())`

<br>

## pivot table透视表 
*********

`df.pivot_table(values, index, columns, aggfunc, margins)`   
•values: 透视表中的元素值 column to aggregate（其结果根据聚合函数得出的）  
•index：透视表的行索引  
•columns：透视表的列索引 
•aggfunc：聚合函数，可以指定多个函数, 默认`aggfunc=np.mean`.可设置为列表. 例如:`aggfunc=[np.mean,np.sum]`  
•margins：表示是否对所有数据进行统计, 默认为 False

---------

例子数据
```
  Name	    Semester	Subject     Score
0	Alisa	Semester 1	Mathematics	62
1	Bobby	Semester 1	Mathematics	47
2	Cathr	Semester 1	Mathematics	55
3	Alisa	Semester 1	Science	    74
4	Bobby	Semester 1	Science	    31
5	Cathr	Semester 1	Science	    77
6	Alisa	Semester 2	Mathematics	85
7	Bobby	Semester 2	Mathematics	63
8	Cathr	Semester 2	Mathematics	42
9	Alisa	Semester 2	Science	    67
10	Bobby	Semester 2	Science	    89
11	Cathr	Semester 2	Science	    81
```
求每学期每门课的平均分
* 如果使用groupby方法
`df.groupby(['Semester', 'Subject']).mean()`
  ```
                              Score
  Semester   	Subject	
  Semester 1	Mathematics  	54.666667
              Science	        60.666667
  Semester 2	Mathematics	    63.333333
              Science	        79.000000
  ```
* 使用pivot方法
  `df.pivot_table(values='Score', index='Semester', columns='Subject', aggfunc=np.mean)`
  ```
  Subject	    Mathematics	    Science
  Semester		
  Semester 1	 54.666667	    60.666667
  Semester 2	 63.333333	    79.000000
  ```



## 数据合并与重组
***
### DataFrame.append()
按列↓方向,扩充行, 行增加
Append rows of other to the end of caller, returning a new object
`DataFrame.append(other, ignore_index=False, verify_integrity=False, sort=False)`
* other: The data to append.可以是:DataFrame or Series/dict-like object, or list of these
* ignore_index: If True, the resulting axis will be labeled 0, 1, …, n - 1.如果index没有意义,仅仅是序列,那ignore_index最好设置为True
* sort:Sort columns if the columns of self and other are not aligned.
* 例如: `pd.concat([pd.DataFrame([i], columns=['A']) for i in range(3)], ignore_index=True)`
  返回结果:
  ```
     A
  0  0
  1  1
  2  2
  ```

### DataFrame.join()
按行→方向, 扩充列,列增加
Join columns with other DataFrame either on index or on a key column. Efficiently join multiple DataFrame objects by index at once by passing a list.
`DataFrame.join(other, on=None, how='left', lsuffix='', rsuffix='', sort=False)`
* 如果不是按index连接而是指定key的话, other的key列必须先设置为index. 例如: `staff_df.join(student_df.set_index('姓名'),  on='姓名')`
* 默认左连接

### pandas.concat()
`pandas.concat(objs, axis=0, join='outer', ignore_index=False, keys=None, levels=None, names=None, verify_integrity=False, sort=False)`
* objs: 要合并的数据, 列表形式. 例如: `pd.concat([df1, df2])`
* axis: 0按索引方向（纵向,行变多了），1按列方向（横向, 列变多了). 缺失值为NaN, 可以用`df.fillna(num)` 补齐
* join: {'inner', 'outer'} 默认"outer",所有数据合并. 'inner'为有交集的才合并
* ignore_index:默认为False. 如果True,忽略原有index,重新编0, 1,…, n-1.如果index没有意义,仅仅是序列,那ignore_index最好设置为True
* keys: sequence, default None 传入最外层index参数.要在相接的时候在加上一个层次的key来识别数据源自于哪张表，可以增加key参数 例如:` keys=['table_x', 'table_y', 'table_z']`
* names:list. index的名字. Names for the levels in the resulting hierarchical index.
* sort: 将合并列排序
* 例子: `pd.concat([df1,df3],ignore_index=False, keys=['df1','df2'],names=['DF name', 'Row ID'], sort=False)`

### DataFrame.merge() 
Merge DataFrame or named Series objects with a database-style join.
`DataFrame.merge(right, how='inner', on=None, left_on=None, right_on=None, left_index=False, right_index=False, sort=False, suffixes=('_x', '_y'), copy=True, indicator=False, validate=None)`
注意与concat的区别, concat不会将相同的值进行合并处理,只单纯地按行或按列合并表格
* how: {'left', 'right', 'outer', 'inner', 'cross'}, default 'inner'
* on: 指定“外键”. label or list.必须在两个df中都有. Nond的话默认以df1 df2都有的列进行合并
* left_on/right_on: label or list 左/右表要合并的外键
* left_index/right_index:bool, default False. 以左右表的索引作为合并的外键.如果不是默认的索引,首先需要把这个外键设置为索引. 使用`DataFrame.set_index('col1', inplace=True)`
* suffixes:list-like, 默认(“_x”, “_y”).处理重复列名. 例如:`suffixes=('_staff','_student')`

### 数据重构stack、unstack
`DataFrame.stack(level=- 1, dropna=True)` 将数据的列旋转为行
Stack the prescribed level(s) from columns to index.
* level: 索引的层级。默认为-1，表示操作最里面的一层索引 level[0,1,2,...-2,-1]
* dropna: 默认为True 删除有缺失值的行.

实例:
```py
# 创建dataframe
header = pd.MultiIndex.from_product([['Semester1','Semester2'],['Maths','Science']])
d = [[12,45,67,56],[78,89,45,67],[45,67,89,90],[67,44,56,55]]
df = pd.DataFrame(d, index=['Alisa','Bobby','Cathri','Jack'], columns=header)
print(df)
stacked_df_1 = df.stack()
print(stacked_df_1)
stacked_df_0 = df.stack(0)
print(stacked_df_0)
```
结果:
```
df:
        Semester1	    Semester2
      Maths	Science	    Maths	Science
Alisa	12	 45	        67	    56
Bobby	78	 89	        45	    67
Cathri	45	 67	        89	    90
Jack	67	 44	        56	    55
------------------------------------------------
stacked_df_1:
             Semester1	Semester2
Alisa	Maths	12	      67
      Science	45	      56
Bobby	Maths	78	      45
      Science	89	      67
Cathri	Maths	45        89
        Science	67        90
Jack	Maths	67	      56
      Science	44	      55
------------------------------------------------
stacked_df_0:
                  Maths	Science
Alisa	Semester1	12	45
        Semester2	67	56
Bobby	Semester1	78	89
        Semester2	45	67
Cathri	Semester1	45	67
        Semester2	89	90
Jack	Semester1	67	44
        Semester2	56	55
```

`DataFrame.unstack(level=- 1, fill_value=None)`
将数据的index旋转为colum.
Pivot a level of the (necessarily hierarchical) index labels.
* 例如: `stacked_df_1.unstack()`将重新变位df的样子

## 其他设置
* 最多显示10列数据`pd.set_option('display.max_columns', 10)`

