- [pandas 笔记](#pandas-笔记)
  - [Series](#series)
    - [创建Series](#创建series)
    - [Series属性和方法.](#series属性和方法)
      - [series.str  处理字符串](#seriesstr--处理字符串)
    - [切片](#切片)
    - [General Function](#general-function)
    - [数据补齐](#数据补齐)
  - [DataFrame](#dataframe)
    - [创建DataFrame](#创建dataframe)
    - [DataFrame属性和方法.](#dataframe属性和方法)
    - [切片](#切片-1)
    - [数据补齐](#数据补齐-1)
  - [Series/DataFrame的常用函数](#seriesdataframe的常用函数)
    - [map()函数 Series](#map函数-series)
    - [apply() Series/DataFrame](#apply-seriesdataframe)
    - [applymap() DataFrame](#applymap-dataframe)
    - [排序](#排序)
  - [读写操作](#读写操作)
    - [读取CSV/EXCEL](#读取csvexcel)
    - [写入CSV/EXCEL](#写入csvexcel)
  - [数据清洗](#数据清洗)
    - [1. 处理缺失数据](#1-处理缺失数据)
    - [2. 处理重复数据](#2-处理重复数据)
    - [3. 替换数值](#3-替换数值)
<br>
# pandas 笔记

## Series
### 创建Series
`pd.Series(data=None, index=None, dtype=None, name=None)`
* data类型:   
    * list
    * ndarray
    * dictionary  

### Series属性和方法.
返回数据类型 <pandas.core.series.Series> 
* 属性:
    * `series.index` 查看index, 返回类型<pandas.core.indexes.base.Index>
    * `series.values` 查看值,返回类型<numpy.ndarray>
    * `series.name`
    * `series.dtype` 查看类型
    * `series.shape`
    * `series.size`  查看有几个元素

* 方法
    * `series.astype()` 转化数据类型'float64' 'int64' 'int32' 'object'('str')
    * `series.mean()/max()/min()`
    * `series.head(num)`
    * `series.tail(num)`
    * `series.value_counts(sort=True, ascending=False)` 统计每个值出现的次数
    * `series.items()` 返回可迭代元祖 (index, value),用for遍历
    * `series.tolist()/to_numpy()`  将值变位列表/ndarray
#### series.str  处理字符串
  * `series.str.lower()/upper()/capitalize()`
  * `series.str.contains(pat, case=True)`  大小写敏感
  * `series.str.split(pat=None)`  默认空格, split后变成列表
  * `series.str.isalpha()/isnumeric()/isalnum()` 返回布尔值
  * `Series.str.cat(others=None, sep=None, na_rep=None, join='left')` 用sep连接series中的字符串
    
    
### 切片
* ser_obj[]
* ser_obj.loc[]  可以是函数,遮罩
* ser_obj.iloc\[i\]  查第i个位置所对应的值,可以是整数,整数列表,布尔列表,匿名函数

### General Function
* pd.to_numeric(Series, downcast=None) 将Series数据类型转换为数值型
    * downcast:{‘integer’, ‘signed’, ‘unsigned’, ‘float’}

### 数据补齐
Series + Series 按index相加,缺失数据为NaN,或者使用fill_value参数
* `ser1.add(ser2, fill_value = None)`

------
## DataFrame
### 创建DataFrame
`pd.DataFrame(data=None, index=None, columns=None, dtype=None, copy=False)`
* data类型:   
    * list  例如:\[\[1,2,3\],\[2,1,1\],\[4,3,1\],\[5,1,2\]\]
    * ndarray 例如:np.random.rand(5,4) 和 np.ones((3,3))
    * dictionary
    * pd.Series
### DataFrame属性和方法.
返回数据类型 <pandas.core.frame.DataFrame> 
* 属性:
    * `DataFrame.index` 查看index的名字, 返回类型<pandas.core.indexes.base.Index>
    * `DataFrame.columns`  查看columns的名字, 返回类型<pandas.core.indexes.base.Index>
    * `DataFrame.values` 查看值,返回类型<numpy.ndarray>
    * `DataFrames.dtypes` 查看每一列的类型, 注意series.dtype的区别
    * `DataFrame.shape`
    * `DataFrame.size`  查看有几个元素
    

* 方法
    * `DataFrame.info()` 快速查看数据基本信息
    * `DataFrame.items()` 返回可迭代元祖(column name, Series),用for遍历
    * `DataFrame.astype(dtype)` 转化某列的数据类型
        * dtype可为column name的字典, 例如:`df.astype({'Happiness Rank':'int32'})`
        * 转化数据类型'float64' 'int64' 'object'等
    * `DataFrame.drop(labels=None, axis=0, index=None, columns=None, level=None, inplace=False)` 删除行或列
        * axis 默认为0:index, 删除行
        * 可以用两种方法设置: 
            * (index=index's name, columns=column's name) 推荐
            * (label, axis={'index', 'columns'})
        * inplace 默认为False 将操作结果返回为一个copy, 原数据不变
    * `DataFrame.reset_index(level=None, drop=False, inplace=False)` 重置index, 原索引变成普通列, drop=True 原索引被删除
    * `DataFrame.set_index(keys, drop=True, append=False, inplace=False)` 设置某列为index,注意不可直接修改index,可以先重置index
    * `DataFrame.rename(mapper=None, index=None, columns=None, axis=None, inplace=False, level=None)` 重命名column或label
        * 可以用两种方法设置: 
            * (index=index_mapper, columns=columns_mapper, ...) 推荐
            * (mapper, axis={'index', 'columns'}, ...)
        * 也可以对index和column的名字进行重命名
            * df.index.rename('new name', inplace=False) 最好设置inplace=True
            * df.columns.rename('new name', inplace=False)
    * `DataFrame.describe()` 快速查看每列数据的统计信息 count mean std max quantile
    * `DataFrame.mean(axis=None, skipna=True, level=None)` 类似还有sum(), max(), idxmax(),median()
    * `DataFrame.var(axis=None, skipna=None, level=None, ddof=1)` 默认求sample, 设置ddof=0求population
    * DataFrame.count(axis=0, level=None) 统计非空值的个数
### 切片
默认为列索引
* `df[[col1, col2, col3]]`  

行索引  
* `df.loc[[index1, index2, index3]]`
* `df.iloc[[0,1,2]]`

先行后列/先列后行  
* `df.loc[[index1, index2]][col3]` 
* 'df.loc[index1, col1] `
* `df[[col1, col2]].loc[index1]`

bool切片
* `df[Series]`   dtype: bool

### 数据补齐
DataFrame + Series, Series被看作行数据,index会被当做列, index和column对应计算
* `df.add(ser, axis='columns', fill_value=None)` axis可以设置为'index'
DataFrame +DataFrame
* `df1.add(df2,axis='columns', level=None, fill_value=None)` 分别对df1 df2缺失的数据补全
* `df1.add(df2).fillna(value=None, method=None, axis=None, inplace=False)` 对结果进行补全
其他方法:add(), sub(), mul(), div(), mod(), pow()

------------------
## Series/DataFrame的常用函数
### map()函数 Series
* `map(function, iterable)`, python自带,iterable为一个或多个序列,返回结果
* Series.map(arg,na_action=None) arg可以为dic(series中没有对应key的会被转成NaN), series和函数, na_action可以设置为'ignore'
### apply() Series/DataFrame
`df.apply(func, axis=0 result_type=None)` 
* axis={0 or 'index', 1 or 'columns}, 默认0: 按index的方向,即按列操作(apply function to each column)
### applymap() DataFrame
`DataFrame.applymap(func, na_action=None)` 对df中每一个值进行操作
### 排序
* `DataFrame.sort_index(axis=0, level=None, ascending=True, inplace=False, na_position='last', ignore_index=False,by=None)` 按索引排序
* `DataFrame.sort_values(by, axis=0, ascending=True, inplace=False, na_position='last', ignore_index=False)` 按值排序
    * axis{0 or ‘index’, 1 or ‘columns’}, default 0: Axis to be sorted.
    * 多列排序 by=['col1','col2']
------------
## 读写操作
### 读取CSV/EXCEL
* `pd.read_csv((filepath, sep=',', header=0, names=None, index_col=None, usecols=None, dtype=None, skiprows=None, nrows=None,encoding=None) ` 读取csv
    * header默认为0, 设置header=None 意思是没有表头
    * `names=['col1', 'col2']` 自己设置column names,与header=None搭配使用
    * usecols:  指定需要读取的列,默认全部读取  
    * index_col 将某一列设置为索引列 
    * dtype 指定数据类型,可以用字典指定每一列的数据类型,例如 {‘a’: np.float64, ‘b’: np.int32, ‘c’: ‘Int64’} 
    * skiprows: 从文件开始处，需要跳过的行数或行号列表
    * nrows: 从文件开头处需要读入的行数, 默认全读
* `pandas.read_excel(filepath, sheet_name=0, header=0, names=None, index_col=None, usecols=None, dtype=None,skiprows=None, nrows=None)`
    * sheet_name: 默认读第一张sheet,设置为None读所有sheet. 列表设置读取的sheet,例如:`[0, 1, "Sheet5"]`
  
### 写入CSV/EXCEL
* `df.to_csv(filepath, index=True, encoding=None)` 保存数据至csv
    * index: 是否将索引列保存，默认为True
    * encoding='utf-8-sig'   解决中文乱码问题
* `DataFrame.to_excel(excel_writer, sheet_name='Sheet1', columns=None, header=True, index=True, index_label=None, startrow=0, startcol=0, encoding=None)`
    * 可以先打开一个excel文件 `with pd.ExcelWriter(filepath) as writer:`
--------------
## 数据清洗
### 1. 处理缺失数据
判断是否有缺失数据:
* `Series.isnull()`
* `DataFrame.isnull()`
    * 返回bool型的DataFrame数据
    * `DataFrame.isnull().any(axis=0)` 默认, 判断是否有包含空值的列, 设置axis=1 判断是否有空值的行
    * `df.isnull().to_numpy().any()` 转化数据结构为ndarray,判断所有数据中是否有空值
* `DataFrame.notnull()` 非空值
  
丢弃缺失数值:
* `DataFrame.dropna(axis=0, how='any', thresh=None, subset=None, inplace=False)`
    * {0 or 'index', 1 or 'columns'} 默认为0,丢弃行. axis=1丢弃列
    * how{'any', 'all'}, 默认'any' 有一个空值就丢弃, how='all'全部空值才丢弃
    * subset: array-like 设置要检查是否有空值的列 例如`df.dropna(subset=["volume"])` 丢掉volume上有缺失数据的行
    * inplace: 默认不修改原数据,返回操作结果  
*  DataFrame.ffill(axis=0) 用前面的数据填充缺失数据, 最好先按值排序
*  DataFrame.bfill(axis=0) 用后面的数据填充缺失数据
### 2. 处理重复数据 
判断是否有重复数据:
* `DataFrame.duplicated(subset=None, keep='first')`
    * `subset=['col1','col2']` 只考虑subset中指定的列是否有重复值, 默认所有
* `DataFrame.nunique(axis=0, dropna=True)` 数每列有几个独特值,不把空值计算在内
* `Series.unique()` 返回唯一值, 类型为numpy.ndarray
    * Series.unique().tolist() 返回为列表
  
丢弃重复行:
* `DataFrame.drop_duplicates(subset=None, keep='first', inplace=False)`
    * keep{'first', 'last', False}, 默认first, 丢弃重复除了第一个数值. last:丢弃重复除了最后一个数值. False:全部丢弃

### 3. 替换数值
* `DataFrame.replace(to_replace=None, value=None, inplace=False, limit=None, regex=False, method='pad')`
  * to_replace: 需要被替换的值, 可以是数值,字符窜,列表,字典,正则表达
      * list列表: 如果to_replace和value,都为列表,这长度相同,一一对应着替换. 如果value为一个值,着to_replace列表中的值都会被替换为value
      * dict字典: 有3种情况:
          * 必须value=None,键值对应替换,  例如:{'a': 'b', 'y': 'z'} a->b, y->z
          * 必须value=None,键为col_name, 值为字典,{col_name:{to_replace: value}}. 例如: {'a': {'b': np.nan}} a列中值为'b'的被替换为np.nan
          * value必须有值, {col1:to_replace, col2:to_replace}. 例如:{'A': 0, 'B': 5} A列中的0 ,B列中5, 都替换成value的值
      * 正则表达: 设置regex=True, 或者to_replace=None, 在regex里写正则表达式
    