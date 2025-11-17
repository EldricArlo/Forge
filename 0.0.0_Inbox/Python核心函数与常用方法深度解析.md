# Python核心函数与常用方法深度解析

### 导航目录

- [Python核心函数与常用方法深度解析](#python核心函数与常用方法深度解析)
    - [导航目录](#导航目录)
    - [引言：函数 vs 方法](#引言函数-vs-方法)
  - [一、 核心内置函数](#一-核心内置函数)
    - [1. 输入与输出](#1-输入与输出)
      - [print()](#print)
      - [input()](#input)
    - [2. 类型转换](#2-类型转换)
      - [str(), int(), float()](#str-int-float)
      - [list(), tuple(), set(), dict(), frozenset()](#list-tuple-set-dict-frozenset)
    - [3. 数值计算](#3-数值计算)
      - [sum(), max(), min()](#sum-max-min)
      - [abs(), round()](#abs-round)
      - [pow(), divmod()](#pow-divmod)
    - [4. 序列与可迭代对象](#4-序列与可迭代对象)
      - [len(), range(), slice()](#len-range-slice)
      - [sorted(), reversed()](#sorted-reversed)
      - [zip(), enumerate()](#zip-enumerate)
      - [all(), any()](#all-any)
      - [map(), filter()](#map-filter)
    - [5. 对象、类型与属性](#5-对象类型与属性)
      - [type(), isinstance()](#type-isinstance)
      - [callable()](#callable)
      - [getattr(), setattr(), delattr()](#getattr-setattr-delattr)
    - [6. 字符与编码](#6-字符与编码)
      - [chr(), ord()](#chr-ord)
      - [bin(), hex(), oct()](#bin-hex-oct)
      - [bytes(), ascii()](#bytes-ascii)
    - [7. 动态代码执行](#7-动态代码执行)
      - [exec()](#exec)
    - [8. 文件操作](#8-文件操作)
      - [open()](#open)
  - [二、 常用数据类型方法](#二-常用数据类型方法)
    - [1. 字符串方法 (String Methods)](#1-字符串方法-string-methods)
      - [replace()](#replace)
      - [strip()](#strip)
      - [format()](#format)
    - [2. 列表方法 (List Methods)](#2-列表方法-list-methods)
      - [append(), extend()](#append-extend)
      - [insert()](#insert)
      - [remove(), pop()](#remove-pop)
      - [index(), count()](#index-count)
      - [sort(), reverse()](#sort-reverse)
  - [三、 标准库常用函数](#三-标准库常用函数)
      - [functools.reduce()](#functoolsreduce)
      - [random.random()](#randomrandom)
      - [time.sleep()](#timesleep)
      - [os.listdir()](#oslistdir)

---

### 引言：函数 vs 方法

在深入学习之前，理解**内置函数 (Built-in Function)** 和 **方法 (Method)** 的区别至关重要。

*   **内置函数**：是Python解释器自带的，可以在任何地方直接调用，无需导入任何模块或依赖于特定对象。例如 `print()` 和 `len()`。
*   **方法**：是与特定数据类型（对象）关联的函数。必须通过对象实例来调用，使用点 `.` 语法。例如，`"hello".replace("h", "H")` 中的 `replace()` 就是一个字符串方法。

本篇文档将明确区分这两者，并介绍一些标准库中的常用函数。

---

## 一、 核心内置函数

这些是Python提供的最基本、最核心的函数，无需导入任何模块即可全局使用。

### 1. 输入与输出

#### print()

*   **中文说明**：打印输出给定的内容到控制台。
*   **详细说明**：`print()` 是最常用的调试和输出函数，可以接收一个或多个参数，并用指定的分隔符和结尾符输出。
*   **示例代码**：
    ```python
    print("Hello, World!")

    name = "Alice"
    age = 30
    # 打印多个对象，默认以空格分隔
    print("Name:", name, "Age:", age) # 输出: Name: Alice Age: 30

    # 自定义分隔符和结尾符
    print(1, 2, 3, 4, sep='-', end=' END\n') # 输出: 1-2-3-4 END
    ```

#### input()

*   **中文说明**：接收用户的键盘输入，并始终以字符串形式返回。
*   **详细说明**：该函数会暂停程序执行，等待用户输入内容并按回车键。可以提供一个字符串参数作为提示信息。
*   **示例代码**：
    ```python
    prompt = "Please enter your name: "
    user_name = input(prompt)
    print(f"Hello, {user_name}!")

    # 注意：input() 返回的是字符串，如果需要数字，必须进行类型转换
    age_str = input("Enter your age: ")
    age_int = int(age_str)
    print(f"You will be {age_int + 1} next year.")
    ```

### 2. 类型转换

#### str(), int(), float()

*   **中文说明**：分别将对象转换为字符串、整数和浮点数。
*   **详细说明**：这是最基础的类型转换函数。`int()` 可以将符合格式的字符串或浮点数（舍去小数部分）转为整数。`float()` 可以将整数或符合格式的字符串转为浮点数。
*   **示例代码**：
    ```python
    # str()
    num = 100
    num_str = str(num)
    print(f"'{num_str}' is of type {type(num_str)}") # 输出: '100' is of type <class 'str'>

    # int()
    float_num = 99.9
    str_num = "123"
    print(int(float_num)) # 输出: 99
    print(int(str_num))   # 输出: 123

    # float()
    int_val = 77
    str_val = "3.14"
    print(float(int_val)) # 输出: 77.0
    print(float(str_val)) # 输出: 3.14
    ```

#### list(), tuple(), set(), dict(), frozenset()

*   **中文说明**：将一个可迭代对象转换为列表、元组、集合、字典或不可变集合。
*   **详细说明**：
    *   `list()`: 创建一个可变列表。
    *   `tuple()`: 创建一个不可变元组。
    *   `set()`: 创建一个包含不重复元素的可变集合。
    *   `dict()`: 从键值对序列中创建字典。
    *   `frozenset()`: 创建一个不可变的集合，可以作为字典的键。
*   **示例代码**：
    ```python
    my_tuple = (1, 2, 2, 3, 4)
    my_list_from_tuple = list(my_tuple)
    print(f"List: {my_list_from_tuple}") # 输出: List: [1, 2, 2, 3, 4]

    my_list = ['a', 'b', 'c', 'a']
    print(f"Tuple: {tuple(my_list)}")   # 输出: Tuple: ('a', 'b', 'c', 'a')
    print(f"Set: {set(my_list)}")     # 输出: Set: {'a', 'c', 'b'} (顺序可能不同)
    print(f"Frozenset: {frozenset(my_list)}") # 输出: Frozenset: frozenset({'b', 'a', 'c'})

    # dict()
    kv_pairs = [('name', 'Bob'), ('age', 25)]
    my_dict = dict(kv_pairs)
    print(f"Dict: {my_dict}") # 输出: Dict: {'name': 'Bob', 'age': 25}
    ```

### 3. 数值计算

#### sum(), max(), min()

*   **中文说明**：分别计算可迭代对象中所有元素的和、最大值和最小值。
*   **详细说明**：这些函数通常用于数字列表、元组等。`sum()` 还可以接受第二个参数作为起始值。
*   **示例代码**：
    ```python
    numbers = [10, -5, 100, 42, 0]
    print(f"Sum: {sum(numbers)}")     # 输出: Sum: 147
    print(f"Max: {max(numbers)}")     # 输出: Max: 100
    print(f"Min: {min(numbers)}")     # 输出: Min: -5

    # sum() with a starting value
    print(f"Sum with start 1000: {sum(numbers, 1000)}") # 输出: Sum with start 1000: 1147
    ```

#### abs(), round()

*   **中文说明**：`abs()` 返回一个数的绝对值，`round()` 对一个数进行四舍五入。
*   **详细说明**：`round()` 可以接受第二个参数，指定保留的小数位数。
*   **示例代码**：
    ```python
    # abs()
    print(f"abs(-99): {abs(-99)}")     # 输出: abs(-99): 99
    print(f"abs(3-4j): {abs(3-4j)}") # 输出: abs(3-4j): 5.0 (复数的模)

    # round()
    pi = 3.14159
    print(f"round(pi): {round(pi)}")         # 输出: round(pi): 3
    print(f"round(pi, 2): {round(pi, 2)}") # 输出: round(pi, 2): 3.14
    print(f"round(2.5): {round(2.5)}")       # 输出: round(2.5): 2 (Python 3 中会舍入到最近的偶数)
    print(f"round(3.5): {round(3.5)}")       # 输出: round(3.5): 4
    ```

#### pow(), divmod()

*   **中文说明**：`pow()` 计算乘方，`divmod()` 返回商和余数。
*   **详细说明**：`pow(x, y)` 等价于 `x ** y`。`divmod(a, b)` 返回一个元组 `(a // b, a % b)`。
*   **示例代码**：
    ```python
    # pow()
    print(f"pow(2, 10): {pow(2, 10)}") # 输出: pow(2, 10): 1024
    print(f"pow(3, 3, 5): {pow(3, 3, 5)}") # (3**3) % 5 = 27 % 5 = 2

    # divmod()
    result = divmod(10, 3)
    print(f"divmod(10, 3): {result}")       # 输出: divmod(10, 3): (3, 1)
    print(f"Quotient: {result[0]}, Remainder: {result[1]}")
    ```

### 4. 序列与可迭代对象

#### len(), range(), slice()

*   **中文说明**：`len()` 返回长度，`range()` 生成整数序列，`slice()` 创建切片对象。
*   **详细说明**：
    *   `len()`: 返回容器（如字符串、列表、字典）中的项目数。
    *   `range(start, stop, step)`: 生成一个从 `start` 到 `stop-1` 的整数序列，步长为 `step`。
    *   `slice()`: 用于创建切片对象，在索引操作中复用。
*   **示例代码**：
    ```python
    # len()
    my_str = "Python"
    print(f"Length of '{my_str}' is {len(my_str)}") # 输出: Length of 'Python' is 6

    # range()
    print(f"range(5): {list(range(5))}")             # 输出: range(5): [0, 1, 2, 3, 4]
    print(f"range(2, 8): {list(range(2, 8))}")       # 输出: range(2, 8): [2, 3, 4, 5, 6, 7]
    print(f"range(0, 10, 2): {list(range(0, 10, 2))}") # 输出: range(0, 10, 2): [0, 2, 4, 6, 8]

    # slice()
    items = [0, 1, 2, 3, 4, 5, 6]
    s = slice(2, 5) # 创建一个从索引2到5（不含）的切片
    print(f"items[s]: {items[s]}") # 输出: items[s]: [2, 3, 4]
    ```

#### sorted(), reversed()

*   **中文说明**：`sorted()` 返回一个排序后的新列表，`reversed()` 返回一个反向迭代器。
*   **详细说明**：`sorted()` 不会修改原始对象，可以对任何可迭代对象进行排序。`reversed()` 也是非破坏性的，它返回一个迭代器，需要用 `list()` 等转换为具体序列。
*   **示例代码**：
    ```python
    numbers = [3, 1, 4, 1, 5, 9, 2]

    # sorted()
    sorted_numbers = sorted(numbers)
    print(f"Original: {numbers}")           # 输出: Original: [3, 1, 4, 1, 5, 9, 2]
    print(f"Sorted: {sorted_numbers}")     # 输出: Sorted: [1, 1, 2, 3, 4, 5, 9]
    print(f"Reverse Sorted: {sorted(numbers, reverse=True)}") # 输出: Reverse Sorted: [9, 5, 4, 3, 2, 1, 1]

    # reversed()
    reversed_iterator = reversed(numbers)
    print(f"Reversed list: {list(reversed_iterator)}") # 输出: Reversed list: [2, 9, 5, 1, 4, 1, 3]
    ```

#### zip(), enumerate()

*   **中文说明**：`zip()` 将多个可迭代对象聚合，`enumerate()` 同时返回索引和值。
*   **详细说明**：
    *   `zip()`: 将多个序列的对应元素打包成一个个元组，返回一个zip对象（迭代器）。聚合长度以最短的序列为准。
    *   `enumerate()`: 在遍历一个序列时，同时获得每个元素的索引和值，非常方便。
*   **示例代码**：
    ```python
    # zip()
    names = ["Alice", "Bob", "Charlie"]
    ages = [30, 25, 35]
    cities = ["New York", "London", "Paris"]
    zipped = zip(names, ages, cities)
    print(f"Zipped list: {list(zipped)}")
    # 输出: Zipped list: [('Alice', 30, 'New York'), ('Bob', 25, 'London'), ('Charlie', 35, 'Paris')]

    # enumerate()
    fruits = ["apple", "banana", "cherry"]
    for index, fruit in enumerate(fruits):
        print(f"Index {index}: {fruit}")
    # 输出:
    # Index 0: apple
    # Index 1: banana
    # Index 2: cherry
    ```

#### all(), any()

*   **中文说明**：`all()` 判断所有元素是否为真，`any()` 判断是否存在任何一个为真的元素。
*   **详细说明**：对于空的迭代器，`all()` 返回 `True`，`any()` 返回 `False`。
*   **示例代码**：
    ```python
    list1 = [True, 1, "hello"]      # 所有元素都为 "truthy"
    list2 = [True, 0, "hello"]      # 包含 "falsy" 元素 0
    list3 = [False, 0, ""]          # 所有元素都为 "falsy"

    # all()
    print(f"all(list1): {all(list1)}") # 输出: all(list1): True
    print(f"all(list2): {all(list2)}") # 输出: all(list2): False

    # any()
    print(f"any(list2): {any(list2)}") # 输出: any(list2): True
    print(f"any(list3): {any(list3)}") # 输出: any(list3): False
    ```

#### map(), filter()

*   **中文说明**：`map()` 对序列每个元素应用函数，`filter()` 根据函数条件过滤序列。
*   **详细说明**：这两个函数都返回迭代器，这在处理大数据集时非常高效。
    *   `map(function, iterable)`: 将 `function` 应用于 `iterable` 的每个元素，返回结果组成的迭代器。
    *   `filter(function, iterable)`: 保留 `iterable` 中使 `function` 返回 `True` 的元素，组成新的迭代器。
*   **示例代码**：
    ```python
    numbers = [1, 2, 3, 4, 5]

    # map()
    def square(n):
        return n * n
    squares = map(square, numbers)
    print(f"Squares with map: {list(squares)}") # 输出: Squares with map: [1, 4, 9, 16, 25]

    # filter()
    def is_even(n):
        return n % 2 == 0
    evens = filter(is_even, numbers)
    print(f"Evens with filter: {list(evens)}") # 输出: Evens with filter: [2, 4]
    ```

### 5. 对象、类型与属性

#### type(), isinstance()

*   **中文说明**：`type()` 返回对象类型，`isinstance()` 检查对象是否为指定类型（或其子类）的实例。
*   **详细说明**：在检查类型时，通常推荐使用 `isinstance()`，因为它能处理继承关系，代码更健壮。
*   **示例代码**：
    ```python
    x = 10
    y = "hello"

    print(f"type(x) is {type(x)}") # 输出: type(x) is <class 'int'>

    print(f"isinstance(x, int): {isinstance(x, int)}") # 输出: isinstance(x, int): True
    print(f"isinstance(y, str): {isinstance(y, str)}") # 输出: isinstance(y, str): True
    print(f"isinstance(x, (str, float)): {isinstance(x, (str, float))}") # 输出: isinstance(x, (str, float)): False
    ```

#### callable()

*   **中文说明**：检查一个对象是否可以被“调用”（如函数或方法）。
*   **示例代码**：
    ```python
    def my_function():
        pass

    x = 10
    print(f"Is my_function callable? {callable(my_function)}") # 输出: Is my_function callable? True
    print(f"Is x callable? {callable(x)}")                     # 输出: Is x callable? False
    ```

#### getattr(), setattr(), delattr()

*   **中文说明**：分别用于获取、设置和删除对象的属性。
*   **详细说明**：这些函数允许通过字符串名称动态地操作对象的属性，在元编程中非常有用。
*   **示例代码**：
    ```python
    class Person:
        name = "Alice"

    p = Person()

    # getattr()
    print(f"getattr(p, 'name'): {getattr(p, 'name')}") # 输出: getattr(p, 'name'): Alice

    # setattr()
    setattr(p, 'age', 30)
    print(f"After setattr, p.age is: {p.age}") # 输出: After setattr, p.age is: 30

    # delattr()
    delattr(p, 'name')
    try:
        print(p.name)
    except AttributeError as e:
        print(f"After delattr: {e}") # 输出: After delattr: 'Person' object has no attribute 'name'
    ```

### 6. 字符与编码

#### chr(), ord()

*   **中文说明**：`ord()` 将字符转为其对应的Unicode码点（整数），`chr()` 将Unicode码点转为字符。
*   **示例代码**：
    ```python
    # ord()
    print(f"ord('A'): {ord('A')}") # 输出: ord('A'): 65
    print(f"ord('中'): {ord('中')}") # 输出: ord('中'): 20013

    # chr()
    print(f"chr(66): '{chr(66)}'")   # 输出: chr(66): 'B'
    print(f"chr(22269): '{chr(22269)}'") # 输出: chr(22269): '国'
    ```

#### bin(), hex(), oct()

*   **中文说明**：将一个整数转换为其二进制、十六进制和八进制的字符串表示。
*   **示例代码**：
    ```python
    num = 42
    print(f"bin(42): {bin(42)}") # 输出: bin(42): 0b101010
    print(f"hex(42): {hex(42)}") # 输出: hex(42): 0x2a
    print(f"oct(42): {oct(42)}") # 输出: oct(42): 0o52
    ```

#### bytes(), ascii()

*   **中文说明**：`bytes()` 创建一个字节序列对象，`ascii()` 返回对象的可打印字符串表示。
*   **详细说明**：
    *   `bytes()`: 用于处理二进制数据。
    *   `ascii()`: 对于非ASCII字符，会使用 `\` 进行转义。
*   **示例代码**：
    ```python
    # bytes()
    s = "你好"
    b = s.encode('utf-8') # 字符串必须先编码
    print(f"bytes object: {b}") # 输出: bytes object: b'\xe4\xbd\xa0\xe5\xa5\xbd'
    print(bytes([104, 101, 108, 108, 111])) # 输出: b'hello'

    # ascii()
    s_non_ascii = "你好 world"
    print(f"ascii(): {ascii(s_non_ascii)}") # 输出: ascii(): '\u4f60\u597d world'
    ```

### 7. 动态代码执行

#### exec()

*   **中文说明**：执行存储在字符串或对象中的Python代码。
*   **详细说明**：`exec()` 是一个强大的函数，但使用时需非常小心，因为它可能带来安全风险（执行恶意代码）。
*   **示例代码**：
    ```python
    code_str = """
    x = 10
    y = 20
    print(f'The sum from exec is: {x + y}')
    """
    exec(code_str) # 输出: The sum from exec is: 30
    ```

### 8. 文件操作

#### open()

*   **中文说明**：打开一个文件，返回一个文件对象。
*   **详细说明**：最常用的文件操作函数，通常与 `with` 语句结合使用，以确保文件被自动关闭。文件对象自身拥有 `read()`, `write()`, `close()` 等方法。
*   **示例代码**：
    ```python
    # 写入文件
    with open('test.txt', 'w', encoding='utf-8') as f:
        f.write("Hello, this is a test file.\n")
        f.write("你好，世界！\n")

    # 读取文件
    with open('test.txt', 'r', encoding='utf-8') as f:
        content = f.read()
        print("File content:\n" + content)
    ```

---

## 二、 常用数据类型方法

这些不是全局的内置函数，而是属于特定数据类型（如字符串、列表）的方法。

### 1. 字符串方法 (String Methods)

#### replace()

*   **中文说明**：返回一个新字符串，其中旧的子串被替换为新的子串。
*   **示例代码**：
    ```python
    text = "hello world, hello python"
    new_text = text.replace("hello", "hi")
    print(f"Original: '{text}'")
    print(f"Replaced: '{new_text}'") # 输出: Replaced: 'hi world, hi python'

    # 指定替换次数
    limited_replace = text.replace("hello", "hi", 1)
    print(f"Limited replace: '{limited_replace}'") # 输出: Limited replace: 'hi world, hello python'
    ```

#### strip()

*   **中文说明**：去除字符串首尾指定的字符（默认为空格或换行符）。
*   **示例代码**：
    ```python
    text = "   some spaces   "
    print(f"'{text.strip()}'") # 输出: 'some spaces'

    text2 = "---title---"
    print(f"'{text2.strip('-')}'") # 输出: 'title'
    ```

#### format()

*   **中文说明**：格式化字符串，将值插入到字符串的占位符中。
*   **详细说明**：虽然f-string (`f"{var}"`) 在现代Python中更常用，但 `format()` 方法在某些场景下仍然非常有用，例如当格式化模板预先定义时。
*   **示例代码**：
    ```python
    template = "Hello, {name}. You are {age} years old."
    formatted_str = template.format(name="Alice", age=30)
    print(formatted_str) # 输出: Hello, Alice. You are 30 years old.

    template2 = "Position 0: {0}, Position 1: {1}"
    print(template2.format("first", "second")) # 输出: Position 0: first, Position 1: second
    ```

### 2. 列表方法 (List Methods)

#### append(), extend()

*   **中文说明**：`append()` 在列表末尾添加单个元素，`extend()` 将一个可迭代对象中的所有元素添加到列表末尾。
*   **示例代码**：
    ```python
    my_list = [1, 2, 3]

    # append()
    my_list.append(4)
    print(f"After append(4): {my_list}") # 输出: After append(4): [1, 2, 3, 4]

    # extend()
    another_list = [5, 6]
    my_list.extend(another_list)
    print(f"After extend([5, 6]): {my_list}") # 输出: After extend([5, 6]): [1, 2, 3, 4, 5, 6]
    ```

#### insert()

*   **中文说明**：在列表的指定索引位置插入一个元素。
*   **示例代码**：
    ```python
    my_list = ['a', 'b', 'd']
    my_list.insert(2, 'c') # 在索引2处插入'c'
    print(f"After insert: {my_list}") # 输出: After insert: ['a', 'b', 'c', 'd']
    ```

#### remove(), pop()

*   **中文说明**：`remove()` 移除列表中第一个匹配的元素，`pop()` 移除并返回指定索引的元素（默认为最后一个）。
*   **示例代码**：
    ```python
    my_list = ['a', 'b', 'c', 'b', 'd']

    # remove()
    my_list.remove('b') # 移除第一个 'b'
    print(f"After remove('b'): {my_list}") # 输出: After remove('b'): ['a', 'c', 'b', 'd']

    # pop()
    popped_item = my_list.pop(2) # 移除索引2的元素 'b'
    print(f"Popped item: {popped_item}, List after pop: {my_list}") # 输出: Popped item: b, List after pop: ['a', 'c', 'd']
    ```

#### index(), count()

*   **中文说明**：`index()` 返回元素在列表中首次出现的索引，`count()` 返回元素在列表中出现的总次数。
*   **示例代码**：
    ```python
    my_list = [1, 2, 3, 2, 4, 2]
    print(f"Index of 3: {my_list.index(3)}") # 输出: Index of 3: 2
    print(f"Count of 2: {my_list.count(2)}") # 输出: Count of 2: 3
    ```

#### sort(), reverse()

*   **中文说明**：`sort()` 对列表进行**原地**排序，`reverse()` 将列表中的元素**原地**反转。
*   **详细说明**：这两个方法直接修改原始列表，且返回值为 `None`。这与返回新对象的内置函数 `sorted()` 和 `reversed()` 不同。
*   **示例代码**：
    ```python
    numbers = [3, 1, 4, 1, 5, 9, 2]

    # sort()
    numbers.sort() # 原地升序排序
    print(f"After sort(): {numbers}") # 输出: After sort(): [1, 1, 2, 3, 4, 5, 9]

    # reverse()
    numbers.reverse() # 原地反转
    print(f"After reverse(): {numbers}") # 输出: After reverse(): [9, 5, 4, 3, 2, 1, 1]
    ```

---

## 三、 标准库常用函数

这些函数不是内置的，需要从Python的标准库中导入后才能使用。

#### functools.reduce()

*   **中文说明**：使用一个二元函数对序列中的元素进行累积计算。
*   **详细说明**：在Python 3中，`reduce()` 已被移至 `functools` 模块。它将一个函数（接收两个参数）从左到右应用到序列的元素上，以便将序列缩减为单个值。
*   **示例代码**：
    ```python
    from functools import reduce

    numbers = [1, 2, 3, 4]
    def add(x, y):
        return x + y

    # 计算 1+2+3+4
    sum_val = reduce(add, numbers)
    print(f"Sum with reduce: {sum_val}") # 输出: Sum with reduce: 10
    ```

#### random.random()

*   **中文说明**：生成一个 `[0.0, 1.0)` 范围内的随机浮点数。
*   **示例代码**：
    ```python
    import random

    # 生成一个随机浮点数
    random_float = random.random()
    print(f"Random float: {random_float}")

    # 生成 10 到 20 之间的一个随机整数
    random_int = random.randint(10, 20)
    print(f"Random integer between 10 and 20: {random_int}")
    ```

#### time.sleep()

*   **中文说明**：让程序暂停执行指定的秒数。
*   **示例代码**：
    ```python
    import time

    print("Program started.")
    time.sleep(2) # 暂停2秒
    print("Program resumed after 2 seconds.")
    ```

#### os.listdir()

*   **中文说明**：返回指定目录下的所有文件和文件夹名称的列表。
*   **示例代码**：
    ```python
    import os

    # 列出当前目录下的内容
    # 注意：结果取决于你的文件系统和当前工作目录
    try:
        current_dir_contents = os.listdir('.')
        print(f"Contents of current directory: {current_dir_contents}")
    except FileNotFoundError:
        print("Directory not found.")
    ```