
# 二维折线图：Matplotlib基础入门与数据可视化

这个Python绘图项目使用经典的`matplotlib`函数库，专为生成高质量的静态、动态、交互式图表而设计。
本项目在一个图表中同时绘制了三条折线，便于用户对不同数据集（平均气温、最高气温、最低气温）进行对比与趋势分析。
让我们从这个最简单、最基础的项目开始，详细了解如何使用`matplotlib`绘制二维折线图！

---

# 目录

- [二维折线图：Matplotlib基础入门与数据可视化](#二维折线图matplotlib基础入门与数据可视化)
- [目录](#目录)
- [项目简介](#项目简介)
- [完整代码示例](#完整代码示例)
- [代码注释与详解](#代码注释与详解)
  - [1. 准备环境与数据](#1-准备环境与数据)
  - [2. 中文显示配置](#2-中文显示配置)
  - [3. 创建画布与绘图](#3-创建画布与绘图)
  - [4. 图表元素设置](#4-图表元素设置)
  - [5. 数据点标签与优化](#5-数据点标签与优化)
- [扩展示例与高级技巧](#扩展示例与高级技巧)
  - [示例一：自定义线条样式对照表](#示例一自定义线条样式对照表)
  - [示例二：添加文字注释（Annotation）](#示例二添加文字注释annotation)
  - [示例三：保存图表至文件](#示例三保存图表至文件)

---

# 项目简介

本项目旨在通过一个具体实例（滨海市气温变化趋势）来展示`matplotlib`中`pyplot`模块的核心功能，包括：
1.  **数据准备**：使用Python列表定义X轴和Y轴数据。
2.  **基本绘图**：使用`plt.plot()`函数绘制多条折线。
3.  **图表美化**：设置标题、轴标签、网格线和图例。
4.  **细节增强**：为数据点添加文本标签，提高图表可读性。

---

# 完整代码示例

```python
import matplotlib.pyplot as plt

# 1. 中文显示配置
# 设置中文黑体字体，解决中文乱码问题
plt.rcParams["font.family"] = ["SimHei"]
# 解决负号（'-'）显示为方框或其他乱码的问题
plt.rcParams["axes.unicode_minus"] = False

# 2. 准备数据 - 滨海市2010-2020年气温数据
year = [2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018, 2019, 2020]
avg_temps = [15.2, 14.8, 15.5, 15.0, 15.6, 15.8, 16.0, 15.9, 16.2, 16.1, 16.3]
max_temps = [38, 37, 39, 38, 40, 39, 41, 40, 42, 40, 41]
min_temps = [-6, -8, -5, -7, -4, -5, -3, -6, -4, -5, -3]

# 3. 创建画布对象
plt.figure(figsize = (12, 7)) # 创建一个尺寸为12x7英寸的画布

# 4. 绘制三条折线
# 平均气温：蓝色实线
plt.plot(year, avg_temps, color = "blue", linestyle = '-', marker = 'o',
        linewidth = 2.5, markersize = 7, label = '平均气温（摄氏度）')
# 最高气温：红色虚线
plt.plot(year, max_temps, color = 'red' , linestyle = '--',
         marker = 'o', linewidth = 2.5, markersize = 7, label = '最高气温（摄氏度）')
# 最低气温：绿色点划线
plt.plot(year, min_temps, color = 'green', linestyle = '-.',
         marker = 'o', linewidth = 2.5, markersize = 7, label = '最低气温（摄氏度）')

# 5. 设置图表元素
plt.title('滨海市2010-2020年气温变换趋势', fontweight = 'bold', fontsize = 16)
plt.xlabel('年份', fontsize = 14)
plt.ylabel('气温（摄氏度）', fontsize = 14)

# 添加网格线
plt.grid(True, linestyle = '--', alpha = 0.7)

# 添加图例
plt.legend(fontsize = 12, loc = 'best')

# 设置X轴刻度（显示所有年份并旋转45度）
plt.xticks(year, rotation = 45)
plt.yticks(fontsize = 11)

# 6. 为数据点添加标签
# ha='center' (水平居中), va='bottom' (垂直靠下，让文字在点上方)
# 注意：这里将文本颜色改成了与线条一致，并统一向上偏移1个单位
for x, y in zip(year, avg_temps):
    plt.text(x, y + 1, f"{y}", ha = 'center', va = 'bottom', fontsize = 10, color = 'blue')
for x, y in  zip(year, max_temps):
    plt.text(x, y + 1, f'{y}', ha = 'center', va = 'bottom', fontsize = 10, color = 'red')
for x, y in zip(year, min_temps):
    plt.text(x, y + 1, f'{y}', ha = 'center', va = 'bottom', fontsize = 10, color = 'green')

# 7. 布局优化与显示
# 自动调整子图参数，使图表元素不重叠，并充分利用图窗空间
plt.tight_layout()

# 显示图形
plt.show()
```

---

# 代码注释与详解

## 1. 准备环境与数据

```python
import matplotlib.pyplot as plt
```
**解释:** 导入`matplotlib`库中的`pyplot`模块并命名为`plt`。`pyplot`提供了一个类似MATLAB的绘图接口，是`matplotlib`中最常用的模块。

```python
# 准备数据 - 滨海市2010-2020年气温数据
year = [2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018, 2019, 2020]
avg_temps = [15.2, 14.8, 15.5, 15.0, 15.6, 15.8, 16.0, 15.9, 16.2, 16.1, 16.3]
# ... max_temps 和 min_temps 数据
```
**要求:**
*   **X/Y 数据量一致**：`plt.plot()`要求X轴数据和Y轴数据的元素数量必须完全对应，否则会报错（`len(X) == len(Y)`）。

## 2. 中文显示配置

```python
# 设置中文显示，确保标题和标签能正常显示中文
plt.rcParams["font.family"] = ["SimHei"] # 设置字体为黑体（常用）
plt.rcParams["axes.unicode_minus"] = False # 解决负号显示问题
```
**解释:**
*   `plt.rcParams`: 这是一个字典，用于配置`matplotlib`的运行时参数。
*   `"font.family"`: 用于设置文本字体。`SimHei`（黑体）是Windows系统中常用的中文字体。在其他系统（如Linux/macOS）上可能需要安装或使用其他字体。
*   `"axes.unicode_minus"`: 当设置为`False`时，可以正确显示Unicode负号，避免在某些字体下负号显示为方块的问题。

## 3. 创建画布与绘图

```python
# 创建图形对象，设置画布大小为12*7英寸
plt.figure(figsize = (12, 7))
```
**解释:**
*   `plt.figure()`: 创建一个新的图表窗口（Figure对象）。
*   `figsize = (12, 7)`: 设置图形的宽度为12英寸，高度为7英寸。

```python
# 绘制平均气温折线图
plt.plot(year, avg_temps, color = "blue", linestyle = '-', marker = 'o',
        linewidth = 2.5, markersize = 7, label = '平均气温（摄氏度）')
# ... 绘制其他两条线
```
**`plt.plot()` 参数详解:**
| 参数 | 描述 | 示例值 |
| :--- | :--- | :--- |
| 第一个参数 | X轴数据 | `year` |
| 第二个参数 | Y轴数据 | `avg_temps` |
| `color` | 折线的颜色 | `"blue"`, `'red'`, `"#FF5733"` (Hex值) |
| `linestyle` | 折线的样式 | `'-'` (实线), `'--'` (虚线), `'-.'` (点划线) |
| `marker` | 数据点的标记样式 | `'o'` (圆圈), `'s'` (方块), `'^'` (三角) |
| `linewidth` | 折线的粗细 | `2.5` |
| `markersize` | 数据标记的大小 | `7` |
| `label` | 图例中显示的文本 | `'平均气温（摄氏度）'` |

## 4. 图表元素设置

```python
# 添加标题，设置为黑体加粗，字号为16
plt.title('滨海市2010-2020年气温变换趋势', fontweight = 'bold', fontsize = 16)

# 添加坐标轴标签，字号设置为14
plt.xlabel('年份', fontsize = 14)
plt.ylabel('气温（摄氏度）', fontsize = 14)
```
**解释:**
*   `plt.title()`: 设置图表主标题。
    *   `fontweight='bold'`: 设置字体为加粗。
    *   `fontsize`: 设置字体大小。
*   `plt.xlabel()`/`plt.ylabel()`: 分别设置X轴和Y轴的标签。

```python
# 添加网格线，使用虚线样式，透明度0.7
plt.grid(True, linestyle = '--', alpha = 0.7)
```
**解释:**
*   `plt.grid(True)`: 启用网格线。
*   `linestyle = '--'`: 设置网格线为虚线。
*   `alpha = 0.7`: 设置网格线的透明度。值域为**0 (完全透明)**到**1 (完全不透明)**。

```python
# 添加图例，字体大小12，位置自动选择最佳位置
plt.legend(fontsize = 12, loc = 'best')
```
**解释:**
*   `plt.legend()`: 显示图例，图例内容来源于`plt.plot()`中的`label`参数。
*   `loc = 'best'`: 让`matplotlib`自动选择一个**不遮挡数据**的最佳位置来放置图例。其他可选值包括 `'upper left'`, `'lower right'`, 等。

```python
# 设置坐标轴刻度，x轴标签旋转45度
plt.xticks(year, rotation = 45)
plt.yticks(fontsize = 11)
```
**解释:**
*   `plt.xticks(year)`: 设置X轴的刻度位置为`year`列表中的所有值（即每一年都有一个刻度）。
*   `rotation = 45`: 将X轴的刻度标签旋转45度，防止年份标签重叠，提高可读性。
*   `plt.yticks()`: 调整Y轴刻度标签的字体大小。

## 5. 数据点标签与优化

```python
# 为平均气温添加数据标签
for x, y in zip(year, avg_temps):
    # f"{y}" 使用 f-string 格式化输出数据y
    # va='bottom' 让标签的底部与 y + 1 的位置对齐，确保标签在点的上方
    plt.text(x, y + 1, f"{y}", ha = 'center', va = 'bottom', fontsize = 10, color = 'blue')
# ... 其他两条线的标签
```
**`plt.text()` 参数详解:**
*   `x, y + 1`: 文本标签的坐标。这里将Y坐标向上偏移了`1`个单位（`y + 1`），使得标签位于数据点的上方。
*   `f"{y}"`: 使用**f-string**（格式化字符串字面值）将变量`y`转换为字符串。
*   `ha = 'center'`: **水平对齐**（Horizontal Alignment）设置为居中。
*   `va = 'bottom'`: **垂直对齐**（Vertical Alignment）设置为“底部”，即文本的底边与指定Y坐标对齐。
*   `zip(year, avg_temps)`: Python内置函数，用于将两个列表按索引打包成一个个对应的元组`(x, y)`，方便循环迭代。

```python
plt.tight_layout()
```
**解释:**
*   `plt.tight_layout()`是`matplotlib`中一个非常实用的函数。它的主要功能是**自动调整图表中的子图参数**，以确保图表元素（如标题、坐标轴标签、刻度标签、图例等）之间不会重叠，并尽可能美观地填充整个图窗区域。

```python
# 显示图形
plt.show()
```
**解释:** 调用此函数才会将设置完成的图形窗口显示出来。

---

# 扩展示例与高级技巧

## 示例一：自定义线条样式对照表

| 样式/标记 | 完整参数 | 描述 |
| :---: | :---: | :--- |
| **实线** | `linestyle='-'` | 默认线条 |
| **虚线** | `linestyle='--'` | 用于次要数据的趋势展示 |
| **点划线** | `linestyle='-.'` | |
| **点线** | `linestyle=':'` | |
| **圆点** | `marker='o'` | 默认标记之一 |
| **方块** | `marker='s'` | Square |
| **三角** | `marker='^'` | |
| **加号** | `marker='+'` | |

**代码示例：**
```python
# 绘制一条不同的折线
plt.plot(year, max_temps, color='purple', linestyle=':', marker='s', label='新的样式')
```

## 示例二：添加文字注释（Annotation）

除了数据标签，我们还可以添加带有箭头的注释，以突出显示图表中的关键数据点。

**代码示例：**
```python
# 找出最高气温出现年份和数值
max_temp_year = 2018
max_temp_value = 42

plt.annotate(
    '历史最高气温',                 # 注释文本
    xy=(max_temp_year, max_temp_value), # 箭头指向的数据点坐标
    xytext=(2016, 43),              # 文本标签的起始坐标
    arrowprops=dict(facecolor='black', shrink=0.05, width=1, headwidth=8), # 箭头样式
    fontsize=12,
    color='red'
)
# 注意：此代码片段应放在 plt.show() 之前
```

## 示例三：保存图表至文件

如果不需要在屏幕上显示图表，而是直接保存为图片文件，可以使用`plt.savefig()`代替`plt.show()`。

**代码示例：**
```python
# 替换 plt.show()
plt.savefig(
    '滨海市气温趋势图.png', # 文件名和格式
    dpi=300,             # 分辨率，300dpi是常用的高质量标准
    bbox_inches='tight'  # 确保所有图表元素（如标签）都能被完整保存
)
```
