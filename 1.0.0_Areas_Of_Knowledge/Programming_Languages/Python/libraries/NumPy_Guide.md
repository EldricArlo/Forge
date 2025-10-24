# Areas_Of_Knowledge\Programming_Languages\Python\libraries\NumPy_Guide.md

## NumPy 语法宝典：一份深入、清晰的完全指南

NumPy（Numerical Python 的简称）是 Python 语言的一个扩展程序库，对于进行科学计算至关重要。它提供了一个高性能的多维数组对象 `ndarray`，以及用于处理这些数组的大量函数。 本文将为您提供一份详尽且深刻的 NumPy 语法指南，配有完整且支持跳转的目录，旨在帮助您全面掌握 NumPy 的核心功能。

### 目录

1.  **[NumPy 基础](#numpy-基础)**
    *   [安装 NumPy](#安装-numpy)
    *   [导入 NumPy](#导入-numpy)
    *   [NumPy ndarray 对象](#numpy-ndarray-对象)

2.  **[创建 NumPy 数组](#创建-numpy-数组)**
    *   [从 Python 列表或元组创建](#从-python-列表或元组创建)
    *   [使用内置函数创建数组](#使用内置函数创建数组)
        *   [`numpy.zeros()`](#numpyzeros)
        *   [`numpy.ones()`](#numpyones)
        *   [`numpy.arange()`](#numpyarange)
        *   [`numpy.linspace()`](#numpylinspace)
        *   [`numpy.random.rand()`](#numpyrandomrand)
        *   [`numpy.empty()`](#numpyempty)
        *   [`numpy.eye()`](#numpyeye)

3.  **[NumPy 数据类型](#numpy-数据类型)**
    *   [常见数据类型](#常见数据类型)
    *   [查看数组的数据类型](#查看数组的数据类型)
    *   [指定和转换数据类型](#指定和转换数据类型)

4.  **[数组索引与切片](#数组索引与切片)**
    *   [一维数组索引与切片](#一维数组索引与切片)
    *   [多维数组索引与切片](#多维数组索引与切片)
    *   [布尔索引](#布尔索引)
    *   [花式索引](#花式索引)

5.  **[数组操作与广播](#数组操作与广播)**
    *   [数组形状操作](#数组形状操作)
    *   [数组的连接与分割](#数组的连接与分割)
    *   [广播机制](#广播机制)

6.  **[通用函数 (ufunc)](#通用函数-ufunc)**
    *   [什么是通用函数？](#什么是通用函数)
    *   [元素级运算](#元素级运算)
    *   [数学函数](#数学函数)
    *   [聚合函数](#聚合函数)

7.  **[线性代数](#线性代数)**
    *   [矩阵乘法](#矩阵乘法)
    *   [行列式](#行列式)
    *   [矩阵的逆](#矩阵的逆)
    *   [求解线性方程组](#求解线性方程组)
    *   [特征值与特征向量](#特征值与特征向量)
    *   [奇异值分解 (SVD)](#奇异值分解-svd)

8.  **[随机数生成](#随机数生成)**
    *   [基础随机数](#基础随机数)
    *   [从特定分布中抽样](#从特定分布中抽样)
    *   [随机排列](#随机排列)
    *   [可复现的随机性：随机数种子](#可复现的随机性随机数种子)

---

### NumPy 基础

#### 安装 NumPy

在开始使用 NumPy 之前，您需要先安装它。通过 Python 的包管理器 pip 可以轻松完成安装：

```bash
pip install numpy
```

#### 导入 NumPy

按照惯例，我们通常将 NumPy 导入并简写为 `np`：

```python
import numpy as np
```

#### NumPy ndarray 对象

NumPy 的核心是其 `ndarray`（n-dimensional array）对象，它是一个多维的、同质的数组。 这意味着数组中的所有元素都必须是相同的数据类型。`ndarray` 对象具有以下重要属性：

*   **`ndim`**: 数组的维度数量。
*   **`shape`**: 一个元组，表示数组在每个维度上的大小。
*   **`size`**: 数组中元素的总数。
*   **`dtype`**: 描述数组中元素类型的数据类型对象。
*   **`itemsize`**: 数组中每个元素所占用的字节大小。
*   **`data`**: 包含数组实际元素的缓冲区。

---

### 创建 NumPy 数组

有多种方法可以创建 NumPy 数组。

#### 从 Python 列表或元组创建

最直接的方式是使用 `numpy.array()` 函数将 Python 的列表或元组转换为 `ndarray`。

```python
# 创建一维数组
a = np.array([1, 2, 3, 4, 5])

# 创建二维数组
b = np.array([[1, 2, 3], [4, 5, 6]])
```

#### 使用内置函数创建数组

NumPy 提供了多个函数来创建具有特定初始值的数组。

##### `numpy.zeros()`

创建一个用 0 填充的数组。

```python
zeros_array = np.zeros((3, 4)) # 创建一个 3x4 的二维数组，所有元素为 0
```

##### `numpy.ones()`

创建一个用 1 填充的数组。

```python
ones_array = np.ones((2, 3, 4), dtype=np.int16) # 创建一个 2x3x4 的三维数组，所有元素为 1，数据类型为 16 位整数
```

##### `numpy.arange()`

类似于 Python 的 `range()` 函数，但在 NumPy 中返回一个 `ndarray`。

```python
range_array = np.arange(10, 30, 5) # 创建一个从 10 开始，到 30 结束（不包含），步长为 5 的数组
```

##### `numpy.linspace()`

创建一个包含等间隔数值的数组。

```python
linspace_array = np.linspace(0, 2, 9) # 创建一个从 0 到 2 之间，包含 9 个等间隔元素的数组
```

##### `numpy.random.rand()`

创建一个给定形状的数组，并用[0, 1)之间的随机样本填充。

```python
random_array = np.random.rand(2, 2)
```

##### `numpy.empty()`

创建一个内容随机且未初始化的数组。它的值是创建时内存中存在的值。

```python
empty_array = np.empty((2, 3))
```

##### `numpy.eye()`

创建一个单位矩阵（主对角线为 1，其余为 0 的方阵）。

```python
identity_matrix = np.eye(3) # 创建一个 3x3 的单位矩阵
```

---

### NumPy 数据类型

NumPy 提供了比 Python 更多的数值类型。

#### 常见数据类型

*   **`int8`, `int16`, `int32`, `int64`**: 不同位数的有符号整数。
*   **`uint8`, `uint16`, `uint32`, `uint64`**: 不同位数的无符号整数。
*   **`float16`, `float32`, `float64`**: 不同精度的浮点数。
*   **`complex64`, `complex128`**: 不同精度的复数。
*   **`bool`**: 布尔类型，存储 `True` 或 `False`。
*   **`string_`**: 固定长度的字符串类型。
*   **`unicode_`**: 固定长度的 Unicode 字符串类型。

#### 查看数组的数据类型

您可以使用 `dtype` 属性来查看数组的数据类型。

```python
arr = np.array([1, 2, 3])
print(arr.dtype)  # 输出: int64 (具体位数可能因平台而异)
```

#### 指定和转换数据类型

可以在创建数组时使用 `dtype` 参数显式指定数据类型。 也可以使用 `astype()` 方法来转换现有数组的数据类型。

```python
# 指定数据类型
float_array = np.array([1, 2, 3], dtype=np.float64)

# 转换数据类型
int_array = float_array.astype(np.int32)
```

---

### 数组索引与切片

NumPy 数组的索引和切片机制非常灵活，是其强大功能的重要组成部分。

#### 一维数组索引与切片

一维数组的索引和切片与 Python 列表类似。

```python
arr = np.arange(10)

# 索引
print(arr[5])  # 输出: 5

# 切片
print(arr[2:5]) # 输出: [2 3 4]

# 省略起始或结束索引
print(arr[:5])  # 输出: [0 1 2 3 4]
print(arr[5:])  # 输出: [5 6 7 8 9]
```

#### 多维数组索引与切片

对于多维数组，您可以使用逗号分隔的索引元组来访问元素。

```python
arr_2d = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])

# 访问单个元素
print(arr_2d[1, 2]) # 输出: 6

# 切片子数组
print(arr_2d[:2, 1:]) # 输出: [[2 3]
                   #        [5 6]]
```

#### 布尔索引

布尔索引允许您根据条件来选择数组中的元素。

```python
names = np.array(['Bob', 'Joe', 'Will', 'Bob', 'Will', 'Joe', 'Joe'])
data = np.random.randn(7, 4)

# 选择所有 'Bob' 对应的数据行
print(data[names == 'Bob'])
```

#### 花式索引

花式索引是使用整数数组进行索引。这允许您以任意顺序选择元素。

```python
arr = np.empty((8, 4))
for i in range(8):
    arr[i] = i

# 以特定顺序选择行
print(arr[[4, 3, 0, 6]])
```

---

### 数组操作与广播

#### 数组形状操作

*   **`reshape()`**: 在不改变数据的情况下，为数组赋予新的形状。
*   **`ravel()` / `flatten()`**: 将多维数组展平为一维数组。`ravel()` 返回视图（如果可能），而 `flatten()` 总是返回一个副本。
*   **`T` / `transpose()`**: 对数组进行转置。

```python
arr = np.arange(12)
reshaped_arr = arr.reshape(3, 4)
transposed_arr = reshaped_arr.T
```

#### 数组的连接与分割

*   **`concatenate()`**: 沿指定轴连接一系列数组。
*   **`vstack()` / `hstack()`**: 分别用于垂直和水平堆叠数组。
*   **`split()`**: 将一个数组沿指定轴分割成多个子数组。

#### 广播机制

广播（Broadcasting）是 NumPy 中一项强大的功能，它允许 NumPy 在执行算术运算时，处理不同形状的数组。 广播遵循一套严格的规则，以确定两个数组如何进行交互。

简单来说，如果两个数组的形状不匹配，NumPy 会自动“广播”较小的数组，使其形状与较大的数组兼容，以便进行元素级运算。

```python
a = np.array([1.0, 2.0, 3.0])
b = 2.0
print(a * b) # 输出: [2. 4. 6.]
# 这里，标量 b 被广播到与数组 a 相同的大小。

c = np.array([[0, 0, 0],
              [10, 10, 10],
              [20, 20, 20],
              [30, 30, 30]])
d = np.array([0, 1, 2])
print(c + d) # 这里，一维数组 d 被广播到 c 的每一行。
```

---

### 通用函数 (ufunc)

通用函数（ufunc）是对 `ndarray` 中的数据执行元素级运算的函数。 它们是 NumPy 核心功能的重要组成部分，因为它们以编译后的 C 代码速度执行，从而提供了极高的性能。

#### 什么是通用函数？

ufunc 是一种“矢量化”的包装器，它接受固定数量的输入，并产生固定数量的输出。

#### 元素级运算

包括加、减、乘、除等基本算术运算。

```python
arr1 = np.array([1, 2, 3])
arr2 = np.array([4, 5, 6])
print(arr1 + arr2) # 输出: [5 7 9]
```

#### 数学函数

NumPy 提供了大量的数学函数，如 `sqrt`, `exp`, `log`, `sin`, `cos` 等。

```python
arr = np.array([1, 4, 9])
print(np.sqrt(arr)) # 输出: [1. 2. 3.]
```

#### 聚合函数

这些函数对数组中的元素进行聚合操作，例如：

*   **`sum()`**: 计算元素的和。
*   **`mean()`**: 计算元素的平均值。
*   **`std()`**: 计算元素的标准差。
*   **`min()` / `max()`**: 找出最小值/最大值。
*   **`argmin()` / `argmax()`**: 找出最小值/最大值的索引。

---

### 线性代数

NumPy 的 `linalg` 模块提供了全面的线性代数运算。

#### 矩阵乘法

可以使用 `@` 运算符或 `dot()` 函数进行矩阵乘法。

```python
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])
print(A @ B)
```

#### 行列式

使用 `linalg.det()` 计算矩阵的行列式。

```python
A = np.array([[1, 2], [3, 4]])
print(np.linalg.det(A))
```

#### 矩阵的逆

使用 `linalg.inv()` 计算可逆矩阵的逆。

```python
A = np.array([[1, 2], [3, 4]])
print(np.linalg.inv(A))
```

#### 求解线性方程组

使用 `linalg.solve()` 求解形如 Ax = b 的线性方程组。

```python
A = np.array([[2, 4], [6, 8]])
b = np.array([5, 6])
x = np.linalg.solve(A, b)
```

#### 特征值与特征向量

使用 `linalg.eig()` 计算方阵的特征值和特征向量。

#### 奇异值分解 (SVD)

使用 `linalg.svd()` 对矩阵进行奇异值分解。

---

### 随机数生成

NumPy 的 `random` 模块提供了强大的随机数生成功能。

#### 基础随机数

*   **`rand(d0, d1, ..., dn)`**: 创建一个给定形状的数组，并用[0, 1)之间的均匀分布的随机样本填充。
*   **`randn(d0, d1, ..., dn)`**: 创建一个给定形状的数组，并用标准正态分布（均值为0，方差为1）的随机样本填充。
*   **`randint(low, high=None, size=None, dtype=int)`**: 返回指定范围内的随机整数。

#### 从特定分布中抽样

`random` 模块还支持从多种概率分布中生成随机数，例如 `uniform`, `normal`, `binomial` 等。

#### 随机排列

*   **`shuffle(x)`**: 就地随机打乱序列。
*   **`permutation(x)`**: 返回一个序列的随机排列。

#### 可复现的随机性：随机数种子

为了使随机数生成过程可复现，可以设置随机数种子。 现代 NumPy 推荐使用 `default_rng()` 来创建一个生成器实例，然后使用该实例的方法来生成随机数。

```python
from numpy.random import default_rng
rng = default_rng(seed=42) # 使用种子创建一个生成器
random_numbers = rng.standard_normal(10)
```