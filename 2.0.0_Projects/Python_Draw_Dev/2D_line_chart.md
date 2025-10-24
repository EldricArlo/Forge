# 二维折线图
这个python绘图使用的是matplotlib函数库，这是一个很经典的库；
这个项目中同时在一个白板中绘制了三条折线，方便用户的对比与趋势比较；
开始吧，这个最简单的项目！！！

---

# 目录

> - [代码示例部分](#完整代码示例)
> - [解释性注释部分](#代码注释以及语法相关解释)

# 完整代码示例

```python
import matplotlib.pyplot as plt

plt.rcParams["font.family"] = ["SimHei"]
plt.rcParams["axes.unicode_minus"] = False

year = [2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018, 2019, 2020]
avg_temps = [15.2, 14.8, 15.5, 15.0, 15.6, 15.8, 16.0, 15.9, 16.2, 16.1, 16.3]
max_temps = [38, 37, 39, 38, 40, 39, 41, 40, 42, 40, 41]
min_temps = [-6, -8, -5, -7, -4, -5, -3, -6, -4, -5, -3]

plt.figure(figsize = (12, 7))

plt.plot(year, avg_temps, color = "blue", linestyle = '-', marker = 'o',
        linewidth = 2.5, markersize = 7, label = '平均气温（摄氏度）')
plt.plot(year, max_temps, color = 'red' , linestyle = '--',
         marker = 'o', linewidth = 2.5, markersize = 7, label = '最高气温（摄氏度）')
plt.plot(year, min_temps, color = 'green', linestyle = '-.',
         marker = 'o', linewidth = 2.5, markersize = 7, label = '最低气温（摄氏度）')

plt.title('滨海市2010-2020年气温变换趋势', fontweight = 'bold', fontsize = 16)

plt.xlabel('年份', fontsize = 14)
plt.ylabel('气温（摄氏度）', fontsize = 14)

plt.grid(True, linestyle = '--', alpha = 0.7)

plt.legend(fontsize = 12, loc = 'best')

plt.xticks(year, rotation = 45)
plt.yticks(fontsize = 11)

for x, y in zip(year, avg_temps):
    plt.text(x, y + 1, f"{y}", ha = 'center', fontsize = 10, color = 'red')
for x, y in  zip(year, max_temps):
    plt.text(x, y + 1, f'{y}', ha = 'center', fontsize = 10, color = 'red')
for x, y in zip(year, min_temps):
    plt.text(x, y + 1, f'{y}', ha = 'center', fontsize = 10, color = 'green')

plt.show()
```

---

# 代码注释以及语法相关解释

```python
import matplotlib.pyplot as plt
```
导入matplotlib库中的pyplot模块并命名为plt

这个库的详细解释：[Matplotlib](../../Areas_Of_Knowledge/Programming_Languages/Python/libraries/Matplotlib_Guide.md)

```python
# 设置中文显示，确保标题和标签能正常显示中文
plt.rcParams["font.family"] = ["SimHei"]
plt.rcParams["axes.unicode_minus"] = False # 解决负号显示问题
```
这两行代码用于设置字体，在正常的显示器中是无法正常显示非拉丁字母的文字，而这两行代码可以完美的解决这个问题；


```python
# 准备数据 - 滨海市2010-2020年气温数据
year = [2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018, 2019, 2020]
avg_temps = [15.2, 14.8, 15.5, 15.0, 15.6, 15.8, 16.0, 15.9, 16.2, 16.1, 16.3]
max_temps = [38, 37, 39, 38, 40, 39, 41, 40, 42, 40, 41]
min_temps = [-6, -8, -5, -7, -4, -5, -3, -6, -4, -5, -3]
```
这是一个举例的数据；

要求每个y值数据数量应该和x数据量意义对应，否则会直接报错；

(x_element_numbers == y_element_numbers)；

```python
# 创建图形对象，设置画布大小为12*7英寸
plt.figure(figsize = (12, 7))
```
绘制一个x轴长度为12,y轴长度为7的背景白板；

```python
# 绘制平均气温折线图
# color = "bule" 蓝色线条
# linestyle = '-' 实线样式
# marker = 'o' 圆形数据标记
# linewidth = 2.5 线条宽度设置为2.5
# markersize = 7 标记大小为7
# label 图例文本
plt.plot(year, avg_temps, color = "blue", linestyle = '-', marker = 'o',
        linewidth = 2.5, markersize = 7, label = '平均气温（摄氏度）')
# 绘制最高气温折线图
# color = 'red' 红色线条
# linestyle = '--' 虚线样式
plt.plot(year, max_temps, color = 'red' , linestyle = '--',
         marker = 'o', linewidth = 2.5, markersize = 7, label = '最高气温（摄氏度）')
# 绘制最低气温折线图
# color = 'green' 绿色线条
# linestyle = '-.'
plt.plot(year, min_temps, color = 'green', linestyle = '-.',
         marker = 'o', linewidth = 2.5, markersize = 7, label = '最低气温（摄氏度）')
```
同时绘制三条折线图，括号内的第一个元素为x轴数据，第二个元素为y轴数据；

color - 设置折现的颜色；
linestyle - 设置折现的样式；
marker - 设置折点的样式；
linewidth - 设置折现的粗细；
markersize - 设置折点的大小；
label - 为折线设置标签；

```python
# 添加标题，设置为黑体加粗，字号为16
plt.title('滨海市2010-2020年气温变换趋势', fontweight = 'bold', fontsize = 16)
# 添加坐标轴标签，字号设置为14
plt.xlabel('年份', fontsize = 14)
plt.ylabel('气温（摄氏度）', fontsize = 14)
```
title - 给这个图像设置一个标题；

其中的fontweight给字体设置实体样式，fontsize给字体设置大小；

xlabel - 给这个图像的x轴设置一个标签；

ylabel - 给这个图像的y轴设置一个标签；

```python
# 添加网格线，使用虚线样式，透明度0.7使图标更清晰
plt.grid(True, linestyle = '--', alpha = 0.7)
```
grid - 网格，格子；

给整个图标设置一个网格的背景，其中的True为布尔值，默认是True；

alpha - 设置网格的透明度(透明度程度从0到1逐渐递增为完全不透明)；

```python
# 添加图例，字体大小12，位置自动选择最佳位置
plt.legend(fontsize = 12, loc = 'best')
```
自动由系统设置曲线为最佳的显示状况，使图像能够更合理的合适的显示出来；

loc - location，自动设置最佳的位置；

```python
# 设置坐标轴刻度，x轴标签旋转45度以便更好显示年份
plt.xticks(year, rotation = 45)
plt.yticks(fontsize = 11) # 调整y轴刻度字体大小
```
这个xticks和yticks分别设置在x轴和y轴上的元素的数据和样式；

rotation - 旋转，设置旋转45度角；

```python
# 为平均气温添加数据标签，显示具体数据
# x, y + 0.3 标签位置在数据点上方0.3单位处
# ha = 'center' 水平居中对齐
for x, y in zip(year, avg_temps):
    plt.text(x, y + 1, f"{y}", ha = 'center', va = 'top', fontsize = 10, color = 'red')
# 为最高气温添加数据标签
for x, y in  zip(year, max_temps):
    plt.text(x, y + 1, f'{y}', ha = 'center', va = 'bottom', fontsize = 10, color = 'red')
# 为最低气温添加数据标签
for x, y in zip(year, min_temps):
    plt.text(x, y + 1, f'{y}', ha = 'center', va = 'bottom', fontsize = 10, color = 'green')
```
循环每个折线的x数据，y数据打包成一个一个对应的元组；

然后将这些元组逐个打印到折线的位置；

harizontalalignment - 为水平对齐设置为居中；

verticalalignment - 为垂直对齐设置为折线的上方和下方；

```python
plt.tight_layout()
```
plt.tight_layout() 是 Python 的 Matplotlib 库中一个非常实用且常用的函数;

它的主要功能是自动调整图表中的子图参数，

以确保图表元素（如标题、坐标轴标签、刻度标签等）之间不会重叠，并尽可能美观地填充整个图窗区域;

```python
# 显示图形
plt.show()
```
显示设置完成的图像，否则图像不会被绘制出来；

