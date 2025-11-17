## Pythonic编程之道：从优秀到卓越的代码技巧

### 目录

- [Pythonic编程之道：从优秀到卓越的代码技巧](#pythonic编程之道从优秀到卓越的代码技巧)
  - [目录](#目录)
  - [1. 字符串格式化 (String Formatting)](#1-字符串格式化-string-formatting)
    - [1.1. 案例对比](#11-案例对比)
    - [1.2. 深入补充：f-string 的优势与其他方法](#12-深入补充f-string-的优势与其他方法)
  - [2. 布尔值判断 (Truthiness)](#2-布尔值判断-truthiness)
    - [2.1. 案例对比](#21-案例对比)
    - [2.2. 深入补充：Python 中的“Falsy”值](#22-深入补充python-中的falsy值)
  - [3. 幂运算 (Exponentiation)](#3-幂运算-exponentiation)
    - [3.1. 案例对比](#31-案例对比)
    - [3.2. 深入补充：“^”的真实身份与 pow() 函数](#32-深入补充的真实身份与-pow-函数)
  - [4. 文件操作 (File Handling)](#4-文件操作-file-handling)
    - [4.1. 案例对比](#41-案例对比)
    - [4.2. 深入补充：with 语句与上下文管理器](#42-深入补充with-语句与上下文管理器)
  - [5. 大数字的可读性 (Large Number Readability)](#5-大数字的可读性-large-number-readability)
    - [5.1. 案例对比](#51-案例对比)
    - [5.2. 深入补充：PEP 515 与应用场景](#52-深入补充pep-515-与应用场景)
  - [6. 日志与调试 (Logging \& Debugging)](#6-日志与调试-logging--debugging)
    - [6.1. 案例对比](#61-案例对比)
    - [6.2. 深入补充：logging 模块的强大之处](#62-深入补充logging-模块的强大之处)
  - [7. 可变类型的默认参数 (Mutable Default Arguments)](#7-可变类型的默认参数-mutable-default-arguments)
    - [7.1. 案例对比（陷阱演示）](#71-案例对比陷阱演示)
    - [7.2. 深入补充：问题根源与正确实践](#72-深入补充问题根源与正确实践)
  - [8. 可变类型的传递与修改 (Passing Mutable Types)](#8-可变类型的传递与修改-passing-mutable-types)
    - [8.1. 案例对比](#81-案例对比)
    - [8.2. 深入补充：引用、浅拷贝与深拷贝](#82-深入补充引用浅拷贝与深拷贝)
  - [9. 字典遍历 (Dictionary Iteration)](#9-字典遍历-dictionary-iteration)
    - [9.1. 案例对比](#91-案例对比)
    - [9.2. 深入补充：字典遍历的最佳实践](#92-深入补充字典遍历的最佳实践)
  - [10. 推导式 (Comprehensions)](#10-推导式-comprehensions)
    - [10.1. 案例对比](#101-案例对比)
    - [10.2. 深入补充：推导式的威力与生成器表达式](#102-深入补充推导式的威力与生成器表达式)
  - [11. 类型检查 (Type Checking)](#11-类型检查-type-checking)
    - [11.1. 案例对比](#111-案例对比)
    - [11.2. 深入补充：isinstance 为何更胜一筹](#112-深入补充isinstance-为何更胜一筹)
  - [12. 其他重要技巧](#12-其他重要技巧)
    - [12.1. 元组打包与解包 (Tuple Packing \& Unpacking)](#121-元组打包与解包-tuple-packing--unpacking)
    - [12.2. 精准计时 (Timers)](#122-精准计时-timers)

---

### <a name="1"></a>1. 字符串格式化 (String Formatting)

提升代码可读性的第一步，往往是从如何清晰地构建字符串开始。

#### <a name="1.1"></a>1.1. 案例对比

*   **不推荐**: 使用 `+` 拼接，繁琐且易出错。当混合非字符串类型时，必须手动调用 `str()` 进行转换。

    ```python
    # 不推荐
    name = "Alice"
    score = 95
    print("Congratulations " + name + "! your score is " + str(score))
    ```

*   **推荐**: 使用 f-string (格式化字符串字面值)，代码更简洁、直观。

    ```python
    # 推荐
    name = "Alice"
    score = 95
    print(f"Congratulations {name}! your score is {score}")
    ```

#### <a name="1.2"></a>1.2. 深入补充：f-string 的优势与其他方法

> f-string (自 Python 3.6 引入) 之所以备受推崇，是因为它在编译时将表达式直接解析并嵌入字符串，不仅可读性极高，执行效率也通常优于其他方法。

*   **f-string 的高级用法**:
    你可以在 `{}` 中执行函数、进行计算，甚至设置格式。
    ```python
    import datetime
    name = "Bob"
    scores = [88, 92, 76]
    print(f"Hello, {name.upper()}!")
    print(f"Your average score is {sum(scores) / len(scores):.2f}.") # :.2f 表示保留两位小数
    print(f"Report generated on: {datetime.date.today()}")
    ```

*   **其他格式化方法**:
    *   **`str.format()`**: 在 f-string 出现之前是官方推荐的方法，功能强大且灵活。
        ```python
        name = "Charlie"
        score = 89
        # 通过位置
        print("Congratulations {}! your score is {}".format(name, score))
        # 通过关键字
        print("Congratulations {name}! your score is {score}".format(name=name, score=score))
        ```
    *   **`%` 操作符**: 这是从 C 语言 `printf` 继承的传统方法，在新代码中已不推荐使用。
        ```python
        name = "David"
        score = 78
        print("Congratulations %s! your score is %d" % (name, score))
        ```

### <a name="2"></a>2. 布尔值判断 (Truthiness)

利用 Python 的真值测试特性，可以让条件判断语句更加简洁、地道。

#### <a name="2.1"></a>2.1. 案例对比

*   **不推荐**: 显式地检查集合的长度。
    ```python
    # 不推荐
    my_list = [1, 2, 3]
    if len(my_list) > 0:
        print("The list is not empty.")
    ```

*   **推荐**: 直接利用对象的“真值”进行判断。
    ```python
    # 推荐
    my_list = [1, 2, 3]
    if my_list:
        print("The list is not empty.")
    ```

#### <a name="2.2"></a>2.2. 深入补充：Python 中的“Falsy”值

> 在布尔上下文中，以下对象会被自动视为 `False`，我们称之为“Falsy”值。除它们之外的一切都被视为 `True`。

*   `None`
*   `False`
*   任何数值类型的零: `0`, `0.0`, `0j`
*   空的序列或集合: `''` (空字符串), `()` (空元组), `[]` (空列表), `{}` (空字典), `set()` (空集合), `range(0)`

**示例代码**:
```python
def check_truthiness(obj):
    if obj:
        print(f"{repr(obj)} is Truthy")
    else:
        print(f"{repr(obj)} is Falsy")

check_truthiness([1, 2])    # [1, 2] is Truthy
check_truthiness([])        # [] is Falsy

check_truthiness("Hello")   # 'Hello' is Truthy
check_truthiness("")        # '' is Falsy

check_truthiness(100)       # 100 is Truthy
check_truthiness(0)         # 0 is Falsy

check_truthiness({'a': 1})  # {'a': 1} is Truthy
check_truthiness({})        # {} is Falsy

check_truthiness(None)      # None is Falsy
```

### <a name="3"></a>3. 幂运算 (Exponentiation)

选择正确的运算符至关重要，`^` 在 Python 中并非用于数学中的幂运算。

#### <a name="3.1"></a>3.1. 案例对比

*   **错误**: 使用 `^` 进行幂运算，这是一个常见的误解。
    ```python
    # 错误
    # 意图: 计算 2 的 3 次方 (结果应为 8)
    # 实际: 执行按位异或 (2^3 的结果是 1)
    result = 2 ^ 3
    print(f"2 ^ 3 = {result}") # 输出: 2 ^ 3 = 1
    ```

*   **正确**: 使用 `**` 运算符。
    ```python
    # 正确
    result = 2 ** 3
    print(f"2 ** 3 = {result}") # 输出: 2 ** 3 = 8
    ```

#### <a name="3.2"></a>3.2. 深入补充：“^”的真实身份与 pow() 函数

> `^` 在 Python 中是 **按位异或 (Bitwise XOR)** 运算符。它对两个数的二进制表示进行操作。

**`^` (XOR) vs `**` (Power) 详解**:
```python
# 5 的二进制是 101
# 3 的二进制是 011

# 5 ^ 3 (XOR)
# 101
# 011
# ---
# 110  (二进制的 110 是十进制的 6)
print(f"5 ^ 3 = {5 ^ 3}") # 输出 6

# 5 ** 3 (Power)
# 5 * 5 * 5 = 125
print(f"5 ** 3 = {5 ** 3}") # 输出 125
```

*   **`pow()` 函数**:
    除了 `**`，还可以使用内置函数 `pow(x, y)`。它还接受可选的第三个参数用于计算模幂，这在密码学等领域非常高效。
    ```python
    # 等同于 2 ** 3
    print(pow(2, 3)) # 输出 8

    # 计算 (2 ** 3) % 5, 即 8 % 5
    print(pow(2, 3, 5)) # 输出 3
    ```

### <a name="4"></a>4. 文件操作 (File Handling)

使用 `with` 语句管理文件等资源，是保证代码健壮性的关键。

#### <a name="4.1"></a>4.1. 案例对比

*   **不推荐**: 手动 `open()` 和 `close()`。如果 `write` 过程中发生异常，`f.close()` 可能永远不会被执行，导致资源泄漏。
    ```python
    # 不推荐
    f = open("hello.txt", "w")
    f.write("Hello world")
    # 如果这里发生异常，文件将不会被关闭
    f.close()
    ```

*   **推荐**: 使用 `with` 语句，无论代码块内是否发生异常，都能确保文件被自动关闭。
    ```python
    # 推荐
    with open("hello.txt", "w") as f:
        f.write("Hello world")
    # 到这里，文件 f 已被自动关闭
    ```

#### <a name="4.2"></a>4.2. 深入补充：with 语句与上下文管理器

> `with` 语句使用的是 Python 的 **上下文管理器协议 (Context Manager Protocol)**。任何实现了 `__enter__()` 和 `__exit__()` 方法的对象都可以用于 `with` 语句。

这使得 `with` 的应用远不止文件操作，还包括：
*   数据库连接
*   线程锁
*   网络请求会话

**`try...finally` 的等价实现**:
在 `with` 出现之前，为了确保资源被释放，需要使用 `try...finally` 结构，代码更为冗长。
```python
f = open("hello.txt", "w")
try:
    f.write("Hello world")
finally:
    f.close() # 确保无论 try 块中发生什么，都会执行 close
```
`with` 语句是上述模式的优雅语法糖。

### <a name="5"></a>5. 大数字的可读性 (Large Number Readability)

让长串的数字更易于人类阅读。

#### <a name="5.1"></a>5.1. 案例对比

*   **不推荐**: 数字过长，难以一眼看出其量级。
    ```python
    # 不推荐
    population = 1400000000
    fee = 12500.50
    ```
*   **推荐**: 使用下划线 `_` 作为视觉分隔符。
    ```python
    # 推荐
    population = 1_400_000_000
    fee = 12_500.50
    ```

#### <a name="5.2"></a>5.2. 深入补充：PEP 515 与应用场景

> 这个特性是在 **Python 3.6** 中通过 PEP 515 引入的。下划线 `_` 仅用于提升可读性，对数字的值本身没有任何影响。

它同样适用于浮点数、十六进制、八进制和二进制数：
```python
# 整数和浮点数
million = 1_000_000
pi_approx = 3.141_592_653

# 十六进制、八进制和二进制
hex_val = 0x_BAD_C0DE
oct_val = 0o_17_42
bin_val = 0b_1110_1011

print(million)      # 1000000
print(hex_val)      # -113524230
print(bin_val)      # 235
```

### <a name="6"></a>6. 日志与调试 (Logging & Debugging)

从 `print()` 迁移到 `logging` 是代码从“脚本”走向“工程”的重要一步。

#### <a name="6.1"></a>6.1. 案例对比

*   **不推荐**: 使用 `print()` 进行调试。在生产环境中，这些 `print` 语句难以管理，要么需要手动删除，要么会污染标准输出。
    ```python
    # 不推荐
    def process_data(data):
        print("Starting data processing...")
        # ... 业务逻辑 ...
        print(f"Processing complete for: {data}")
    ```

*   **推荐**: 使用 `logging` 模块，它提供了更灵活、更强大的日志系统。
    ```python
    # 推荐
    import logging

    logging.basicConfig(level=logging.INFO) # 基础配置

    def process_data(data):
        logging.info("Starting data processing...")
        # ... 业务逻辑 ...
        logging.debug(f"Intermediate data state: {data}") # DEBUG 级别默认不显示
        logging.info(f"Processing complete for: {data}")
    ```

#### <a name="6.2"></a>6.2. 深入补充：logging 模块的强大之处

> `logging` 模块提供了层次化的日志级别、灵活的输出配置和格式化功能。

1.  **分级系统**:
    *   `DEBUG`: 详细信息，通常仅在诊断问题时使用。
    *   `INFO`: 确认事情按预期工作。
    *   `WARNING`: 表明有意外发生，或预示未来可能出现问题。
    *   `ERROR`: 由于更严重的问题，软件的某些功能无法执行。
    *   `CRITICAL`: 严重错误，表明程序本身可能无法继续运行。

2.  **灵活配置**: 可以将日志同时输出到控制台和文件，并为它们设置不同的日志级别和格式。

**实用配置示例**:
```python
import logging

# 1. 创建一个 logger
logger = logging.getLogger('my_app')
logger.setLevel(logging.DEBUG) # 设置 logger 的最低处理级别

# 2. 创建一个 handler，用于写入日志文件
file_handler = logging.FileHandler('app.log')
file_handler.setLevel(logging.WARNING) # 文件只记录 WARNING 及以上级别

# 3. 创建另一个 handler，用于输出到控制台
console_handler = logging.StreamHandler()
console_handler.setLevel(logging.INFO) # 控制台记录 INFO 及以上级别

# 4. 定义 handler 的输出格式
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
file_handler.setFormatter(formatter)
console_handler.setFormatter(formatter)

# 5. 将 handler 添加到 logger
logger.addHandler(file_handler)
logger.addHandler(console_handler)

# --- 使用 logger ---
logger.debug("This is a debug message.")       # 不会显示在任何地方
logger.info("This is an info message.")        # 只显示在控制台
logger.warning("This is a warning message.")    # 同时显示在控制台和文件
logger.error("This is an error message.")      # 同时显示在控制台和文件
```

### <a name="7"></a>7. 可变类型的默认参数 (Mutable Default Arguments)

这是 Python 中最著名的“陷阱”之一，必须警惕。

#### <a name="7.1"></a>7.1. 案例对比（陷阱演示）

*   **危险用法**: 将列表 `[]` 或字典 `{}` 等可变类型用作函数默认参数。
    ```python
    # 危险
    def add_item(item, item_list=[]):
        item_list.append(item)
        return item_list

    # 第一次调用，符合预期
    list1 = add_item("apple")
    print(list1)  # 输出: ['apple']

    # 第二次调用，出现意外！
    list2 = add_item("banana")
    print(list2)  # 期望输出: ['banana'], 实际输出: ['apple', 'banana']

    # 验证它们是同一个对象
    print(f"list1 is list2: {list1 is list2}") # 输出: list1 is list2: True
    ```

#### <a name="7.2"></a>7.2. 深入补充：问题根源与正确实践

> **核心原因**: 函数的默认参数是在**函数定义时**被创建和评估的，只创建一次。因此，所有对该函数的调用（在未提供该参数的情况下）都共享同一个默认参数对象。

*   **推荐做法**: 将默认值设为不可变的 `None`，在函数内部检查并初始化。
    ```python
    # 推荐
    def add_item_safe(item, item_list=None):
        if item_list is None:
            item_list = []  # 在函数调用时创建新列表
        item_list.append(item)
        return item_list

    # 第一次调用
    list1 = add_item_safe("apple")
    print(list1)  # 输出: ['apple']

    # 第二次调用，现在表现正常
    list2 = add_item_safe("banana")
    print(list2)  # 输出: ['banana']

    # 验证它们是不同的对象
    print(f"list1 is list2: {list1 is list2}") # 输出: list1 is list2: False
    ```

### <a name="8"></a>8. 可变类型的传递与修改 (Passing Mutable Types)

理解 Python 中变量赋值是“引用赋值”对于避免意外修改数据至关重要。

#### <a name="8.1"></a>8.1. 案例对比

*   **问题**: 直接赋值（`lst2 = lst`）不会创建新列表，`lst` 和 `lst2` 指向同一个内存地址。修改其中一个会影响另一个。
    ```python
    def modify_list(lst):
        print(f"Original list ID inside function: {id(lst)}")
        lst2 = lst # 引用赋值，lst2 和 lst 指向同一个对象
        print(f"Copied list ID inside function:   {id(lst2)}")
        lst2.append(100) # 修改操作会影响原始列表
        print("List modified inside function.")

    original = [1, 2, 3]
    print(f"Original list ID before call: {id(original)}")
    modify_list(original)
    print(f"Original list after call: {original}") # 输出: [1, 2, 3, 100]
    ```

*   **解决方案**: 创建列表的副本进行操作。
    ```python
    def modify_list_safely(lst):
        # 创建一个浅拷贝
        lst2 = lst.copy()
        # 或者使用切片: lst2 = lst[:]
        print(f"Original list ID inside function: {id(lst)}")
        print(f"Copied list ID inside function:   {id(lst2)}") # ID 不同了！
        lst2.append(100)
        print("Copied list modified, original remains unchanged.")

    original = [1, 2, 3]
    print(f"Original list ID before call: {id(original)}")
    modify_list_safely(original)
    print(f"Original list after call: {original}") # 输出: [1, 2, 3]
    ```

#### <a name="8.2"></a>8.2. 深入补充：引用、浅拷贝与深拷贝

*   **浅拷贝 (Shallow Copy)**: `list.copy()` 或 `list[:]` 创建一个新列表，但如果列表内包含其他可变对象（如子列表），它只拷贝对这些内部对象的引用。

    **浅拷贝的局限性**:
    ```python
    import copy

    original = [1, 2, [10, 20]]
    shallow_copy = original.copy()

    shallow_copy.append(3) # 不影响 original
    shallow_copy[2].append(30) # !!! 会影响 original !!!

    print(f"Original:       {original}")       # 输出: [1, 2, [10, 20, 30]]
    print(f"Shallow Copy:   {shallow_copy}")   # 输出: [1, 2, [10, 20, 30], 3]
    ```
*   **深拷贝 (Deep Copy)**: 如果需要一个完全独立的副本（包括所有嵌套的可变对象），必须使用 `copy` 模块的 `deepcopy`。

    ```python
    import copy

    original = [1, 2, [10, 20]]
    deep_copy = copy.deepcopy(original)

    deep_copy.append(3) # 不影响 original
    deep_copy[2].append(30) # 不会影响 original

    print(f"Original:   {original}")   # 输出: [1, 2, [10, 20]]
    print(f"Deep Copy:  {deep_copy}")  # 输出: [1, 2, [10, 20, 30], 3]
    ```

### <a name="9"></a>9. 字典遍历 (Dictionary Iteration)

选择最高效、最易读的方式来遍历字典。

#### <a name="9.1"></a>9.1. 案例对比

*   **冗余**: 使用 `d.keys()` 遍历键。虽然功能正确，但不是最简洁的方式。
    ```python
    my_dict = {'a': 1, 'b': 2}
    # 冗余
    for key in my_dict.keys():
        print(key)
    ```
*   **推荐**: 直接遍历字典对象本身，默认就是遍历它的键。
    ```python
    my_dict = {'a': 1, 'b': 2}
    # 推荐
    for key in my_dict:
        print(key)
    ```

#### <a name="9.2"></a>9.2. 深入补充：字典遍历的最佳实践

*   **遍历值**: 使用 `.values()`
    ```python
    for value in my_dict.values():
        print(value)
    ```
*   **同时遍历键和值 (最佳实践)**: 使用 `.items()`，这是最高效、最常用的方式，因为它避免了通过键再次查找值的开销。
    ```python
    # 推荐
    for key, value in my_dict.items():
        print(f"Key: {key}, Value: {value}")

    # 不推荐 (效率稍低)
    for key in my_dict:
        value = my_dict[key] # 每次循环都需要做一次哈希查找
        print(f"Key: {key}, Value: {value}")
    ```

### <a name="10"></a>10. 推导式 (Comprehensions)

推导式是 Python 的标志性特性之一，它能让你用一行代码优雅地创建列表、字典和集合。

#### <a name="10.1"></a>10.1. 案例对比

*   **不推荐**: 使用传统的 for 循环和 `.append()` 来创建列表。
    ```python
    # 不推荐
    data = [1, 2, 3, 4, 5]
    squares = []
    for i in data:
        squares.append(i * i)
    ```

*   **推荐**: 使用列表推导式，代码更紧凑，也通常更快。
    ```python
    # 推荐
    data = [1, 2, 3, 4, 5]
    squares = [i * i for i in data]
    ```

#### <a name="10.2"></a>10.2. 深入补充：推导式的威力与生成器表达式

*   **带条件的推导式**:
    ```python
    # 只计算偶数的平方
    even_squares = [i * i for i in data if i % 2 == 0]
    print(even_squares) # 输出: [4, 16]
    ```
*   **字典推导式 (Dictionary Comprehension)**:
    ```python
    names = ["Alice", "Bob", "Charlie"]
    name_lengths = {name: len(name) for name in names}
    print(name_lengths) # 输出: {'Alice': 5, 'Bob': 3, 'Charlie': 7}
    ```
*   **集合推导式 (Set Comprehension)**:
    ```python
    numbers = [1, 2, 2, 3, 4, 4, 5]
    unique_squares = {x * x for x in numbers}
    print(unique_squares) # 输出: {1, 4, 9, 16, 25} (自动去重)
    ```
*   **生成器表达式 (Generator Expression)**:
    `t = (i * i for i in data)` 创建的不是元组，而是一个 **生成器**。它不会立即计算所有值并存入内存，而是在被迭代时“懒惰地”逐个生成值，极大地节省了内存，特别适合处理大数据集。

    ```python
    data = [1, 2, 3, 4, 5]
    squares_gen = (i * i for i in data)

    print(squares_gen) # 输出一个生成器对象, <generator object <genexpr> at ...>

    # 迭代生成器以获取值
    for square in squares_gen:
        print(square)

    # 如果确实需要一个元组，必须显式转换
    squares_tuple = tuple(i * i for i in data)
    print(squares_tuple) # 输出: (1, 4, 9, 16, 25)
    ```

### <a name="11"></a>11. 类型检查 (Type Checking)

在需要检查对象类型时，`isinstance()` 是比 `type()` 更健壮、更符合面向对象思想的选择。

#### <a name="11.1"></a>11.1. 案例对比

*   **不推荐**: 使用 `type(obj) == T`。这种检查过于严格，无法处理继承关系。
    ```python
    class MyList(list): # MyList 继承自 list
        pass

    my_instance = MyList([1, 2, 3])

    if type(my_instance) == list:
        print("type() check: It's a list.")
    else:
        print("type() check: It's not a list.") # 输出这句
    ```

*   **推荐**: 使用 `isinstance(obj, T)`，它会考虑继承关系。
    ```python
    class MyList(list):
        pass

    my_instance = MyList([1, 2, 3])

    if isinstance(my_instance, list):
        print("isinstance() check: It's an instance of list or its subclass.") # 输出这句
    else:
        print("isinstance() check: It's not.")
    ```

#### <a name="11.2"></a>11.2. 深入补充：isinstance 为何更胜一筹

> `isinstance()` 检查一个对象是否是某个类或其任何子类的实例。这使得代码更加灵活和健壮，能够适应未来的代码扩展（例如，有人创建了你的类的新子类）。`type()` 则只进行精确的类型匹配。

`isinstance` 还支持检查多种类型：
```python
def process_item(item):
    if isinstance(item, (list, tuple, set)):
        print(f"Processing a collection: {item}")
    elif isinstance(item, str):
        print(f"Processing a string: {item}")
    else:
        print(f"Cannot process type: {type(item)}")

process_item([1, 2])
process_item("hello")
process_item(123)
```

### <a name="12"></a>12. 其他重要技巧

#### <a name="12.1"></a>12.1. 元组打包与解包 (Tuple Packing & Unpacking)

*   **打包 (Packing)**: 将多个值赋给一个变量时，Python 会自动将它们“打包”成一个元组。
    ```python
    my_tuple = 1, "hello", True # 自动创建元组 (1, "hello", True)
    print(my_tuple)
    ```
*   **解包 (Unpacking)**: 将一个序列（如元组或列表）的元素“解开”并赋值给多个变量。
    ```python
    coordinates = (10, 20, 30)
    x, y, z = coordinates # 解包
    print(f"x={x}, y={y}, z={z}")

    # 常用于交换变量
    a, b = 1, 2
    a, b = b, a # 优雅地交换 a 和 b 的值
    print(f"a={a}, b={b}") # a=2, b=1
    ```

#### <a name="12.2"></a>12.2. 精准计时 (Timers)

*   **不推荐**: `time.time()` 返回自纪元以来的秒数，它会受系统时间调整（如NTP同步）的影响，不适合精确测量代码执行耗时。`time.clock()` 已在 Python 3.8 中被移除。

*   **推荐**: `time.perf_counter()`，它提供了一个单调递增的时钟，不受系统时间变化的影响，是测量短时间间隔的首选。

    ```python
    import time

    start_time = time.perf_counter()

    # --- 执行一些耗时操作 ---
    sum(x*x for x in range(1_000_000))
    # ----------------------

    end_time = time.perf_counter()
    elapsed_time = end_time - start_time

    print(f"Operation took {elapsed_time:.6f} seconds.")
    ```