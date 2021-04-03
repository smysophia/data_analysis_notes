# Matplotlib 笔记
- [Matplotlib 笔记](#matplotlib-笔记)
  - [官方文档](#官方文档)
  - [折线图](#折线图)
  - [散点图](#散点图)
  - [柱状图](#柱状图)
  - [直方图](#直方图)
  - [矩阵绘图](#矩阵绘图)
  - [子图](#子图)
  - [设置标题/刻度/图例/第二轴](#设置标题刻度图例第二轴)
    - [标题](#标题)
    - [刻度](#刻度)
    - [图例](#图例)
    - [第二坐标轴设置](#第二坐标轴设置)
  - [其他设置](#其他设置)
  - [# Seaborn笔记](#-seaborn笔记)
  - [对列的统计](#对列的统计)
  - [单变量分布](#单变量分布)
  - [双变量分布](#双变量分布)
  - [变量关系](#变量关系)
  - [类别散布](#类别散布)
  - [# Bokeh](#-bokeh)
  - [接口charts 版本0.12.5 更高版本不使用](#接口charts-版本0125-更高版本不使用)
    - [Scatter 散点图](#scatter-散点图)
    - [柱状图](#柱状图-1)
    - [盒子图](#盒子图)
  - [接口plotting](#接口plotting)
- [3D绘图](#3d绘图)
    - [3D曲线](#3d曲线)
    - [散点图](#散点图-1)
    - [柱状图](#柱状图-2)
- [pandas绘图](#pandas绘图)
  - [python中的颜色](#python中的颜色)

----------------------------
## 官方文档
[实用官方文档](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.html)

Matplotlib 笔记
* 引入包
    `import matplotlib.pyplot as plt` 
* 创建单一画布
    `fig = plt.figure(figsize=(20, 5))`
* 创建子图画布
  `fig, ax = plt.subplots(几行,几列,figsize=(20,5))`
* 生成数据
    `random_data = np.random.randn(100)`
* 绘制和显示
    `plt.show()`

## 折线图
***
ax.plot()
plt.plot(x, y, fmt, x2,y2,fmt,...)
* x轴 可以不提供,默认0,1,2...
* y: 数据
* fmt:颜色 标记 线型 例如:
  * 'r.--'红色点标记-----虚线
  * 'gv:'绿色下三角形.....点虚线
  * 'b<-'蓝色左三角形实线
  * 'ys-.' 黄色正方形-.-.-.线 
* **kwargs
  * label 在图例中显示的label
  * linewidth
  
[plot官方文档](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.plot.html#matplotlib.pyplot.plot)



## 散点图
***
plt.scatter(x, y, s, c, marker)
* x轴数据, y轴数据
* s= size 点的大小
* c= color r g b y m(magenta 赤紫色) c(cyan 青蓝色) w(white) k(black)
* marker 点的形状. 常用:o + * . x  ^ > < v s(square) d(diamond) p(pentagram) h(hexagram)
* 例子:
  `plt.scatter(x, y1, s=10, c='r', marker='x')`
  `plt.scatter(x, y2, s=20, c='g', marker='s')`

  
  
## 柱状图
  ***
  plt.bar(x, height, width, bottom,color, edgecolor, tick_label,label)
  * x: x轴的位置序列.
    * 一般采用arange函数产生一个序列`x=np.arange(n)+1` 1,2,3,4,...
    * 设置x+width, 并列柱状图
  * height: y轴数值序列,为绘制的数据
  * width: 柱形图宽度, 默认0.8
  * bottom: 柱子距离x轴的高度
    * 设置bottom=x1, 可将x2数据堆叠在x1数据上
  * color or facecolor or fc: 柱形图填充色
  * edgecolor: 柱形图边缘颜色
  * tick_label: 轴刻度的标签. 例如: `tick_label=['TOM','LILEI','LILY']`
  * label: 标签 需要外加`plt.legend('upper left')`以显示标签, 默认右上
  
[bar官方文档](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.bar.html#matplotlib.pyplot.bar)


## 直方图
***
plt.hist(x, bins, rwidth)
* x:数据
* bins: 分组个数或者分组的边界.例如[20,30,40,50,60]
* rwieth: 默认为1, 柱子的宽度
* orientation: 默认'vertical`,可以设置为'horizontal'

## 矩阵绘图
***
plt.imshow(X, cmap)
* X 矩阵数据（二维数组）
* cmap颜色主题 `cmap=plt.cm.gnuplot` 例如:twilight, summer, ocean, hot
* plt.colorbar()

## 子图
***
plt.subplots(nrows, ncols, sharex, sharey)
* nrows, ncols分割的行数和列数
* sharex, sharey是否共享x轴、y轴
* 返回有两个:  新创建的figure和subplot数组,subplot是由AxesSubplot组成的ndarray


## 设置标题/刻度/图例/第二轴
  ***
  ### 标题
  * 图的名字
    * plt.title('Title', color, fontsize=9, fontfamily='sans-serif', fontstyle='italic')
    * ax.set_title('Title')
  * 坐标轴的名字
    * plt.xlabel('label name'), plt.ylabel()
    * ax.set_xlabel('label name'), ax.set_ylabel() 
  ### 刻度
  * 刻度的范围
    * plt.xlim(列表), plt.ylim() 例如: `plt.xlim([0,500])`
    * ax.set_xlim(列表), ax.set_ylim() 
  * 设置显示的刻度 
    * plt.xticks(列表), plt.yticks() 例如: `plt.xticks([0, 100, 200])`
    * ax.set_xticks(列表), ax.set_yticks()
  * 设置刻度标签
    * 使用plt.xticks(列表), plt.yticks()进行配置 `plt.xticks([0, 100, 200], ['x1', 'x2', 'x3'])`
    * plt.xticks(rotation=90) 竖着写刻度
    * ax.set_xticklabels(), ax.set_yticklabels() `ax.set_xticklabels(['x1', 'x2', 'x3', 'x4', 'x5'])`
  ### 图例 
  * 图例 legend(labels,loc)
    * labels:list of str, optional 图例的标签. 例如:`ax.legend(labels=['食物','运动'], loc='best')`
    * loc参数: best, upper right, upper left, lower right, lower left, right, center left, center right, lower center, upper center, center
  ### 第二坐标轴设置
  * ax.right_ax.set_ylabel
  * ax.right_ax.set_ylim(列表)

## 其他设置
***
* 正常显示中文标签
  `plt.rcParams['font.sans-serif']=['SimHei']`
* 解决保存图像是负号'-'显示为方块的问题
  `matplotlib.rcParams['axes.unicode_minus'] = False`

<br>
<br>

# Seaborn笔记
-------------------------
`import seaborn as sns`
## 对列的统计
Show the counts of observations in each categorical bin using bars.
sns.countplot(x=None, y=None, hue=None, data=None)
`sns.countplot(data=df, x='group')`

## 单变量分布
sns.kdeplot(data)
* 核密度估计图(kernel density estimation)
sns.distplot(kde=True, hist=True, rug=False)
* kde 核密度图
* hist 直方图
* rug 观测条
  
## 双变量分布
sns.jointplot(x, y, data, kind)
* x, y 二维数据，向量或字符串. x y的数据类型必须是数值
* data，如果x, y是字符串data 应该为DataFrame
* kind='scatter' 默认二维散点图. ['scatter', 'hist', 'hex', 'kde', 'reg', 'resid']
  ```py
  df = pd.DataFrame({'x': np.random.randn(500),
                   'y': np.random.randn(500)})
  # 二维散点图
  sns.jointplot(x='x', y='y', data=df) 
  ```
  <img src='https://github.com/smysophia/markdown_pictures/blob/main/sns.png' title='sns jointplot' width="150" height="150">

## 变量关系
sns.pairplot(data, hue, vars, kind, diag_kind)
* data: DataFrame数据集，列为变量，行为样本
* hue：数据集中作为类别的列名 `sns.pairplot(data, hue='species')`
* vars：可视化的列（默认可视化所有列间的关系） `sns.pairplot(data, hue='species', vars=['sepal_length', 'sepal_width'])`
* kind：默认scatter 散点，reg 添加拟合线 ['scatter', 'hist', 'hex', 'kde', 'reg', 'resid']
* diag_kind：对角线的图像，默认hist 直方图，kde核密度估计图

## 类别散布
加载seaborn自带数据
`tips_data = sns.load_dataset('tips')`
`titanic_data = sns.load_dataset("titanic")`
`exe_data = sns.load_dataset('exercise')`
类别散布图:
* 分布散点图 sns.stripplot() `sns.stripplot(x='tip', y='day', data=tips_data)`
* 分族散点图 sns.swarmplot() `sns.swarmplot(x='tip', y='day', data=tips_data, hue='sex')`
  * dodge=False 可以设置为True 分开类别  will separate the strips for different hue levels 
类别数据可视化:
* 盒子图 sns.boxplot(x,y,data,hue) `sns.boxplot(x='day', y='tip', data=tips_data, hue='sex')`
* 小提琴图 sns.violinplot()
类别内统计图:
* 柱状图 sns.barplot()
* 点图 sns.pointplot()

<br>

# Bokeh
---------------------
`from bokeh.io import output_notebook, output_file, show`
`from bokeh.charts import Scatter, Bar, BoxPlot, Chord`
show in Jupyter notebook
`output_notebook()`

## 接口charts 版本0.12.5 更高版本不使用
### Scatter 散点图
bokeh.charts.Scatter(data, x, y, title, xlabel, ylabel)
`p = Scatter(data=exe_data, x='id', y='pulse', title='散点图', xlabel='ID', ylabel='脉搏')`
### 柱状图
bokeh.charts.Bar(data, label, values, agg, stack, group )
* label: 横轴的分组
* values: 纵轴的值，或“聚合”后的值
* agg: 聚合函数，默认为sum
* stack: 层级分组--堆叠柱状图
* group: 层级分组--分组柱状图
* 例如:`p = Bar(data=exe_data, values='pulse', label='diet', stack='kind', title='堆叠柱状图', agg='mean', xlabel='饮食', ylabel='脉搏（均值）')`
### 盒子图
bokeh.charts.Boxplot(data, label, values)
`box = BoxPlot(data=exe_data, values='pulse', label='diet', title='盒子图')`

## 接口plotting
`from bokeh.plotting import figure`
```py
# 设置画布
p = figure(plot_width=400, plot_height=400, title='测试')
p.xaxis.axis_label = 'id'
p.yaxis.axis_label = '脉搏'
```
图形元素:`bokeh.plotting.figure.circle(x, y, size, color, alpha)`
柱状图:`p.vbar(x=exe_data['id'],top=exe_data['pulse'],width=0.6)`
  
   
# 3D绘图
`from mpl_toolkits.mplot3d import Axes3D`
```py
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
# 创建3D图像对象
fig = plt.figure()
ax = Axes3D(fig)
# 绘制图像
ax.plot(xline, yline, zline)
# 设置坐标轴
ax.set_xlabel('X')
# 展示
plt.show()
```
### 3D曲线
Axes3D.plot(xs, ys, zs, zdir)
* zdir 竖直的轴,默认为z轴
### 散点图
Axes3D.scatter(xs, ys, zs, zdir, s, c, marker)
* s: size
* c: color:
* marker
* `ax.scatter(x1, y1, z1, s=10, c='r', marker='o')`
### 柱状图
Axes3D.bar(left, height, zs, zdir)
* left: 柱子处于“左边”坐标轴的位置
* zs:柱子处于z坐标轴的位置
  ```py
  # 绘制图像
  ax.bar(x, y1, 0,zdir='y' )
  ax.bar(x, y2, 1,zdir='y')
  ax.set_yticks([0, 1])
  ```

# pandas绘图
plot(x, y, kind)
* 默认x轴
* kind默认line, 可设置bar barh hist box area scatter pie
`iris_data.groupby('species').mean().plot(kind='bar')`
饼图按类别计算
```py
count_ser = iris_data.groupby('species').size()
count_ser.name = 'Count'
ax = count_ser.plot(kind='pie', autopct='%.2f%%', figsize=(6, 6))  # 设置画布大小,默认是长方形
# 避免标签重叠
ax.yaxis.labelpad = 30
```
其他参数:
* figsize
* title
* grid: 网格
* xticks: sequence. Values to use for the xticks
* xlim:2-tuple/list   Set the x limits of the current axes.
* xlabel
* fontsize: Font size for xticks and yticks.
* colormapstr or matplotlib colormap object, default None
* stacked


## python中的颜色

<img src='https://github.com/smysophia/markdown_pictures/blob/main/python%20color.png' title='color'>
