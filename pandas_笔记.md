# Pandas
`import pandas as pd`  
## Series
```py
# from list
pd.Series(range(10,20))
# from ndarray
pd.Series(np.random.rand(5))
# from dictionary
d = {'a':0,'b':1,'c':3}  # key->index 

# set index
pd.Series(np.random.rand(3), index=['a','b','c'])
# get index
ser_obj.index
# give a name
ser_obj = pd.Series(np.random.rand(100), name='random_num')
# get name
ser_obj.name

# first 10
ser_obj = pd.Series(np.random.rand(100))
ser_obj.head(10)
# tail 10
ser_obj.tail(10)

# index->value
ser_obj2['b']
ser_obj2.loc['b']
ser_obj2.iloc[1]

# string
Series.str.lower()
Series.str.contains('A', case=False)  # case insensitive

```


## DataFrame  
Create DataFrame
```py
# from ndarry
arr =  np.random.rand(5,4)
df_obj = pd.DataFrame(arr)  # 自动生成column name and index
# ndarry ones
df2 = pd.DataFrame(np.ones((3,3)),columns=['a','b','c'])
# from list
df_obj = pd.DataFrame([[1,2,3],[2,1,1],[4,3,1],[5,1,2]],columns=['a','b','c'])
# from dictionary
country1 = pd.Series({'Name': '中国',
                    'Language': 'Chinese',
                    'Area': '9.597M km2',
                     'Happiness Rank': 79})

country2 = pd.Series({'Name': '美国',
                    'Language': 'English (US)',
                    'Area': '9.834M km2',
                     'Happiness Rank': 14})

country3 = pd.Series({'Name': '澳大利亚',
                    'Language': 'English (AU)',
                    'Area': '7.692M km2',
                     'Happiness Rank': 9})

df = pd.DataFrame([country1, country2, country3], index=['CH', 'US', 'AU'])
```
索引 / 增改数据 / 修改index
```py
# 按列索引
df_obj2.columns
df_obj2['e']  # type: series
df_obj2.e
# add column
df_obj2['f'] = range(4)
# delete column
df_obj3 = df_obj2.drop(columns=['e'],index=[2],)  # 返回值是操作的结果,原数据不变
del df_obj2['e']  # 在原数据上操作
# 或者设置参数inplace
df2 = df.drop('Name',axis=1, inplace=True)  # 没有返回值, df2为空

# reset index 不可直接修改index,可以重置index
ser_obj.reset_index()  # index 成为新的一列, type = DataFrame
ser_obj.reset_index(drop=True)   # 将原有的index去除, type仍旧是Series
```
切片

|Name	|Language	|Area	|Happiness Rank|
|---|---|---|---|
|CH	}中国	|Chinese	|9.597M |km2	79|
|US	|美国	|English (US)	|9.834M km2	|14}
|AU	|澳大利亚	|English (AU)	|7.692M km2	|9|
  
    
```py
# 列索引
df[['Area','Name']] 
# 行索引
df.loc['CH']   # 按标签,Series, dtype:object
df.iloc[2]       # 按位置
# 先行后列
df.loc['CH']['Area']
df.loc['CH',['Language','Area']]
# 先列后行
df['Area']['CH']
df['Area'].loc['CH']
# bool
filter = df['Language'].str.contains('English')
"""
CH    False
US     True
AU     True
Name: Language, dtype: bool
"""
df[filter]  # 返回结果,原数据不变
df[df['Happiness Rank'] <= 20]  # 返回结果
"""
    a	b	c	d
0	4	3	6	0
1	4	9	0	9
2	0	4	5	0
"""
df_obj['a']==4  # 'a'列==4 的布尔值
"""
0     True
1     True
2    False
Name: a, dtype: bool
"""
df_obj[(df_obj['a']==4)]
"""
    a	b	c	d
0	4	3	6	0
1	4	9	0	9
"""
df_obj.loc[(df_obj['a']==4),'b']  # a列上有4的行 的'b'列
"""
0    3
1    9
Name: b, dtype: int32
"""
```

## 数据补齐
* Series + Series  
按index相加,缺失数据为NaN  
或者使用fill_value参数
```py
ser1 = pd.Series([10,20,30,40,50,60])
ser2 = pd.Series([1,2,3])
ser1.add(ser2, fill_value = 100)
```
* DataFrame + Series  
Series被看作行数据,index会被当做列, index和column对应计算
```py
df1 = pd.DataFrame(np.ones((2,2)),columns=['a','b'])
"""
    a	b
0	1.0	1.0
1	1.0	1.0
"""
s1 = pd.Series(range(1,3), index=range(2))
"""
0    1
1    2
dtype: int64
"""
df1 + s1
"""
    a	b	0	1
0	NaN	NaN	NaN	NaN
1	NaN	NaN	NaN	NaN
"""
```
* DataFrame +DataFrame  
```py
df1.sub(df2, fill_value = 12)   #分别对df1 df2缺失的数据补全
df_fanal = df.fillna(100)   # 对结果进行补全.有返回值,原数据不变
```
## map()函数
* Python自带函数map()
```py
import math
l = list(range(10))
 # py自带函数map(formular, 应用的对象), 输出map的对象.直到去取结果,才会执行操作.
result = map(math.sqrt,l) 
list(result)
```
* Pandas中的map()  
作用于Series的每一个元素
```py
ser = pd.Series(l)
# Series.map(formular)
ser.map(np.sqrt)   # 映射函数
# 自定义函数lambda
ser.map(lambda x: x**2+1)
ser.map(lambda x:".".join([x,'com']))
# 自定义函数
def func(x):
    x1 = (x**3)*2 +1
    return x1
ser.map(func)
```
## apply() applymap() 函数  
可以运用于Series/DataFrame  
```py
df = pd.DataFrame(np.arange(10).reshape(5,2), columns=['col_1','col_2'])
df.apply(np.sum)  # 默认axis=0, 按列操作求和
"""
col_1    20
col_2    25
dtype: int64
"""
staff_df = pd.DataFrame([{'姓名': '张三', '部门': '研发部'},
                        {'姓名': '李四', '部门': '财务部'},
                        {'姓名': '赵六', '部门': '市场部'}])
staff_df['姓'] = staff_df['姓名'].apply(lambda x:x[0])
"""
    姓名	部门	姓
0	张三	研发部	张
1	李四	财务部	李
2	赵六	市场部	赵
"""
```
applymap()   对每一个值进行操作  'Series' object has no attribute 'applymap'  
例如:  
`
df.applymap(np.sqrt)
`
## 读写操作
* pd.read_csv(filepath, usecols, index_col)  
usecols:  指定需要读取的列,默认全部读取  
index_col 将某一列设置为索引列 
 
