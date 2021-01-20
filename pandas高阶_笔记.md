# pandas 高阶
- [pandas 高阶](#pandas-高阶)
  - [层级索引](#层级索引)
  - [分组与聚合介绍](#分组与聚合介绍)
    - [分组操作(1)](#分组操作1)
  - [pivot table透视表](#pivot-table透视表)
  - [数据合并与重组](#数据合并与重组)


## 层级索引 
*** 
•设置多个索引列set_index([‘a’,’b’], inplace=True)    
`data.set_index(['Region', 'Country'], inplace=True)`  
•外层索引loc[‘outer_index’]  
`data.loc['Western Europe']`  
•内层索引
loc[‘out_index’, ‘inner_index’]   
`data.loc['Australia and New Zealand', 'New Zealand']`  必须写两个   
•交换层级顺序swaplevel()  
•层级索引排序sort_index(level=)  
`data.sort_index()` 这时候原位置才会变, 设置参数 默认level=0  
•常用于分组操作、透视表的生成等  

## 分组与聚合介绍
***
### 分组操作(1)
•按单列分组，obj.groupby(‘label’)  
`obj1 = data.groupby('Region')`  
•按多列分组，obj.groupby([‘label1’, ‘label2’])->多层dataframe  
`obj2 = data.groupby(['Region', 'Country']) `type:DataFrameGroupBy   
•对GroupBy对象进行分组聚合操作 mean()，sum()，size()，count()  
`obj1.mean()`  
`obj1.size()` 
   
自定义分组:  
•方法1：`groupby(自定义函数)` 可以传入自定义的函数进行分组，操作针对的是索引  
```py
# 自定义分组规则
def get_score_group(score):
    if score <= 4:
        score_group = 'low'
    elif score <= 6:
        score_group = 'middle'
    else:
        score_group = 'high'
    return score_group

data2 = data.set_index('Happiness Score')  # 首先设置为索引
data2.groupby(get_score_group).size()  # 传入自定义函数 .size() 看一下分组的结果
```

•方法2：项目中，通常可以先人为构造出一个分组列，然后再进行groupby  
```py
# 构造新的一列
data['score group'] = data['Happiness Score'].apply(get_score_group)
data.groupby('score group').size()
```
  
自定义聚合操作    
•使用agg()函数, 聚合操作  
```py
data.groupby('Region').max()
# agg(函数)
data.groupby('Region').agg(np.max)
# 传入包含多个函数的列表
data.groupby('Region')['Happiness Score'].agg([np.max, np.min, np.mean])
# 通过字典为每个列指定不同的操作方法
data.groupby('Region').agg({'Happiness Score': np.mean, 'Happiness Rank': np.max})
# 传入自定义函数
def max_min_diff(x):
    return x.max() - x.min()

data.groupby('Region')['Happiness Rank'].agg(max_min_diff)
```

## pivot table透视表  
***
broupby方法
`df.groupby(by=['Semester', 'Subject'])['Score'].mean().to_frame()`
pivot方法
`df.pivot_table(values, index, columns, aggfunc, margins)`  
例如:  
`df.pivot_table(values='Score', index='Semester', columns='Subject', aggfunc=np.sum)`  
•values: 透视表中的元素值（根据聚合函数得出的）  
•index：透视表的行索引  
•columns：透视表的列索引   
•aggfunc：聚合函数，可以指定多个函数, 默认`aggfunc=np.mean`或者`aggfunc='mean'`    
`df.pivot_table(values='Score', index='Semester',   columns='Subject', aggfunc=['mean', 'max', 'min'])`   
•margins：表示是否对所有数据进行统计, 默认为  False`margins=True`  

## 数据合并与重组
***
按照指定的轴方向对多个数据对象进行数据合并
* pd.concat(objs, axis), 
  `pd.concat([df1, df2], axis=1)`
  * objs: 多个数据对象，如包含DataFrame的列表
  * axis: 0按索引方向（纵向,行变多了），1按列方向（横向, 列变多了）
  * join="outer"默认, 可以改为"inner"
* merge()
  `pd.merge(staff_df, student_df, how='outer', on='姓名')`
  或者 `staff_df.merge(student_df, how='outer', on='姓名')`
  * on显示指定“外键”
    * left_on，左侧数据的“外键”
    * right_on，右侧数据的“外键”
  * how= 指定连接方式: outer inner left right
  * suffixes, 默认为('_x','_y'),处理重复列名 
    * 如果两个数据中包含有相同的列名（不是要合并的列）时，merge会自动加后缀作为区别,有相同列名的时候需要写出left_on right_on
    * `pd.merge(staff_df, student_df, how='left', left_on='姓名', right_on='姓名')`
  * left_index=True, right_index=True 用索引作为"外键", 首先需要`staff_df.set_index('姓名', inplace=True)`
  之后 `pd.merge(staff_df, student_df, how='left', left_index=True, right_index=True)`
* 数据重构stack、unstack
  `stacked_df = df.stack()`
  * stack():将数据的列旋转为行
  * 参数level: 索引的层级。默认为-1，表示操作最里面的一层索引 level[0,1,2,...-2,-1]
    `df.stack(level=0)`或者`df.stack(0)`
  * unstack(): 将数据的行旋转为列
