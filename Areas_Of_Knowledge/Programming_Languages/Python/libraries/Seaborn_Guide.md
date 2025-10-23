# Areas_Of_Knowledge\Programming_Languages\Python\libraries\Seaborn_Guide.md

## Seaborn 库语法详解：一份深入、清晰的指南

Seaborn 是一个基于 Matplotlib 的 Python 数据可视化库，它提供了一个高级界面，用于绘制引人入胜且内容丰富的统计图形。 Seaborn 与 Pandas 数据结构的紧密集成，使得从数据框中直接创建复杂的图表变得异常简单。 本文将为您提供一份详尽的 Seaborn 语法指南，从基本概念到高级应用，并附有可跳转的完整目录，帮助您快速掌握并精通 Seaborn。

### 目录

- [Areas\_Of\_Knowledge\\Programming\_Languages\\Python\\libraries\\Seaborn\_Guide.md](#areas_of_knowledgeprogramming_languagespythonlibrariesseaborn_guidemd)
  - [Seaborn 库语法详解：一份深入、清晰的指南](#seaborn-库语法详解一份深入清晰的指南)
    - [目录](#目录)
    - [Seaborn 核心概念](#seaborn-核心概念)
      - [Figure-level 与 Axes-level 函数](#figure-level-与-axes-level-函数)
      - [与 Matplotlib 的关系](#与-matplotlib-的关系)
    - [关系型图表 (Relational Plots)](#关系型图表-relational-plots)
      - [`relplot()`](#relplot)
      - [`scatterplot()`](#scatterplot)
      - [`lineplot()`](#lineplot)
    - [分类型图表 (Categorical Plots)](#分类型图表-categorical-plots)
      - [`catplot()`](#catplot)
      - [分类散点图](#分类散点图)
        - [`stripplot()`](#stripplot)
        - [`swarmplot()`](#swarmplot)
      - [分类分布图](#分类分布图)
        - [`boxplot()`](#boxplot)
        - [`violinplot()`](#violinplot)
        - [`boxenplot()`](#boxenplot)
      - [分类估计图](#分类估计图)
        - [`pointplot()`](#pointplot)
        - [`barplot()`](#barplot)
        - [`countplot()`](#countplot)
    - [分布型图表 (Distribution Plots)](#分布型图表-distribution-plots)
      - [`displot()`](#displot)
      - [`histplot()`](#histplot)
      - [`kdeplot()`](#kdeplot)
      - [`ecdfplot()`](#ecdfplot)
      - [`rugplot()`](#rugplot)
    - [回归模型图 (Regression Plots)](#回归模型图-regression-plots)
      - [`regplot()`](#regplot)
      - [`lmplot()`](#lmplot)
    - [矩阵型图表 (Matrix Plots)](#矩阵型图表-matrix-plots)
      - [`heatmap()`](#heatmap)
      - [`clustermap()`](#clustermap)
    - [多图网格 (Multi-plot Grids)](#多图网格-multi-plot-grids)
      - [`FacetGrid`](#facetgrid)
      - [`PairGrid`](#pairgrid)
      - [`pairplot()`](#pairplot)
    - [风格和颜色管理](#风格和颜色管理)
      - [设置图表风格](#设置图表风格)
      - [管理颜色面板](#管理颜色面板)

---

### Seaborn 核心概念

#### Figure-level 与 Axes-level 函数

Seaborn 的绘图函数可以分为两大类：**Figure-level** (图形级别) 和 **Axes-level** (坐标轴级别)。

*   **Axes-level 函数**：直接在给定的 Matplotlib `Axes` 对象上绘图。例如 `scatterplot()` 和 `boxplot()`。这类函数提供了更灵活的控制，允许用户将多个 Seaborn 图表组合到复杂的 Matplotlib 图形布局中。
*   **Figure-level 函数**：通过管理 Matplotlib 的 Figure 和 Axes 来创建图表。例如 `relplot()`、`catplot()` 和 `displot()`。 这类函数通过 `kind` 参数来调用对应的 Axes-level 函数，并且可以轻松地创建分面图 (faceted plots)。

#### 与 Matplotlib 的关系

Seaborn 是建立在 Matplotlib 之上的，这意味着你可以将 Seaborn 的功能与 Matplotlib 的自定义选项结合使用，以获得更高程度的控制和定制化。

---

### 关系型图表 (Relational Plots)

关系型图表用于可视化两个数值变量之间的统计关系。

#### `relplot()`

`relplot()` 是一个 Figure-level 函数，用于绘制关系型图表。 它是 `scatterplot()` 和 `lineplot()` 的高级接口，通过 `kind` 参数进行切换（默认为 `"scatter"`）。 它的主要优势在于能够轻松创建按不同子集划分的多面图。

**关键参数**:
*   `data`: 用于绘图的 Pandas DataFrame。
*   `x`, `y`: 指定 x 轴和 y 轴的变量。
*   `hue`: 根据一个分类变量对数据进行颜色编码。
*   `style`: 根据一个分类变量改变标记点的样式。
*   `size`: 根据一个数值变量改变标记点的大小。
*   `col`, `row`: 根据分类变量在不同的列或行中创建子图。
*   `kind`: 指定要绘制的图表类型，`"scatter"` 或 `"line"`。

#### `scatterplot()`

用于绘制散点图，展示两个数值变量之间的关系。

#### `lineplot()`

用于绘制线图，通常用来展示一个变量随另一个变量（通常是时间）变化的趋势。

---

### 分类型图表 (Categorical Plots)

分类型图表用于展示一个数值变量和一个或多个分类变量之间的关系。

#### `catplot()`

与 `relplot()` 类似，`catplot()` 是一个 Figure-level 函数，统一了所有分类型图表的接口。

**关键参数**:
*   `kind`: 指定图表类型，包括 `"strip"`, `"swarm"`, `"box"`, `"violin"`, `"boxen"`, `"point"`, `"bar"`, `"count"`。

#### 分类散点图

这类图表会展示每个类别下的所有数据点。

##### `stripplot()`

绘制一个其中一维是分类变量的散点图。当数据点较多时可能会出现重叠。

##### `swarmplot()`

与 `stripplot()` 类似，但它使用算法调整数据点的位置以避免重叠，从而更好地展示数据的分布。

#### 分类分布图

这类图表提供了每个类别下数据分布的摘要信息。

##### `boxplot()`

箱形图，展示了数据的中位数、四分位数和异常值。

##### `violinplot()`

小提琴图，结合了箱形图和核密度估计图的特点，不仅展示了摘要统计信息，还显示了数据的分布形状。

##### `boxenplot()`

增强版的箱形图，适合于较大的数据集，能够提供关于数据分布形状更详细的信息。

#### 分类估计图

这类图表通过使用统计估计来展示每个类别数据的集中趋势。

##### `pointplot()`

点图通过点的位置来表示集中趋势的估计值（默认为均值），并用误差线表示置信区间。

##### `barplot()`

条形图使用条形的高度来表示数据的集中趋势。

##### `countplot()`

计数图，用于显示每个类别中的观测数量。

---

### 分布型图表 (Distribution Plots)

分布型图表用于可视化单个变量（单变量分布）或两个变量（双变量分布）的数据分布情况。

#### `displot()`

作为 Figure-level 函数，`displot()` 提供了绘制分布图的统一接口。

**关键参数**:
*   `kind`: 可选 `"hist"`, `"kde"`, `"ecdf"`。

#### `histplot()`

直方图，通过条形的高度来表示数据在不同区间内的频数。

#### `kdeplot()`

核密度估计图，用平滑的曲线来估计数据的概率密度函数。

#### `ecdfplot()`

经验累积分布函数图，展示了数据中小于或等于某个值的观测所占的比例。

#### `rugplot()`

在坐标轴上为每个数据点绘制一条小的垂直刻度线，用于显示数据的边际分布。

---

### 回归模型图 (Regression Plots)

回归模型图用于可视化两个变量之间的线性关系。

#### `regplot()`

`regplot()` 是一个 Axes-level 函数，用于绘制散点图并拟合一条线性回归模型。

#### `lmplot()`

`lmplot()` 是一个 Figure-level 函数，它结合了 `regplot()` 和 `FacetGrid` 的功能，可以轻松地在不同的数据子集上拟合和可视化回归模型。

---

### 矩阵型图表 (Matrix Plots)

矩阵图用于可视化矩阵形式的数据。

#### `heatmap()`

热力图将矩阵数据表示为颜色编码的图。 这对于展示变量之间的相关性矩阵非常有用。

#### `clustermap()`

聚类图将数据矩阵进行层次聚类，并用热力图的形式进行可视化，有助于发现数据中的结构和模式。

---

### 多图网格 (Multi-plot Grids)

Seaborn 提供了强大的工具来创建结构化的多图网格，以便在不同的数据子集上比较图表。

#### `FacetGrid`

`FacetGrid` 是一个通用的类，允许用户根据数据集中的变量来创建子图网格，然后将特定的绘图函数映射到每个子图上。

#### `PairGrid`

`PairGrid` 用于绘制数据集中变量两两之间的关系图。 网格的对角线通常显示单个变量的分布，而非对角线则显示两个变量之间的关系。

#### `pairplot()`

`pairplot()` 是 `PairGrid` 的一个更简单的高级接口，可以快速创建变量两两关系的散点图矩阵，并在对角线上绘制单变量的分布图（通常是直方图或核密度估计图）。

---

### 风格和颜色管理

Seaborn 的一大优势是其美观的默认样式和灵活的颜色管理功能。

#### 设置图表风格

你可以使用 `sns.set_theme()` 或 `sns.set_style()` 来设置全局的图表主题和风格。Seaborn 提供了几种内置的风格，如 `"darkgrid"`, `"whitegrid"`, `"dark"`, `"white"`, 和 `"ticks"`。

#### 管理颜色面板

Seaborn 拥有丰富多样的颜色面板，可以有效地传达数据信息。
*   **定性调色板 (Qualitative Palettes)**: 用于区分没有内在顺序的类别。
*   **顺序调色板 (Sequential Palettes)**: 用于表示具有顺序关系的数据，通常从浅色到深色渐变。
*   **发散调色板 (Diverging Palettes)**: 用于表示数据相对于某个中间值的变化，通常在两端使用不同的颜色，中间为浅色。

可以使用 `sns.color_palette()` 来创建或查看颜色面板，并使用 `palette` 参数在大多数绘图函数中应用它们。