* df.info() 快速查看数据基本信息  
* df.to_csv(filepath, index) 保存数据  
filepath: 保存的路径  
index: 是否将索引列保存，默认为True

```py
data = pd.read_csv(filepath, usecols=["Country","Region", "Happiness Rank", "Happiness Score"], index_col="Country")

data["int_score"] = data["Happiness Score"].apply(np.around)# 使用apply()或者map(), 四舍五入生成整数

data.to_csv("2016.csv")  # 默认index(country列)为True, index会被写入
```

## 排序
* 索引排序  sort_index()
* 值排序 sort_values(by='label', ascending)
* 多列排序 by=[], ascending=[]

```py
# sort index 默认axis=0 按行排序
data.sort_index(ascending= False).head(10)  # 默认ascending=True 按升序排列

# sort values
data.sort_values(by='Country').head(10)

# 多列排序 by=[],ascending=[]
data.sort_values(by=['Region','Country'], ascending=[True, False]).head(10)  # Region升序,Country降序
```

## 数据清洗
### 1. 处理缺失数据
* 判断是否有缺失数据?  
** ser_obj.isnull()  df_obj.isnull()  
** any()  有一个True 返回值为True  
`log_data.isnull().any()  # 丢掉有空值的列`  
`log_data.isnull().any(axis=1)  # 丢掉有空值的行`  

* 丢弃缺失数据  
** dropna() 默认inplace=False 不在原数据上操作.默认subset=None  
` log_data.dropna(subset=["volume"]) # 丢掉volume上有缺失的行`   
* 填充数据  
** 前向填充front fill: ffill()   
** 后向填充behind fill:bfill(axis=0默认)  df_obj.fillna(method="bfill")   
    ```py
    # 先对数据进行排序,再填充
    sorted_log_data = log_data.sort_values(by=['time','user'])
    sorted_log_data.ffill()
    sorted_log_data.bfill()
    ```
### 2. 处理重复数据  
* 判断是否有重复   
    duplicated(subset=[]) 返回布尔型Series默认表示 *每行* 是否为重复行  
    设置subset指定列   
    `data.duplicated(subset=['age', 'surname'])`  
* 过滤重复行  
    drop_duplicates(subset, keep)  
    ** 默认判断全部列，可通过参数subset指定某些列  
    ** keep，默认（first）保留第一次出现的数据   
    `data.drop_duplicates(subset=['age', 'surname'], keep='last')  # 保留最后出现的数据` 

### 3. 替换数据
* replace(to_replace)  
    * 默认regex=False 不使用正则表达, inplace=False 不替换原数据  
    * 参数to_replace为需要被替换的值，可以是：  
    •数值、字符串  
    •列表  
    •字典   
` data.replace(0, 100) # 把数值0换成100`   
` data.replace([0, 1, 2, 3], 4)  # 把0 1 2 3 都换成4`  
` data.replace([0, 1, 2, 3], [4, 3, 2, 1])  # 列表替换为对应列表中的值`  
` data.replace({0: 10, 1: 100}) # 0->10,  1->100`

## 常用统计方法
* describe()，快速查看每列数据的统计信息：  
    `data.describe() `  
    •count，数据个数（非空数据）  
    •mean，均值  
    •std，标准差  
    •min，最小值  
    •25%，第1四分位数，即第25百分位数  
    •50%，第2四分位数，即第50百分位数  
    •75%，第3四分位数，即第75百分位数  
    •max，最大值  
    •quantile(q)，输出指定位置的百分位数，默认q=0.5  
    `data.quantile(q=0.5) # 取中位数` 

•max()，求最大值   
•min()，求最小值   
•idxmax()，返回最大值对应的索引,相当于numpy中的argmax()   
` data['Happiness Score'].idxmax()`  
•idxmin()，返回最小值对应的索引,相当于numpy中的argmin()       

• sum()，求和
• mean()，求均值
• median()，求中位数
• count()，求非空的个数  
不对缺失数据进行计算.

•mad()，求平均绝对误差（mean absolute deviation）  
•var()，求方差   
! 默认求sample的variance.若要求population(即按标准差公式), 则设置ddof=0 (delta degree of freedom)  
•std()，求标准差, 同上.   
•cumsum()，求累加  





