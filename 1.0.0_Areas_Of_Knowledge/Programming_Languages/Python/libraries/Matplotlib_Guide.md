# Areas_Of_Knowledge\Programming_Languages\Python\libraries\Matplotlib_Guide.md

## Matplotlib 语法详解：一份带有完整可跳转目录的深度指南

Matplotlib 是 Python 中应用最广泛的绘图库，以其强大的功能和灵活性著称，能够创建出版级别的图表。 本指南将为您提供一份详细、深刻且清晰的 Matplotlib 语法解析，并附带一个完整的、支持内部跳转的目录，帮助您快速掌握 Matplotlib 的核心功能。
Matplotlib 是 Python 中一个功能强大且广泛使用的第三方库，主要用于数据的可视化，它可以轻松地将数据转化为各种静态、动态、交互式的图表。
*   **官方网站**: [https://matplotlib.org/](https://matplotlib.org/)
*   **官方中文网站**: [https://matplotlib.org/zh_CN/](https://matplotlib.org/zh_CN/)
*   **官方文档**: [https://matplotlib.org/stable/contents.html](https://matplotlib.org/stable/contents.html)
*   **官方教程**: [https://matplotlib.org/stable/tutorials/index.html](https://matplotlib.org/stable/tutorials/index.html)
*   **示例库**: [https://matplotlib.org/stable/gallery/index.html](https://matplotlib.org/stable/gallery/index.html)
*   **GitHub 仓库**: [https://github.com/matplotlib/matplotlib](https://github.com/matplotlib/matplotlib)

### 目录

- [Areas\_Of\_Knowledge\\Programming\_Languages\\Python\\libraries\\Matplotlib\_Guide.md](#areas_of_knowledgeprogramming_languagespythonlibrariesmatplotlib_guidemd)
  - [Matplotlib 语法详解：一份带有完整可跳转目录的深度指南](#matplotlib-语法详解一份带有完整可跳转目录的深度指南)
    - [目录](#目录)
    - [1. Matplotlib 核心概念：Figure 与 Axes](#1-matplotlib-核心概念figure-与-axes)
      - [Figure：画布](#figure画布)
      - [Axes：坐标轴/绘图区域](#axes坐标轴绘图区域)
      - [创建 Figure 和 Axes](#创建-figure-和-axes)
    - [2. 基础绘图](#2-基础绘图)
      - [线形图 (Line Plot)](#线形图-line-plot)
      - [散点图 (Scatter Plot)](#散点图-scatter-plot)
      - [条形图/柱状图 (Bar Plot)](#条形图柱状图-bar-plot)
    - [3. 多子图 (Subplots)](#3-多子图-subplots)
      - [使用 `plt.subplots()`](#使用-pltsubplots)
      - [共享坐标轴](#共享坐标轴)
    - [4. 图表样式与自定义](#4-图表样式与自定义)
      - [颜色、线型和标记](#颜色线型和标记)
      - [标题、标签和图例](#标题标签和图例)
      - [使用样式表](#使用样式表)
    - [5. 3D 绘图](#5-3d-绘图)
      - [创建 3D 坐标轴](#创建-3d-坐标轴)
      - [3D 线形图和散点图](#3d-线形图和散点图)
      - [3D 曲面图](#3d-曲面图)
    - [6. 保存图表](#6-保存图表)

---

### 1. Matplotlib 核心概念：Figure 与 Axes

在 Matplotlib 中，所有绘图元素都组织在一个层次结构中。最顶层是 `Figure` 对象，它可以被看作是整个图表的画布。在一个 `Figure` 上可以有一个或多个 `Axes` 对象，每个 `Axes` 代表一个独立的绘图区域或子图。

#### Figure：画布

`Figure` 是所有绘图元素的顶级容器，包括 `Axes`、标题、图例等。 创建一个新的 `Figure` 通常是绘图的第一步。

#### Axes：坐标轴/绘图区域

`Axes` 是实际进行数据可视化的区域，包含了坐标轴、刻度、刻度标签以及绘制的数据。 它不仅仅是数学意义上的 x、y 轴，而是整个绘图区域。 大多数绘图操作都是通过 `Axes` 对象的方法来完成的。

#### 创建 Figure 和 Axes

创建 `Figure` 和 `Axes` 最推荐的方式是使用 `matplotlib.pyplot.subplots()` 函数。这个函数会同时创建一个 `Figure` 对象和一个或多个 `Axes` 对象。

**示例代码：**
```python
import matplotlib.pyplot as plt

# 创建一个 Figure 和一个 Axes
fig, ax = plt.subplots()

# 在 Axes 上绘制数据
ax.plot([1, 2, 3, 4], [10, 20, 25, 30])

# 显示图表
plt.show()
```

---

### 2. 基础绘图

Matplotlib 提供了丰富的函数来创建各种类型的图表。

#### 线形图 (Line Plot)

线形图用于展示数据随某个连续变量变化的趋势。使用 `ax.plot()` 方法可以轻松创建线形图。

**语法：**
`ax.plot(x, y, [fmt], **kwargs)`

*   `x`, `y`: 分别代表数据点的水平和垂直坐标。
*   `fmt`: 一个可选的格式化字符串，用于快速设置颜色、标记和线型。

**示例代码：**
```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 10, 100)
y = np.sin(x)

fig, ax = plt.subplots()
ax.plot(x, y, label='sin(x)')
ax.set_title('正弦函数图像')
ax.set_xlabel('x 轴')
ax.set_ylabel('y 轴')
ax.legend()
plt.show()
```

#### 散点图 (Scatter Plot)

散点图用于展示两个变量之间的关系。 使用 `ax.scatter()` 方法创建散点图。

**语法：**
`ax.scatter(x, y, s=None, c=None, marker=None, **kwargs)`

*   `s`: 标记的大小。
*   `c`: 标记的颜色。
*   `marker`: 标记的样式。

**示例代码：**
```python
import matplotlib.pyplot as plt
import numpy as np

x = np.random.rand(50)
y = np.random.rand(50)
colors = np.random.rand(50)
sizes = 1000 * np.random.rand(50)

fig, ax = plt.subplots()
ax.scatter(x, y, c=colors, s=sizes, alpha=0.5)
ax.set_title('散点图示例')
plt.show()
```

#### 条形图/柱状图 (Bar Plot)

条形图或柱状图用于比较不同类别的数据。 使用 `ax.bar()` 创建垂直条形图，使用 `ax.barh()` 创建水平条形图。

**语法：**
`ax.bar(x, height, width=0.8, **kwargs)`

*   `x`: 条形的位置。
*   `height`: 条形的高度。
*   `width`: 条形的宽度。

**示例代码：**
```python
import matplotlib.pyplot as plt

categories = ['A', 'B', 'C', 'D']
values = [15, 30, 22, 18]

fig, ax = plt.subplots()
ax.bar(categories, values)
ax.set_title('条形图示例')
ax.set_xlabel('类别')
ax.set_ylabel('数值')
plt.show()
```

---

### 3. 多子图 (Subplots)

在同一个 `Figure` 中创建多个绘图区域，可以方便地进行多组数据的对比分析。

#### 使用 `plt.subplots()`

`plt.subplots()` 是创建多子图网格最便捷的方法。 它返回一个 `Figure` 对象和一个包含所有 `Axes` 对象的 NumPy 数组。

**语法：**
`plt.subplots(nrows=1, ncols=1, **fig_kw)`

*   `nrows`: 子图的行数。
*   `ncols`: 子图的列数。

**示例代码：**
```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 10, 100)
y1 = np.sin(x)
y2 = np.cos(x)

# 创建一个 2 行 1 列的子图网格
fig, axs = plt.subplots(2, 1)

axs[0].plot(x, y1)
axs[0].set_title('正弦函数')

axs[1].plot(x, y2)
axs[1].set_title('余弦函数')

# 调整子图布局
fig.tight_layout()
plt.show()
```
`plt.subplot()` 函数也可以用于创建子图，它接受三个整数参数：行数、列数和当前子图的索引。

#### 共享坐标轴

在创建多子图时，可以使用 `sharex` 和 `sharey` 参数来共享 x 轴或 y 轴的刻度和范围。

**示例代码：**
```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 10, 100)
y1 = np.sin(x)
y2 = np.cos(x)

# 共享 x 轴
fig, axs = plt.subplots(2, 1, sharex=True)

axs[0].plot(x, y1)
axs[1].plot(x, y2)

plt.show()
```

---

### 4. 图表样式与自定义

Matplotlib 提供了丰富的选项来定制图表的外观，使其更具表现力和专业性。

#### 颜色、线型和标记

在 `plot` 和 `scatter` 等函数中，可以通过参数直接设置颜色、线型和标记样式。

*   **颜色 (color):** 可以使用颜色名称（如 'red'）、十六进制代码（如 '#FF0000'）或 RGB 元组。
*   **线型 (linestyle):** 例如 '-' (实线), '--' (虚线), ':' (点线) 等。
*   **标记 (marker):** 例如 'o' (圆圈), 's' (正方形), '^' (三角形) 等。

**示例代码：**
```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 10, 50)
y = x**2

fig, ax = plt.subplots()
ax.plot(x, y, color='blue', linestyle='--', marker='o')
plt.show()
```

#### 标题、标签和图例

清晰的标题、坐标轴标签和图例是优秀图表的关键组成部分。

*   `ax.set_title()`: 设置子图标题。
*   `ax.set_xlabel()` / `ax.set_ylabel()`: 设置 x 轴和 y 轴的标签。
*   `ax.legend()`: 显示图例。需要在绘图时通过 `label` 参数指定图例内容。

#### 使用样式表

Matplotlib 提供了一系列预定义的样式表，可以轻松地改变图表的整体外观。 使用 `plt.style.use()` 来应用样式。

**示例代码：**
```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 10, 100)
y = np.sin(x)

# 应用 'ggplot' 样式
plt.style.use('ggplot')

fig, ax = plt.subplots()
ax.plot(x, y)
plt.show()
```
您还可以通过修改 `rcParams` 或创建自己的 `.mplstyle` 文件来自定义样式。

---

### 5. 3D 绘图

Matplotlib 的 `mplot3d` 工具包提供了创建各种 3D 图表的功能。

#### 创建 3D 坐标轴

要创建 3D 图表，首先需要创建一个带有 `projection='3d'` 的 `Axes` 对象。

**示例代码：**
```python
import matplotlib.pyplot as plt

fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')
```

#### 3D 线形图和散点图

与 2D 图表类似，可以使用 `ax.plot3D()` 和 `ax.scatter3D()` 在 3D 空间中绘制数据点。

**示例代码：**
```python
import matplotlib.pyplot as plt
import numpy as np

fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')

z = np.linspace(0, 15, 1000)
x = np.sin(z)
y = np.cos(z)

ax.plot3D(x, y, z, 'gray')
plt.show()
```

#### 3D 曲面图

`ax.plot_surface()` 函数用于创建 3D 曲面图，通常需要先生成网格数据。

**示例代码：**
```python
import matplotlib.pyplot as plt
import numpy as np

fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')

x = np.arange(-5, 5, 0.25)
y = np.arange(-5, 5, 0.25)
X, Y = np.meshgrid(x, y)
R = np.sqrt(X**2 + Y**2)
Z = np.sin(R)

ax.plot_surface(X, Y, Z, cmap='viridis')
plt.show()
```

其他 3D 图表类型还包括 3D 条形图 (`bar3d`) 和线框图 (`plot_wireframe`) 等。

---

### 6. 保存图表

使用 `fig.savefig()` 或 `plt.savefig()` 函数可以将图表保存到文件中。 Matplotlib 支持多种文件格式，如 PNG、PDF、SVG 等。

**语法：**
`plt.savefig('filename.extension', dpi=300, bbox_inches='tight')`

*   `'filename.extension'`: 文件名和格式。
*   `dpi`: 分辨率（每英寸点数）。
*   `bbox_inches='tight'`: 调整图表以适应所有元素。

**示例代码：**
```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots()
ax.plot([1, 2, 3], [4, 5, 6])

# 保存为 PNG 文件
plt.savefig('my_plot.png', dpi=300)
```