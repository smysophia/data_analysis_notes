# Numpy笔记
`import numpy as np`  
  
## 一维数组转为numpy.ndarray
```py
lst = [1,2,3,4]
data = np.array(lst)
print(type(data))              # <class 'numpy.ndarray'>
# 属性:
print("ndim=", data.ndim)       # ndim= 1
print("shape=",data.shape)      # shape= (4,)
print("type=", data.dtype)      # type= int32
```

## 生成其他numpy.ndarray
```py
zeros_arr = np.zeros((3,4))  # 第一个参数 元组(3,4) 3行4列
ones_arr =  np.ones((3,4))
rand_arr = np.random.rand(4,5)  # 传入两个参数,行 列
arr = np.arange(10)            # array range [0 1 2 3 4 5 6 7 8 9]
arr0 = np.arange(1,10,1).reshape(3,3)   # [1,10)间隔为1, 3*3
arr2 = arr.reshape((5,2))       # 传入元组,或者不加括号也行
arr3 = arr2.astype(float)   # 将其中的每一个都转换类型float
lst = arr.tolist()   # 转为列表
```
  
## 多维切片
例如:  
data = 
[[0 1 2]  
 [3 4 5]  
 [6 7 8]]  
`print(data[:2,1:])` [头,第二行), [第一列,最后]  
[[1 2]  
 [4 5]]  
 `print(data[2,:])`  # shape (3,)  一维数据 
 `print(data[2:,:])`   # shape(1,3)  两个维度  

 # 条件索引
 arr[condition1 & condition2 | condition3]  不能用and or  
 `is_gt = data > 4`   # 生成布尔型bool数组  
 [[False False False]    
 [False False  True]    
 [ True  True  True]]    
 `greater = data[is_gt]`   # [5 6 7 8]  
 `data[(data > 4) & (data % 2 == 0)]`  # [6 8]  
 

 # 其他通用函数
 ```py
 # MEAN
print(np.mean(data))
print(np.mean(data, axis=0))  # 列求和
print(np.mean(data, axis=1))  # 行求和
# MAX MIN
print(np.max(data, axis=0))  # 每列最大值 numpy.ndarray
print(np.argmax(data))   # 最大值的索引
print(np.argmax(data, axis=0))  # 每行最大值的索引
# 累加
print(np.cumsum(data))  # 累加,shape(10,) ndim=1
print(np.cumsum(data, axis=0))  # 按列累加
# any all
print(np.any(data >5))
print(np.all(data >5))
# unique 求唯一值并返回排序结果
a = [[1,2,3],[3,4,4]]
print(np.unique(a))   # [1 2 3 4]
# 其他
print(np.rint(arr))  # 四舍五入为整数
print(np.isnan(arr))  # is not a number
```


# 读取数据为numpy.ndarray
```py
data_arr = np.loadtxt(filename, 
                    delimiter=',', 
                    dtype=str, 
                    skiprows=1,     # 跳过第一行，即跳过列名
                    usecols=col_index_lst    # 指定读取的列索引号
                    )
```




