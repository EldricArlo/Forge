# Pandas实战指南：从入门到精通 (以游戏抽卡数据分析为例)

## 导航目录

- [Pandas实战指南：从入门到精通 (以游戏抽卡数据分析为例)](#pandas实战指南从入门到精通-以游戏抽卡数据分析为例)
  - [导航目录](#导航目录)
  - [1. Pandas 简介](#1-pandas-简介)
  - [2. 核心数据结构：Series 与 DataFrame](#2-核心数据结构series-与-dataframe)
  - [3. 数据的导入与创建](#3-数据的导入与创建)
    - [3.1. 从 Excel 文件导入](#31-从-excel-文件导入)
    - [3.2. 从 CSV 文件导入](#32-从-csv-文件导入)
    - [3.3. 手动创建 DataFrame](#33-手动创建-dataframe)
  - [4. 查看与检验数据](#4-查看与检验数据)
    - [4.1. 预览数据 (`head`, `tail`)](#41-预览数据-head-tail)
    - [4.2. 查看维度、列名与索引 (`shape`, `columns`, `index`)](#42-查看维度列名与索引-shape-columns-index)
    - [4.3. 查看整体信息 (`info`)](#43-查看整体信息-info)
    - [4.4. 获取快速统计摘要 (`describe`)](#44-获取快速统计摘要-describe)
  - [5. 数据的选取与切片](#5-数据的选取与切片)
    - [5.1. 列的选取](#51-列的选取)
    - [5.2. 行的选取与条件筛选](#52-行的选取与条件筛选)
    - [5.3. 使用 `loc` 与 `iloc` 精准定位](#53-使用-loc-与-iloc-精准定位)
  - [6. 数据操作与转换](#6-数据操作与转换)
    - [6.1. 新增与修改列](#61-新增与修改列)
    - [6.3. 应用自定义函数 (`apply`)](#63-应用自定义函数-apply)
  - [7. 常用统计与聚合](#7-常用统计与聚合)
    - [7.1. 常用描述性统计](#71-常用描述性统计)
    - [7.2. 唯一值与计数](#72-唯一值与计数)
  - [8. 分组聚合 (Group By)](#8-分组聚合-group-by)
    - [8.1. 单列分组](#81-单列分组)
    - [8.2. 多列分组](#82-多列分组)
  - [9. 排序](#9-排序)
    - [9.1. 按值排序 (`sort_values`)](#91-按值排序-sort_values)
    - [9.2. 按索引排序 (`sort_index`)](#92-按索引排序-sort_index)
  - [10. 处理缺失数据](#10-处理缺失数据)
    - [10.1. 识别缺失值 (`isnull`, `notnull`)](#101-识别缺失值-isnull-notnull)
    - [10.2. 删除缺失值 (`dropna`)](#102-删除缺失值-dropna)
  - [11. 数据的导出](#11-数据的导出)
  - [12. 高级技巧：链式操作](#12-高级技巧链式操作)

---

## 1. Pandas 简介

Pandas 是 Python 中用于数据处理和分析的强大开源库。它构建在 NumPy 之上，提供了两种核心的数据结构：**Series** 和 **DataFrame**，使得处理表格化数据（如 Excel 表格、SQL 查询结果）变得异常简单和高效。无论是数据清洗、转换、分析还是可视化，Pandas 都是数据科学领域不可或缺的工具。

## 2. 核心数据结构：Series 与 DataFrame

*   **Series**：一个一维的、带标签的数组，可以看作是 DataFrame 中的一列。它由数据和与之关联的索引组成。
*   **DataFrame**：一个二维的、带标签的数据结构，可以看作是一个 Excel 表格或 SQL 表。它有行索引和列索引，是 Pandas 中最常用的数据结构。

可以将 DataFrame 想象成一个由多个 Series 共享相同索引的集合。

## 3. 数据的导入与创建

在开始分析之前，我们首先需要将数据加载到 Pandas 的 DataFrame 中。

### 3.1. 从 Excel 文件导入

这是处理办公数据时最常见的场景。

```python
import pandas as pd
import numpy as np

# 定义 Excel 文件路径
excel_path = 'the file path of excel'

# 读取指定 sheet 的数据
# sheet_name 参数可以指定工作表的名称或索引（从0开始）
datas_of_characters = pd.read_excel(excel_path, sheet_name = '角色活动祈愿')

# 将读取的数据转换为 DataFrame
df = pd.DataFrame(datas_of_characters)
print("从 Excel 文件成功创建 DataFrame!")
```

### 3.2. 从 CSV 文件导入

CSV（逗号分隔值）是另一种非常流行的数据存储格式。

```python
# 假设我们有一个 CSV 文件
csv_path = 'path_to_your_data.csv'

# 从 CSV 读取数据，通常比 Excel 更快
# encoding 参数确保正确解析中文字符
df_from_csv = pd.read_csv(csv_path, encoding='utf-8')
print("从 CSV 文件成功创建 DataFrame!")
```

### 3.3. 手动创建 DataFrame

在某些情况下，您可能需要从 Python 的字典或列表中直接创建 DataFrame，这对于测试或处理少量数据非常方便。

```python
# 使用字典创建 DataFrame，字典的 key 会成为列名
data_dict = {
    '物品名称': ['纳维娅', '神里绫华', '行秋', '香菱'],
    '星级': [5, 5, 4, 4],
    '抽数': [78, 82, 10, 35],
    '祈愿类型': ['角色活动祈愿', '角色活动祈愿', '角色活动祈愿', '常驻祈愿']
}

df_manual = pd.DataFrame(data_dict)
print("手动创建的 DataFrame:\n", df_manual)
```

## 4. 查看与检验数据

加载数据后，第一步通常是快速了解其结构和内容。

### 4.1. 预览数据 (`head`, `tail`)

查看数据的前几行和后几行，以确保数据已正确加载。

```python
# 查看前5行数据 (默认是5行，可以传入数字指定行数)
print("df.head():\n", df.head())
print("\ndf.head(3):\n", df.head(3))

# 查看后5行数据
print("\ndf.tail():\n", df.tail())
```

### 4.2. 查看维度、列名与索引 (`shape`, `columns`, `index`)

获取 DataFrame 的基本元数据。

```python
# 查看 DataFrame 的形状 (行数, 列数)
print(f"DataFrame 的形状: {df.shape}")

# 查看所有列名
print(f"所有列名: {df.columns.tolist()}")

# 查看索引信息
print(f"索引信息: {df.index}")
```

### 4.3. 查看整体信息 (`info`)

`.info()` 方法提供了 DataFrame 的全面概览，包括索引类型、列名、非空值数量和内存使用情况，对于检查缺失值和数据类型特别有用。

```python
# 查看DataFrame整体信息
print("df.info():\n")
df.info()
```

### 4.4. 获取快速统计摘要 (`describe`)

`.describe()` 会自动对所有数值类型的列进行计算，生成一个包含计数、均值、标准差、最小值、四分位数和最大值的统计摘要。

```python
# 对数值列进行统计描述
print("数值列的统计摘要:\n", df.describe())

# 也可以对非数值列进行描述
print("\n对象类型列的统计摘要:\n", df.describe(include=['object']))
```

## 5. 数据的选取与切片

Pandas 提供了灵活且强大的方式来选择和过滤数据。

### 5.1. 列的选取

```python
# 选取单列 (返回一个 Series)
# 这是最标准的方式
star_rating = df['星级']
print("df['星级'] (单列):\n", star_rating.head())
print(f"df['星级'] 的类型: {type(star_rating)}\n")

# 选取单列的另一种方式 (当列名不含空格或特殊字符时)
item_name = df.物品名称
print("df.物品名称 (单列):\n", item_name.head())

# 选取多列 (返回一个新的 DataFrame)
# 注意需要传入一个由列名组成的列表，因此是双重方括号 [[]]
selected_columns = df[['物品名称', '星级', '抽数']]
print("\ndf[['物品名称', '星级', '抽数']] (多列):\n", selected_columns.head())
print(f"df[['物品名称', '星级', '抽数']] 的类型: {type(selected_columns)}\n")
```

### 5.2. 行的选取与条件筛选

```python
# 筛选单个条件的行 (筛选所有5星物品)
five_star_items = df[df['星级'] == 5]
print("筛选所有5星物品:\n", five_star_items.head())

# 筛选多个条件的行 (筛选出所有5星角色)
# 注意：多个条件之间使用 & (与) 或 | (或) 连接，每个条件都要用括号括起来
five_star_characters = df[(df['星级'] == 5) & (df['祈愿类型'] == '角色活动祈愿')]
print("\n筛选所有5星角色:\n", five_star_characters)

# 使用 .isin() 筛选 (筛选出特定名称的物品)
# 适合筛选一列中包含多个特定值的行
specific_items = df[df['物品名称'].isin(['纳维娅', '香菱', '迪卢克'])]
print("\n筛选特定名称的物品:\n", specific_items)

# 使用 ~ 取反条件 (筛选出所有非5星的物品)
non_five_star_items = df[~(df['星级'] == 5)]
print("\n筛选所有非5星的物品:\n", non_five_star_items.head())

# 结合字符串操作筛选 (筛选物品名称中包含 '剑' 的武器)
# .str 属性提供了丰富的字符串方法，na=False 表示在遇到NaN值时不报错
sword_weapons = df[df['物品名称'].str.contains('剑', na=False)]
print("\n筛选物品名称中包含 '剑' 的武器:\n", sword_weapons)
```

### 5.3. 使用 `loc` 与 `iloc` 精准定位

`loc` 和 `iloc` 是更精确、更规范的数据选择方式，可以避免一些潜在的混淆。

> **核心区别**:
> *   `df.loc[行标签, 列标签]`：基于**标签**（label）进行选择。
> *   `df.iloc[行索引, 列索引]`：基于**整数位置**（integer position）进行选择。

```python
# loc 示例：获取索引标签为 1 到 3 的行，以及'物品名称'和'抽数'这两列的数据
subset_loc = df.loc[1:3, ['物品名称', '抽数']]
print("使用 .loc 筛选指定行和列 (标签):\n", subset_loc)

# iloc 示例：获取位置为 0、2、4 的行，以及位置为 1、3 的列的数据
# （即第1、3、5行，第2、4列）
subset_iloc = df.iloc[[0, 2, 4], [1, 3]]
print("\n使用 .iloc 筛选指定行和列 (整数位置):\n", subset_iloc)

# iloc 使用切片：获取前5行，前3列的数据
subset_iloc_slice = df.iloc[0:5, 0:3]
print("\n使用 .iloc 进行切片:\n", subset_iloc_slice)
```

## 6. 数据操作与转换

### 6.1. 新增与修改列

```python
# 新增一列 (例如，根据星级判断是否为限定角色/武器)
# 使用 np.where(条件, 条件为真时的值, 条件为假时的值)
df['是否限定'] = np.where(df['祈愿类型'].isin(['角色活动祈愿', '武器活动祈愿']), '是', '否')
print("新增 '是否限定' 列:\n", df.head())

# 修改现有列的值 (例如，创建一个新的星级展示列)
# 使用 .apply 和 lambda 函数进行更复杂的逻辑判断
df['星级展示'] = df['星级'].apply(lambda x: '金色' if x == 5 else ('紫色' if x == 4 else '蓝色'))
print("\n新增 '星级展示' 列:\n", df[['物品名称', '星级', '星级展示']].head())

# 基于现有列计算新列
# 假设我们有一个'成本'列，值为160
df['原石成本'] = df['抽数'] * 160
print("\n新增 '原石成本' 列:\n", df[['物品名称', '抽数', '原石成本']].head())```

### 6.2. 删除列与行 (`drop`)

```python
# 删除单列 (axis=1 表示按列操作)
# inplace=True 会直接修改原 DataFrame，否则会返回一个修改后的新 DataFrame
df_dropped_col = df.drop('原石成本', axis=1)
print("\n删除 '原石成本' 列后的 DataFrame (前5行):\n", df_dropped_col.head())

# 删除多行 (axis=0 表示按行操作)
df_dropped_rows = df.drop([0, 2, 4], axis=0)
print("\n删除索引为 0, 2, 4 的行后 (前5行):\n", df_dropped_rows.head())
```

### 6.3. 应用自定义函数 (`apply`)

当内置函数无法满足需求时，`apply` 可以将您自定义的函数应用到数据上。

```python
# 对 '抽数' 列应用一个函数，判断是否为大保底 (假设 >=70 为大保底)
df['是否大保底'] = df['抽数'].apply(lambda x: '是' if x >= 70 else '否')
print("新增 '是否大保底' 列 (使用 apply):\n", df[['物品名称', '抽数', '是否大保底']].head())

# 对DataFrame的每一行进行操作 (axis=1)
# 例如：创建一个描述性列，结合星级和物品名称
def create_description(row):
    return f"{row['星级']}星 {row['物品名称']} ({row['祈愿类型']})"

df['描述'] = df.apply(create_description, axis=1)
print("\n新增 '描述' 列 (按行 apply):\n", df[['描述']].head())
```

## 7. 常用统计与聚合

### 7.1. 常用描述性统计

```python
# 计算 '抽数' 列的总和
total_pulls = df['抽数'].sum()
print(f"总抽数: {total_pulls}")

# 计算 '抽数' 列的平均值
avg_pulls = df['抽数'].mean()
print(f"平均抽数: {avg_pulls:.2f}")

# 找到获得5星角色的最高抽数 (最大值)
max_pity = df[df['星级'] == 5]['抽数'].max()
print(f"获得5星的最高抽数: {max_pity}")

# 找到获得5星角色的最低抽数 (最小值)
min_pity = df[df['星级'] == 5]['抽数'].min()
print(f"获得5星的最低抽数: {min_pity}")
```

### 7.2. 唯一值与计数

```python
# 统计每个星级的数量 (非常常用，类似 Excel 的数据透视表计数)
star_counts = df['星级'].value_counts()
print("各星级数量统计:\n", star_counts)

# 统计每种祈愿类型的物品数量
type_counts = df['祈愿类型'].value_counts()
print("\n各祈愿类型物品数量统计:\n", type_counts)

# 统计 '物品名称' 列的唯一值数量
unique_items_count = df['物品名称'].nunique()
print(f"\n不重复的物品名称数量: {unique_items_count}")

# 获取 '物品名称' 列的所有唯一值
unique_items = df['物品名称'].unique()
print("\n所有不重复的物品名称:\n", unique_items)
```

## 8. 分组聚合 (Group By)

`groupby` 操作是数据分析的核心，它遵循“分割-应用-合并”（Split-Apply-Combine）的模式。

### 8.1. 单列分组

```python
# 按照 '祈愿类型' 分组，计算每种类型的总抽数
pulls_by_type = df.groupby('祈愿类型')['抽数'].sum()
print("按祈愿类型统计总抽数:\n", pulls_by_type)

# 按照 '星级' 分组，计算每个星级的平均抽数
avg_pulls_by_star = df.groupby('星级')['抽数'].mean()
print("\n按星级统计平均抽数:\n", avg_pulls_by_star)
```

### 8.2. 多列分组

```python
# 按照 '祈愿类型' 和 '星级' 双重分组，统计每个组合的数量
# .size() 会返回每个组的行数
count_by_type_star = df.groupby(['祈愿类型', '星级']).size()
# 使用 .reset_index() 将分组结果转换为 DataFrame
count_df = count_by_type_star.reset_index(name='数量')
print("按祈愿类型和星级统计数量:\n", count_df)```

### 8.3. 使用 `agg` 进行多重聚合

当需要对分组后的数据应用多个不同的聚合函数时，`.agg()` 非常有用。

```python
# 按祈愿类型统计总抽数、平均抽数和记录数
agg_by_type = df.groupby('祈愿类型')['抽数'].agg(['sum', 'mean', 'count'])
print("按祈愿类型进行多重聚合:\n", agg_by_type)

# 对不同列应用不同聚合函数
agg_multiple_columns = df.groupby('祈愿类型').agg(
    总抽数=('抽数', 'sum'),
    平均抽数=('抽数', 'mean'),
    获取物品数=('物品名称', 'count')
)
print("\n对不同列应用不同聚合函数:\n", agg_multiple_columns)
```

## 9. 排序

### 9.1. 按值排序 (`sort_values`)

```python
# 按照 '抽数' 列降序排序 (从大到小)
sorted_df_pity = df.sort_values(by='抽数', ascending=False)
print("按抽数降序排序:\n", sorted_df_pity.head())

# 按照 '星级' 降序，然后按照 '获取时间' 升序排序
# 假设存在 '获取时间' 列
# sorted_df_multi = df.sort_values(by=['星级', '获取时间'], ascending=[False, True])
# print("\n按星级降序、获取时间升序排序:\n", sorted_df_multi.head())
```

### 9.2. 按索引排序 (`sort_index`)

```python
# 假设索引被打乱，我们可以按索引升序排序
df_shuffled = df.sample(frac=1) # frac=1 表示随机抽取所有行，打乱顺序
print("\n打乱后的 DataFrame:\n", df_shuffled.head())
df_sorted_index = df_shuffled.sort_index(ascending=True)
print("\n按索引排序后的 DataFrame:\n", df_sorted_index.head())
```

## 10. 处理缺失数据

真实世界的数据往往是不完美的，Pandas 提供了强大的工具来处理缺失值（通常表示为 `NaN`）。

### 10.1. 识别缺失值 (`isnull`, `notnull`)

```python
# 为了演示，手动制造一些缺失值
df_with_nan = df.copy()
df_with_nan.loc[2, '物品名称'] = np.nan # 将第3行的物品名称设为缺失
df_with_nan.loc[5, '抽数'] = np.nan   # 将第6行的抽数设为缺失

# 检查每列的缺失值数量
print("每列缺失值数量:\n", df_with_nan.isnull().sum())

# 检查是否存在任何缺失值
print(f"\n是否存在任何缺失值: {df_with_nan.isnull().values.any()}")
```

### 10.2. 删除缺失值 (`dropna`)

```python
# 删除含有任何缺失值的行 (默认行为)
df_dropna_rows = df_with_nan.dropna()
print("\n删除含有缺失值的行后:\n", df_dropna_rows)

# 删除所有值都为缺失的列 (不常见但有用)
df_dropna_cols = df_with_nan.dropna(axis=1, how='all')```

### 10.3. 填充缺失值 (`fillna`)

删除数据有时会造成信息损失，填充是更常用的策略。

```python
# 将缺失的 '抽数' 用该列的平均值填充
mean_pity = df_with_nan['抽数'].mean()
df_filled_pity = df_with_nan['抽数'].fillna(value=mean_pity)
print("\n用平均值填充 '抽数' 缺失值:\n", df_filled_pity.head(7))

# 填充字符串类型的缺失值 (例如，填充为 '未知物品')
df_filled_item = df_with_nan.fillna({'物品名称': '未知物品', '抽数': 0})
print("\n分类填充 '物品名称' 和 '抽数' 缺失值:\n", df_filled_item.head(7))

# 使用前一个有效值填充 (Forward-fill)
df_ffill = df_with_nan.fillna(method='ffill')
print("\n使用前向填充:\n", df_ffill.head(7))
```

## 11. 数据的导出

在完成数据处理和分析后，您可能希望将结果保存到文件中。

```python
# 导出为CSV文件
# index=False 表示不将 DataFrame 的索引列写入文件
# encoding='utf-8-sig' 可以更好地处理中文字符，尤其是在Windows上的Excel中打开
# df.to_csv('processed_genshin_data.csv', index=False, encoding='utf-8-sig')
# print("数据已导出到 'processed_genshin_data.csv'")

# 导出为Excel文件 (需要安装 openpyxl: pip install openpyxl)
# df.to_excel('processed_genshin_data.xlsx', index=False, sheet_name='分析结果')
# print("数据已导出到 'processed_genshin_data.xlsx'")
```

## 12. 高级技巧：链式操作

许多 Pandas 操作都会返回一个新的 DataFrame，您可以将这些操作像链条一样连接起来，使代码更简洁、更易读。

```python
# 示例链式操作：
# 1. 筛选出5星角色活动祈愿的记录
# 2. 按抽数降序排序
# 3. 创建一个新的'成本'列
# 4. 最后只显示'物品名称', '抽数', '成本'这三列
# 5. 并重置索引

result = (
    df[(df['星级'] == 5) & (df['祈愿类型'] == '角色活动祈愿')]
    .sort_values(by='抽数', ascending=False)
    .assign(成本=lambda x: x['抽数'] * 160)
    [['物品名称', '抽数', '成本']]
    .reset_index(drop=True)
)

print("链式操作最终结果:\n", result)
```