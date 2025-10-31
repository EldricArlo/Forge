# Python数据清洗核心指南：从原始数据到“可用”数据的必经之路

在数据分析和机器学习的流程中，数据清洗是至关重要且无可替代的第一步。常言道“垃圾进，垃圾出”（Garbage In, Garbage Out），原始数据中普遍存在的缺失值、重复值、异常值和格式不一致等问题，会严重影响后续分析的准确性和模型的有效性。

以下是使用Python中的**Pandas**库进行数据清洗的六个核心步骤，我们将通过一个贯穿始终的示例数据集来演示。

---

## 目录

- [Python数据清洗核心指南：从原始数据到“可用”数据的必经之路](#python数据清洗核心指南从原始数据到可用数据的必经之路)
  - [目录](#目录)
  - [准备工作：创建一个包含各种问题的示例数据](#准备工作创建一个包含各种问题的示例数据)
  - [1. 处理缺失值 (Missing Values)](#1-处理缺失值-missing-values)
    - [识别缺失值](#识别缺失值)
    - [处理策略：删除](#处理策略删除)
    - [处理策略：填充 (Imputation)](#处理策略填充-imputation)
  - [2. 处理重复值 (Duplicates)](#2-处理重复值-duplicates)
    - [识别重复值](#识别重复值)
    - [处理策略：删除重复项](#处理策略删除重复项)
  - [3. 处理异常值/离群点 (Outliers)](#3-处理异常值离群点-outliers)
    - [识别异常值](#识别异常值)
    - [处理策略：替换/盖帽](#处理策略替换盖帽)
  - [4. 数据类型转换与格式标准化](#4-数据类型转换与格式标准化)
    - [数据类型转换](#数据类型转换)
    - [字符串操作与标准化](#字符串操作与标准化)
  - [5. 文本/分类数据编码](#5-文本分类数据编码)
    - [方法一：映射 (Mapping)](#方法一映射-mapping)
    - [方法二：独热编码 (One-Hot Encoding)](#方法二独热编码-one-hot-encoding)
  - [6. 分箱/离散化 (Binning)](#6-分箱离散化-binning)

---

## 准备工作：创建一个包含各种问题的示例数据

首先，我们导入必要的库，并创建一个名为 `df` 的Pandas DataFrame，它模拟了真实世界中可能遇到的各种数据问题。

```python
import pandas as pd
import numpy as np
# 用于可视化，仅用于演示异常值识别，这里先导入
import seaborn as sns 
import matplotlib.pyplot as plt 

# 创建一个包含各种问题的示例数据集
data = {
    '姓名': ['张三', '李四', '王五', '赵六', '钱七', '张三', '孙八'],
    '年龄': [28, 32, 45, np.nan, 29, 300, 28], # 包含NaN和异常值 300
    '城市': ['北京', '上海', '广州', '深圳', '深圳', '成都', '北京 '], # 包含末尾空格
    '薪资': [50000, 62000, 48000, None, 53000, 45000, 50000], # 包含缺失值 (None 和 np.nan)
    '入职日期': ['2021-01-15', '2019-03-01', 'invalid_date', '2020-11-20', '2018-07-07', '2022-12-01', '2021-01-15'],
    '部门': ['技术部', '销售部', '技术部', '市场部', '销售部', '人力资源部', '技术部']
}
df = pd.DataFrame(data)
print("--- 原始数据展示 ---")
print(df)
print("\n--- 原始数据信息 ---")
df.info()
```

## 1. 处理缺失值 (Missing Values)

缺失值是数据中最常见的问题。处理它们的第一步是识别其存在和分布。

### 识别缺失值

```python
# 识别方法 1: 检查每列的缺失值数量
print("\n--- 每列缺失值统计 ---")
print(df.isnull().sum()) 

# 识别方法 2: 查看DataFrame的整体信息 (非空值计数)
# df.info() # 已在准备工作部分展示
```

### 处理策略：删除

如果缺失值所在行的信息不重要，或缺失比例过高，可以选择直接删除。

```python
df_cleaned_drop = df.copy()

# 示例 1.1: 删除任何包含缺失值的行 (至少有一个NaN/None)
# df_cleaned_drop.dropna(inplace=True) 

# 示例 1.2: 仅删除所有值都为缺失的行 (how='all')
# df_cleaned_drop.dropna(how='all', inplace=True) 

# 示例 1.3: 仅删除'薪资'列中缺失的行
df_cleaned_drop_salary = df_cleaned_drop.dropna(subset=['薪资'])
print("\n--- 删除'薪资'缺失值后的数据 ---")
print(df_cleaned_drop_salary)
```

### 处理策略：填充 (Imputation)

当数据非常宝贵时，填充是更好的选择。

```python
df_cleaned_fill = df.copy()

# 示例 1.4: 用固定值（如 '未知'）填充分类变量 '城市' 中的缺失值 (虽然示例中城市没有缺失，但演示方法)
df_cleaned_fill['城市'].fillna('未知', inplace=True)

# 示例 1.5: 用均值/中位数填充数值型数据 '年龄'
# 注意：在填充前，最好先处理异常值，因为异常值会影响均值和中位数。
median_age = df_cleaned_fill['年龄'].median()
df_cleaned_fill['年龄'] = df_cleaned_fill['年龄'].fillna(median_age)
print(f"\n--- '年龄' 中位数 ({median_age}) 填充后的数据 ---")
print(df_cleaned_fill)

# 示例 1.6: 用前一个值填充 ('ffill' = forward fill)
df_cleaned_fill['薪资'].fillna(method='ffill', inplace=True)
print("\n--- '薪资' FFill 填充后的数据 ---")
print(df_cleaned_fill)
```

## 2. 处理重复值 (Duplicates)

重复数据会使分析结果产生偏差，例如夸大某些类别的计数。

### 识别重复值

```python
# 示例 2.1: 检查存在多少完全重复的行
print("\n--- 完全重复行计数 ---")
print(df.duplicated().sum()) 
# 结果为 1 (最后一行 '孙八' 与第一行 '张三' 数据几乎相同，但有一行完全相同的 '张三' 记录)
```

### 处理策略：删除重复项

```python
# 注意：我们先处理了缺失值，但为了演示，我们使用原始 df 的一个副本
df_cleaned_dup = df.copy() 

# 示例 2.2: 删除所有完全重复的行 (默认保留第一次出现的行)
df_all_unique = df_cleaned_dup.drop_duplicates()
print("\n--- 删除所有完全重复行后的数据 ---")
print(df_all_unique)

# 示例 2.3: 基于特定列组合来判断和删除重复项
# 如果 '姓名' 和 '入职日期' 都相同，则认为是重复记录 (保留第一次出现的)
df_subset_unique = df_cleaned_dup.drop_duplicates(subset=['姓名', '入职日期'], keep='first')
print("\n--- 基于'姓名'和'入职日期'删除重复项后的数据 ---")
print(df_subset_unique) 
```

## 3. 处理异常值/离群点 (Outliers)

异常值是指那些与数据集中其他观测值显著不同的数据点，例如示例中的年龄 `300`。

### 识别异常值

```python
# 示例 3.1: 描述性统计
print("\n--- '年龄'列描述性统计 ---")
print(df['年龄'].describe()) 
# max 300 明显异常

# 示例 3.2: 可视化 (箱线图)
plt.figure(figsize=(6, 4))
sns.boxplot(x=df['年龄'].dropna()) # 剔除 NaN 后绘图
plt.title("年龄分布箱线图")
# plt.show() # 在实际环境中运行

# 示例 3.3: 使用 IQR (Interquartile Range) 方法量化识别
Q1 = df['年龄'].quantile(0.25)
Q3 = df['年龄'].quantile(0.75)
IQR = Q3 - Q1
lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR

print(f"\nQ1: {Q1}, Q3: {Q3}, IQR: {IQR}")
print(f"IQR 上界 (离群点阈值): {upper_bound}")

outliers = df[(df['年龄'] < lower_bound) | (df['年龄'] > upper_bound)]
print("\n--- 识别到的 IQR 异常值 ---")
print(outliers) # 记录 5 (年龄 300) 被识别
```

### 处理策略：替换/盖帽

我们可以将异常值替换为 `NaN`（以便后续用中位数等填充），或者用上下限进行“盖帽”（Capping）。

```python
df_cleaned_outlier = df.copy()

# 示例 3.4: 替换为 NaN
df_cleaned_outlier.loc[df_cleaned_outlier['年龄'] > upper_bound, '年龄'] = np.nan
print("\n--- 异常值替换为 NaN 后的数据 ---")
print(df_cleaned_outlier)

# 示例 3.5: 用上界进行盖帽 (Capping)
# df_cleaned_outlier.loc[df_cleaned_outlier['年龄'] > upper_bound, '年龄'] = upper_bound
```

## 4. 数据类型转换与格式标准化

确保每一列的数据类型正确，并且格式统一，是进行计算和分析的基础。

### 数据类型转换

我们使用前面经过异常值处理（替换为NaN）和缺失值填充（ffill）的 DataFrame 副本进行操作。

```python
df_final = df_cleaned_outlier.copy()
df_final['薪资'].fillna(method='ffill', inplace=True) # 再次填充薪资

# 示例 4.1: 将'薪资'转换为整数类型
# 注意：Pandas 默认不能将包含 NaN 的列转换为 int 类型，但 'Int64' 可以处理 NaN
df_final['薪资'] = df_final['薪资'].astype('Int64')
print("\n--- '薪资' 类型转换为 Int64 ---")
df_final.info()

# 示例 4.2: 将'入职日期'转换为日期时间类型
# errors='coerce': 将无法转换的字符串（'invalid_date'）变为 NaT（Not a Time）
df_final['入职日期'] = pd.to_datetime(df_final['入职日期'], errors='coerce')
print("\n--- '入职日期' 类型转换后的数据 ---")
print(df_final[['入职日期']].head(5))
df_final.info()
```

### 字符串操作与标准化

```python
# 示例 4.3: 去除'城市'列中的前后空格 (如 '北京 ')
df_final['城市'] = df_final['城市'].str.strip()
print(f"\n城市列去空格后的唯一值: {df_final['城市'].unique()}")

# 示例 4.4: 内容替换/统一表达方式 (将'人力资源部'替换为'HR部')
df_final['部门'] = df_final['部门'].str.replace('人力资源部', 'HR部')
print(f"部门列替换后的唯一值: {df_final['部门'].unique()}")

# 示例 4.5: 大小写统一 (如果存在英文数据，例如将 'hr部' 统一为 'HR部')
# df_final['部门'] = df_final['部门'].str.upper() 
```

## 5. 文本/分类数据编码

机器学习模型无法直接处理文本数据，需要将其转换为数值形式。

### 方法一：映射 (Mapping)

适用于有序或分类较少的类别。

```python
# 示例 5.1: 对部门进行数值映射
department_mapping = {'技术部': 1, '销售部': 2, '市场部': 3, 'HR部': 4}
df_final['部门编码'] = df_final['部门'].map(department_mapping)
print("\n--- 部门编码 (Mapping) 结果 ---")
print(df_final[['部门', '部门编码']])
```

### 方法二：独热编码 (One-Hot Encoding)

适用于无序的类别（如“城市”），避免模型错误地认为类别间存在顺序关系。

```python
# 示例 5.2: 对'城市'列进行独热编码
df_dummies = pd.get_dummies(df_final, columns=['城市'], prefix='城市')
print("\n--- 城市独热编码 (One-Hot Encoding) 结果 ---")
print(df_dummies.filter(like='城市_'))
```

## 6. 分箱/离散化 (Binning)

分箱是将连续的数值型变量（如年龄）划分为几个离散的区间（如青年、中年、老年），有助于降低数据噪声。

```python
# 示例 6.1: 对'年龄'列进行分箱
# 先用中位数重新填充之前被替换为 NaN 的年龄
df_final['年龄'].fillna(df_final['年龄'].median(), inplace=True)

# 定义区间边界和标签
bins = [0, 30, 45, df_final['年龄'].max() + 1] # 确保包含最大值
labels = ['青年(<=30)', '中年(31-45)', '老年(>45)']

# pd.cut() 函数进行分箱
df_final['年龄阶段'] = pd.cut(
    df_final['年龄'], 
    bins=bins, 
    labels=labels, 
    right=True, # 默认右侧闭合 (e.g., (30, 45])
    include_lowest=True # 包含最低值
)

print("\n--- 年龄分箱 (Binning) 结果 ---")
print(df_final[['年龄', '年龄阶段']])
print(f"年龄阶段分布:\n{df_final['年龄阶段'].value_counts()}")
```
