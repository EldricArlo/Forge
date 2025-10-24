# Areas_Of_Knowledge\Programming_Languages\Python\Python_Syntax_Handbook.md

## Python 语法宝典：从入门到精通

### 目录

1.  **[Python 基础语法](#-python-基础语法)**
    *   [注释](#-注释)
    *   [变量与常量](#-变量与常量)
    *   [数据类型](#-数据类型)
        *   [数字 (Number)](#-数字-number)
        *   [字符串 (String)](#-字符串-string)
        *   [布尔值 (Boolean)](#-布尔值-boolean)
        *   [空值 (None)](#-空值-none)
    *   [输入与输出](#-输入与输出)
    *   [运算符](#-运算符)
        *   [算术运算符](#-算术运算符)
        *   [比较运算符](#-比较运算符)
        *   [逻辑运算符](#-逻辑运算符)
        *   [赋值运算符](#-赋值运算符)
2.  **[Python 数据结构](#-python-数据结构)**
    *   [列表 (List)](#-列表-list)
    *   [元组 (Tuple)](#-元组-tuple)
    *   [字典 (Dictionary)](#-字典-dictionary)
    *   [集合 (Set)](#-集合-set)
3.  **[流程控制](#-流程控制)**
    *   [条件语句 (if...elif...else)](#-条件语句-ifelifelse)
    *   [for 循环](#-for-循环)
    *   [while 循环](#-while-循环)
    *   [break 和 continue 语句](#-break-和-continue-语句)
4.  **[函数](#-函数)**
    *   [函数的定义与调用](#-函数的定义与调用)
    *   [函数参数](#-函数参数)
        *   [位置参数](#-位置参数)
        *   [默认参数](#-默认参数)
        *   [可变参数](#-可变参数)
        *   [关键字参数](#-关键字参数)
    *   [返回值](#-返回值)
    *   [匿名函数 (Lambda)](#-匿名函数-lambda)
    *   [作用域](#-作用域)
5.  **[面向对象编程 (OOP)](#-面向对象编程-oop)**
    *   [类 (Class) 与对象 (Object)](#-类-class-与对象-object)
    *   [封装、继承和多态](#-封装继承和多态)
    *   [实例属性和类属性](#-实例属性和类属性)
    *   [实例方法、类方法和静态方法](#-实例方法类方法和静态方法)
6.  **[模块与包](#-模块与包)**
    *   [模块的导入与使用](#-模块的导入与使用)
    *   [包的组织与管理](#-包的组织与管理)
    *   [常用内置模块](#-常用内置模块)
7.  **[文件操作](#-文件操作)**
    *   [文件的打开与关闭](#-文件的打开与关闭)
    *   [文件的读写](#-文件的读写)
    *   [with 语句](#-with-语句)
8.  **[异常处理](#-异常处理)**
    *   [try...except...finally](#-tryexceptfinally)
    *   [抛出异常 (raise)](#-抛出异常-raise)
9.  **[高级特性](#-高级特性)**
    *   [切片 (Slice)](#-切片-slice)
    *   [列表推导式 (List Comprehensions)](#-列表推导式-list-comprehensions)
    *   [生成器 (Generators)](#-生成器-generators)
    *   [装饰器 (Decorators)](#-装饰器-decorators)

---

### **1. Python 基础语法**

Python 的语法简洁清晰，易于学习。 它使用缩进来组织代码块，通常使用 4 个空格作为一级缩进。

#### **注释**

*   **单行注释**: 以 `#` 开头，后面的内容都会被视为注释。
    ```python
    # 这是一个单行注释
    print("Hello, World!")
    ```
*   **多行注释**: 使用三个单引号 `'''` 或三个双引号 `"""` 将注释内容包围起来。
    ```python
    '''
    这是一个多行注释，
    可以跨越多行。
    '''
    """
    这也是一个多行注释。
    """
    print("Hello again!")
    ```

#### **变量与常量**

*   **变量**: 在 Python 中，你不需要显式声明变量的类型，变量的类型会根据赋给它的值自动确定。
    *   **命名规则**:
        *   变量名只能包含字母、数字和下划线。
        *   变量名不能以数字开头。
        *   变量名区分大小写。
    ```python
    name = "Alice"  # 字符串类型
    age = 30       # 整数类型
    height = 1.75  # 浮点数类型
    is_student = True # 布尔类型
    ```

*   **常量**: Python 没有像其他语言那样的 `const` 关键字来定义常量。通常，我们用全大写的变量名来表示一个常量，这是一种约定俗成的规范。
    ```python
    PI = 3.14159
    ```

#### **数据类型**

Python 中有多种内置的数据类型。

##### **数字 (Number)**

*   **整数 (int)**: 正整数、负整数和零。
*   **浮点数 (float)**: 带有小数点的数字。
*   **复数 (complex)**: 由实部和虚部组成，例如 `3 + 5j`。

##### **字符串 (String)**

由单引号 `'` 或双引号 `"` 包围的文本。 字符串是不可变的。
```python
message = "Python is fun!"
char = 'a'
```

##### **布尔值 (Boolean)**

只有 `True` 和 `False` 两个值，注意首字母大写。它们通常用于逻辑判断。
```python
is_active = True
has_error = False
```

##### **空值 (None)**

`None` 是一个特殊的数据类型，表示一个空对象。它不等于 `0`、`False` 或空字符串。

#### **输入与输出**

*   **输出**: 使用 `print()` 函数向控制台输出信息。
    ```python
    print("Hello, Python!") # 输出: Hello, Python!
    ```
*   **输入**: 使用 `input()` 函数从用户那里获取输入。`input()` 返回的是一个字符串。
    ```python
    user_name = input("请输入你的名字: ")
    print("你好, " + user_name)
    ```

#### **运算符**

##### **算术运算符**

| 运算符 | 描述       | 示例 (a=10, b=3) |
| :----- | :--------- | :--------------- |
| `+`    | 加法       | `a + b` = 13     |
| `-`    | 减法       | `a - b` = 7      |
| `*`    | 乘法       | `a * b` = 30     |
| `/`    | 除法       | `a / b` = 3.33   |
| `//`   | 整除       | `a // b` = 3     |
| `%`    | 取模（余数） | `a % b` = 1      |
| `**`   | 幂         | `a ** b` = 1000  |

##### **比较运算符**

| 运算符 | 描述           | 示例 (a=10, b=3) |
| :----- | :------------- | :--------------- |
| `==`   | 等于           | `a == b` is False |
| `!=`   | 不等于         | `a != b` is True  |
| `>`    | 大于           | `a > b` is True   |
| `<`    | 小于           | `a < b` is False  |
| `>=`   | 大于等于       | `a >= b` is True  |
| `<=`   | 小于等于       | `a <= b` is False |

##### **逻辑运算符**

| 运算符 | 描述                                | 示例 (x=True, y=False) |
| :----- | :---------------------------------- | :-------------------- |
| `and`  | 逻辑与：如果两个都为真，则结果为真 | `x and y` is False    |
| `or`   | 逻辑或：如果至少一个为真，则结果为真 | `x or y` is True      |
| `not`  | 逻辑非：反转逻辑状态                | `not x` is False      |

##### **赋值运算符**

| 运算符   | 示例      | 等价于      |
| :------- | :-------- | :---------- |
| `=`      | `c = a`   | `c = a`     |
| `+=`     | `c += a`  | `c = c + a` |
| `-=`     | `c -= a`  | `c = c - a` |
| `*=`     | `c *= a`  | `c = c * a` |
| `/=`     | `c /= a`  | `c = c / a` |

---

### **2. Python 数据结构**

Python 提供了多种强大的内置数据结构。

#### **列表 (List)**

列表是一个有序且可变的集合。 可以包含不同类型的元素。使用方括号 `[]` 定义。
```python
fruits = ["apple", "banana", "cherry"]
print(fruits[1])  # 输出: banana
fruits.append("orange") # 添加元素
fruits[0] = "avocado"   # 修改元素
```

#### **元组 (Tuple)**

元组是一个有序但不可变的集合。 使用圆括号 `()` 定义。
```python
coordinates = (10.0, 20.0)
print(coordinates[0]) # 输出: 10.0
# coordinates[0] = 5.0 # 这会引发错误
```

#### **字典 (Dictionary)**

字典是一个无序的键值对集合，其中的键必须是唯一的且不可变的。 使用花括号 `{}` 定义。
```python
person = {"name": "Alice", "age": 30}
print(person["name"]) # 输出: Alice
person["city"] = "New York" # 添加键值对
```

#### **集合 (Set)**

集合是一个无序且不包含重复元素的集合。 使用花括号 `{}` 或者 `set()` 函数定义。
```python
unique_numbers = {1, 2, 3, 2, 1}
print(unique_numbers) # 输出: {1, 2, 3}
unique_numbers.add(4)
```

---

### **3. 流程控制**

流程控制结构允许你根据不同的条件执行不同的代码块。

#### **条件语句 (if...elif...else)**

```python
score = 85
if score >= 90:
    print("优秀")
elif score >= 80:
    print("良好")
elif score >= 60:
    print("及格")
else:
    print("不及格")
```

#### **for 循环**

用于遍历序列（如列表、元组、字符串）中的项目。
```python
for fruit in ["apple", "banana", "cherry"]:
    print(fruit)
```
结合 `range()` 函数可以进行指定次数的循环：
```python
for i in range(5): # 循环 5 次，i 从 0 到 4
    print(i)
```

#### **while 循环**

只要条件为真，就会一直执行。
```python
count = 0
while count < 5:
    print(count)
    count += 1
```

#### **break 和 continue 语句**

*   `break`: 立即终止整个循环。
*   `continue`: 跳过当前循环的剩余部分，进入下一次循环。

---

### **4. 函数**

函数是组织好的、可重复使用的，用来实现单一或相关联功能的代码段。

#### **函数的定义与调用**

使用 `def` 关键字定义函数。
```python
def greet(name):
    """这是一个文档字符串，用于解释函数的功能。"""
    print(f"Hello, {name}!")

greet("Alice") # 调用函数
```

#### **函数参数**

##### **位置参数**

调用时按顺序传入的参数。
```python
def power(base, exponent):
    return base ** exponent
```

##### **默认参数**

在定义函数时可以为参数提供默认值。
```python
def greet(name, message="Hello"):
    print(f"{message}, {name}!")
```

##### **可变参数 (`*args`)**

允许你传入任意数量的位置参数，它们会被打包成一个元组。
```python
def sum_all(*numbers):
    total = 0
    for num in numbers:
        total += num
    return total
```

##### **关键字参数 (`**kwargs`)**

允许你传入任意数量的带关键字的参数，它们会被打包成一个字典。
```python
def display_info(**info):
    for key, value in info.items():
        print(f"{key}: {value}")
```

#### **返回值**

使用 `return` 语句从函数中返回值。一个函数可以没有 `return` 语句，这种情况下它默认返回 `None`。

#### **匿名函数 (Lambda)**

一种小型的、匿名的函数，使用 `lambda` 关键字定义。
```python
add = lambda a, b: a + b
print(add(3, 5)) # 输出: 8
```

#### **作用域**

*   **局部作用域 (Local)**: 在函数内部定义的变量。
*   **全局作用域 (Global)**: 在函数外部定义的变量。

---

### **5. 面向对象编程 (OOP)**

面向对象编程是一种编程范式，它使用“对象”来设计软件。

#### **类 (Class) 与对象 (Object)**

*   **类 (Class)**: 创建对象的蓝图或模板。
*   **对象 (Object)**: 类的实例。

```python
class Dog:
    # __init__ 是一个特殊的方法，称为构造函数，在创建新对象时自动调用
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def bark(self):
        print(f"{self.name} is barking!")

# 创建 Dog 类的实例（对象）
my_dog = Dog("Buddy", 3)
my_dog.bark()
```

#### **封装、继承和多态**

*   **封装**: 将数据（属性）和操作数据的代码（方法）捆绑在一起。
*   **继承**: 一个类（子类）可以继承另一个类（父类）的属性和方法。
*   **多态**: 不同的对象可以对相同的消息（方法调用）做出不同的响应。

#### **实例属性和类属性**

*   **实例属性**: 每个对象独有的属性。
*   **类属性**: 所有该类的实例共享的属性。

#### **实例方法、类方法和静态方法**

*   **实例方法**: 第一个参数是 `self`，指向实例本身。
*   **类方法**: 第一个参数是 `cls`，指向类本身，通常用 `@classmethod` 装饰器标识。
*   **静态方法**: 不依赖于类或实例的状态，通常用 `@staticmethod` 装饰器标识。

---

### **6. 模块与包**

#### **模块的导入与使用**

Python 代码可以保存在 `.py` 文件中，这些文件被称为模块。
```python
# a_module.py
def say_hello():
    print("Hello from the module!")

# main.py
import a_module
a_module.say_hello()

from a_module import say_hello
say_hello()
```

#### **包的组织与管理**

包是模块的集合，它是一个包含 `__init__.py` 文件的目录。

#### **常用内置模块**

*   `math`: 数学运算
*   `datetime`: 日期和时间处理
*   `json`: JSON 数据的编码和解码
*   `os`: 与操作系统交互

---

### **7. 文件操作**

#### **文件的打开与关闭**

使用 `open()` 函数来打开文件。
```python
file = open("my_file.txt", "w") # "w" 表示写入模式
# ... 操作文件 ...
file.close()
```

#### **文件的读写**

*   `file.read()`: 读取整个文件。
*   `file.readline()`: 读取一行。
*   `file.readlines()`: 读取所有行并返回一个列表。
*   `file.write()`: 写入字符串。

#### **with 语句**

使用 `with` 语句可以自动管理文件的关闭，即使发生错误也能保证文件被正确关闭，是推荐的文件操作方式。
```python
with open("my_file.txt", "r") as file:
    content = file.read()
    print(content)
```

---

### **8. 异常处理**

#### **try...except...finally**

用于处理代码执行过程中可能出现的错误。
```python
try:
    result = 10 / 0
except ZeroDivisionError:
    print("不能除以零！")
finally:
    print("无论如何都会执行。")
```

#### **抛出异常 (raise)**

可以使用 `raise` 关键字主动抛出一个异常。
```python
x = -1
if x < 0:
    raise ValueError("数值不能为负")
```

---

### **9. 高级特性**

Python 提供了许多高级语法，可以使代码更加简洁高效。

#### **切片 (Slice)**

用于获取序列（列表、元组、字符串）的一部分。
```python
my_list = [0, 1, 2, 3, 4, 5]
print(my_list[1:4]) # 输出: [1, 2, 3]
print(my_list[:3])  # 输出: [0, 1, 2]
print(my_list[::2]) # 输出: [0, 2, 4]
```

#### **列表推导式 (List Comprehensions)**

提供了一种简洁的方式来创建列表。
```python
squares = [x**2 for x in range(10)]
# 等价于:
# squares = []
# for x in range(10):
#     squares.append(x**2)
```

#### **生成器 (Generators)**

生成器是一种特殊的迭代器，它允许你按需生成值，而不是一次性生成所有值，从而节省内存。
```python
def my_generator():
    for i in range(5):
        yield i

for item in my_generator():
    print(item)
```

#### **装饰器 (Decorators)**

装饰器本质上是一个函数，它可以接受一个函数作为参数，并返回一个新的函数，用于在不修改原函数代码的情况下为其添加额外的功能。
```python
def my_decorator(func):
    def wrapper():
        print("在函数执行前做些什么。")
        func()
        print("在函数执行后做些什么。")
    return wrapper

@my_decorator
def say_whee():
    print("Whee!")

say_whee()
```