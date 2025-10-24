# Areas_Of_Knowledge\Programming_Languages\Python\libraries\Pandas_Guide.md

## Pandas 库语法详解

欢迎来到 Pandas 的世界！Pandas 是一个功能强大的 Python 数据分析库，它提供了易于使用的数据结构和数据分析工具，使数据清洗、处理、分析和可视化变得更加简单高效。本指南将为您详细介绍 Pandas 的核心语法和常用功能，并提供一个支持跳转的完整目录，帮助您快速定位和学习所需内容。

### 目录

- [Areas\_Of\_Knowledge\\Programming\_Languages\\Python\\libraries\\Pandas\_Guide.md](#areas_of_knowledgeprogramming_languagespythonlibrariespandas_guidemd)
  - [Pandas 库语法详解](#pandas-库语法详解)
    - [目录](#目录)
    - [1. Pandas 核心数据结构](#1-pandas-核心数据结构)
      - [1.1 Series：一维带标签数组](#11-series一维带标签数组)
      - [1.2 DataFrame：二维带标签表格](#12-dataframe二维带标签表格)
    - [2. 数据的索引与选择](#2-数据的索引与选择)
      - [2.1 基础索引](#21-基础索引)
      - [2.2 基于标签的索引：.loc](#22-基于标签的索引loc)
      - [2.3 基于位置的索引：.iloc](#23-基于位置的索引iloc)
      - [2.4 条件索引](#24-条件索引)
    - [3. 数据操作](#3-数据操作)
      - [3.1 数据的增删改查](#31-数据的增删改查)
      - [3.2 缺失值处理](#32-缺失值处理)
      - [3.3 重复值处理](#33-重复值处理)
      - [3.4 数据类型转换](#34-数据类型转换)
    - [4. 数据的合并与连接](#4-数据的合并与连接)
      - [4.1 concat：沿轴拼接](#41-concat沿轴拼接)
      - [4.2 merge：按键连接](#42-merge按键连接)
      - [4.3 join：按索引连接](#43-join按索引连接)
    - [5. 分组与聚合](#5-分组与聚合)
      - [5.1 GroupBy 分组](#51-groupby-分组)
      - [5.2 聚合函数](#52-聚合函数)
      - [5.3 transform 和 apply](#53-transform-和-apply)
    - [6. 时间序列分析](#6-时间序列分析)
      - [6.1 时间序列的创建](#61-时间序列的创建)
      - [6.2 时间序列的索引与切片](#62-时间序列的索引与切片)
      - [6.3 重采样](#63-重采样)
    - [7. 数据可视化](#7-数据可视化)
      - [7.1 折线图](#71-折线图)
      - [7.2 柱状图](#72-柱状图)
      - [7.3 散点图](#73-散点图)
    - [8. 性能优化](#8-性能优化)
      - [8.1 使用合适的的数据类型](#81-使用合适的的数据类型)
      - [8.2 向量化操作](#82-向量化操作)
      - [8.3 避免不必要的数据复制](#83-避免不必要的数据复制)

---

### 1. Pandas 核心数据结构

Pandas 的核心是其两种主要的数据结构：**Series** 和 **DataFrame**。

#### 1.1 Series：一维带标签数组

Series 是一个类似于一维数组的对象，它由一组数据以及一组与之相关的数据标签（即索引）组成。

*   **创建 Series**
    ```python
    import pandas as pd
    import numpy as np

    # 从列表创建
    s = pd.Series([1, 3, 5, np.nan, 6, 8])

    # 从字典创建，字典的键会成为索引
    d = {'a': 1, 'b': 2, 'c': 3}
    s_from_dict = pd.Series(d)
    ```

*   **Series 的属性**
    *   `s.index`: 获取 Series 的索引。
    *   `s.values`: 获取 Series 的值。
    *   `s.dtype`: 获取 Series 中数据的类型。

#### 1.2 DataFrame：二维带标签表格

DataFrame 是一个表格型的数据结构，它含有一组有序的列，每列可以是不同的值类型（数值、字符串、布尔值等）。 DataFrame 既有行索引也有列索引，可以被看作是由 Series 组成的字典。

*   **创建 DataFrame**
    ```python
    # 从字典创建，字典的键是列名
    data = {'state': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada', 'Nevada'],
            'year': [2000, 2001, 2002, 2001, 2002, 2003],
            'pop': [1.5, 1.7, 3.6, 2.4, 2.9, 3.2]}
    df = pd.DataFrame(data)
    ```

*   **DataFrame 的常用属性和方法**
    *   `df.head()`: 查看前几行数据。
    *   `df.tail()`: 查看后几行数据。
    *   `df.index`: 获取行索引。
    *   `df.columns`: 获取列索引。
    *   `df.describe()`: 生成描述性统计数据。
    *   `df.T`: 转置数据。
    *   `df.sort_values(by='column_name')`: 按列的值进行排序。

---

### 2. 数据的索引与选择

Pandas 提供了多种方式来对数据进行索引和选择。

#### 2.1 基础索引

*   **选择列**: 可以使用 `[]` 来选择 DataFrame 的一列或多列。
    ```python
    df['state']  # 选择 'state' 列，返回一个 Series
    df[['state', 'year']]  # 选择 'state' 和 'year' 列，返回一个 DataFrame
    ```

*   **选择行**: 可以使用切片来选择行。
    ```python
    df[0:3]  # 选择前 3 行
    ```

#### 2.2 基于标签的索引：.loc

`.loc` 主要基于标签进行选择，但也可以与布尔数组一起使用。

```python
df.loc[0]  # 选择索引为 0 的行
df.loc[:, ['state', 'year']]  # 选择所有行的 'state' 和 'year' 列
df.loc[df['state'] == 'Ohio'] # 选择 'state' 列值为 'Ohio' 的行
```

#### 2.3 基于位置的索引：.iloc

`.iloc` 主要基于整数位置进行选择（从 0 到 length-1）。

```python
df.iloc[3]  # 选择第 4 行
df.iloc[3:5, 0:2]  # 选择第 4、5 行和第 1、2 列
df.iloc[[1, 2, 4], [0, 2]]  # 选择第 2、3、5 行和第 1、3 列
```

#### 2.4 条件索引

可以使用布尔数组来进行条件索引。

```python
df[df['pop'] > 2.5]  # 选择 'pop' 列值大于 2.5 的所有行
```

---

### 3. 数据操作

#### 3.1 数据的增删改查

*   **增加列**
    ```python
    df['new_col'] = df['pop'] * 2
    ```
*   **删除列**
    ```python
    df.drop('new_col', axis=1, inplace=True) # axis=1 表示删除列，inplace=True 表示在原 DataFrame 上修改
    ```
*   **修改数据**
    ```python
    df.loc[0, 'pop'] = 1.6 # 修改指定位置的数据
    ```

#### 3.2 缺失值处理

在 Pandas 中，缺失值通常用 `NaN` (Not a Number) 表示。

*   **检查缺失值**: `df.isnull()` 或 `df.isna()`
*   **删除含有缺失值的行或列**: `df.dropna()`
    ```python
    df.dropna() # 删除任何含有缺失值的行
    df.dropna(how='all') # 只删除所有值都为缺失值的行
    ```
*   **填充缺失值**: `df.fillna()`
    ```python
    df.fillna(value=0) # 将所有缺失值填充为 0
    ```

#### 3.3 重复值处理

*   **检查重复值**: `df.duplicated()`
*   **删除重复值**: `df.drop_duplicates()`

#### 3.4 数据类型转换

可以使用 `astype()` 方法来转换数据类型。

```python
df['year'] = df['year'].astype(str)
```

---

### 4. 数据的合并与连接

Pandas 提供了多种方法来合并和连接数据。

#### 4.1 concat：沿轴拼接

`pd.concat()` 函数可以沿着一个轴将多个对象堆叠到一起。

```python
df1 = pd.DataFrame({'A': ['A0', 'A1'], 'B': ['B0', 'B1']})
df2 = pd.DataFrame({'A': ['A2', 'A3'], 'B': ['B2', 'B3']})
pd.concat([df1, df2]) # 默认按行拼接 (axis=0)
```

#### 4.2 merge：按键连接

`pd.merge()` 类似于关系型数据库的连接操作，可以根据一个或多个键将不同的 DataFrame 连接起来。

```python
left = pd.DataFrame({'key': ['K0', 'K1'], 'A': ['A0', 'A1']})
right = pd.DataFrame({'key': ['K0', 'K1'], 'B': ['B0', 'B1']})
pd.merge(left, right, on='key')
```
`merge` 支持多种连接方式，如内连接（`inner`）、左连接（`left`）、右连接（`right`）和外连接（`outer`）。

#### 4.3 join：按索引连接

`df.join()` 方法主要用于基于索引的合并。

```python
left = pd.DataFrame({'A': ['A0', 'A1']}, index=['K0', 'K1'])
right = pd.DataFrame({'B': ['B0', 'B1']}, index=['K0', 'K1'])
left.join(right)
```

---

### 5. 分组与聚合

分组聚合是数据分析中的一个重要环节，通常指的是将数据根据某些标准拆分成组，然后对每个组独立地应用一个函数，最后将结果组合成一个数据结构。

#### 5.1 GroupBy 分组

`groupby()` 方法用于对数据进行分组。

```python
df.groupby('state')
```
这会创建一个 `DataFrameGroupBy` 对象，可以对其进行后续操作。

#### 5.2 聚合函数

可以对分组后的数据应用聚合函数，如 `sum()`, `mean()`, `count()`, `agg()` 等。

```python
df.groupby('state')['pop'].mean() # 按 'state' 分组，计算 'pop' 列的平均值
df.groupby('state').agg({'pop': 'sum', 'year': 'max'}) # 对不同列应用不同的聚合函数
```

#### 5.3 transform 和 apply

*   `transform`: 对每个分组执行某些计算，并返回一个与原数据形状相同的对象。
*   `apply`: 可以应用更复杂的函数，对每个分组执行操作。

---

### 6. 时间序列分析

Pandas 提供了强大的时间序列处理功能。

#### 6.1 时间序列的创建

可以使用 `pd.to_datetime()` 将字符串或数字转换为时间戳。

```python
dates = pd.to_datetime(['2023-01-01', '2023-01-02', '2023-01-03'])
ts = pd.Series(np.random.randn(3), index=dates)
```

#### 6.2 时间序列的索引与切片

可以使用时间字符串进行索引和切片。

```python
ts['2023-01-02']
ts['2023-01-01':'2023-01-02']
```

#### 6.3 重采样

`resample()` 方法用于对时间序列数据进行频率转换。

```python
# 将数据重采样为每月一次，并计算均值
ts.resample('M').mean()
```

---

### 7. 数据可视化

Pandas 提供了与 Matplotlib 和 Seaborn 等可视化库的集成，使得数据的可视化变得简单而高效。

#### 7.1 折线图

折线图通常用于展示数据随时间的变化趋势。
```python
ts.cumsum().plot()
```

#### 7.2 柱状图

柱状图适合比较类别间的差异。
```python
df.groupby('state')['pop'].sum().plot(kind='bar')
```

#### 7.3 散点图

散点图用于展示两个数值变量之间的关系。
```python
df.plot.scatter(x='year', y='pop')
```

---

### 8. 性能优化

对于大规模数据集，优化 Pandas 的性能至关重要。

#### 8.1 使用合适的的数据类型

合理选择数据类型可以显著减少内存占用和加速计算。

#### 8.2 向量化操作

尽量使用 Pandas 内置的向量化操作，避免使用 Python 的原生循环，这样可以利用底层的优化进行快速计算。

#### 8.3 避免不必要的数据复制

在 Pandas 中，很多操作会返回新的对象而不是修改原始对象。 为了避免不必要的数据复制，可以使用 `.loc[]`, `.iloc[]` 等进行原地修改，或者通过设置 `inplace=True` 参数来直接修改原始数据。

希望这份详细的 Pandas 语法指南能帮助您更好地掌握这个强大的数据分析工具！