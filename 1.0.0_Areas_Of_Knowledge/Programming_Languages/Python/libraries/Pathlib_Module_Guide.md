# Areas_Of_Knowledge\Programming_Languages\Python\libraries\Pathlib_Module_Guide.md

## Python `pathlib` 库：现代、优雅、跨平台的文件系统路径操作指南

Python `pathlib` 模块，自 Python 3.4 起成为标准库的一员，提供了一种面向对象的方式来处理文件系统路径。 它从根本上改变了以往将路径作为纯字符串处理的方式，使得代码在不同操作系统间更具可移植性、可读性和安全性。 本文将为您详细、深刻、清晰地解读 `pathlib` 库的语法和常用功能，并提供一个支持跳转的完整目录，帮助您快速掌握这个强大的工具。

### **目录**

1.  [**`pathlib` 入门：告别字符串拼接**](#part-1)
    *   [1.1. 为什么选择 `pathlib`？](<#part-1-1>)
    *   [1.2. 核心类：`Path` 对象简介](<#part-1-2>)
2.  [**路径对象的创建**](#part-2)
    *   [2.1. 从字符串创建路径](<#part-2-1>)
    *   [2.2. 获取特殊路径](<#part-2-2>)
    *   [2.3. 路径的拼接：优雅的 `/` 运算符](<#part-2-3>)
3.  [**路径属性解析：轻松获取路径的各个部分**](#part-3)
    *   [3.1. 文件名、词干和后缀](<#part-3-1>)
    *   [3.2. 父目录与祖先目录](<#part-3-2>)
    *   [3.3. 其他重要属性](<#part-3-3>)
4.  [**文件和目录操作**](#part-4)
    *   [4.1. 检查路径存在性和类型](<#part-4-1>)
    *   [4.2. 读写文件](<#part-4-2>)
    *   [4.3. 创建文件和目录](<#part-4-3>)
    *   [4.4. 移动、重命名和删除](<#part-4-4>)
5.  [**遍历目录：`iterdir`, `glob`, 和 `rglob`**](#part-5)
    *   [5.1. `iterdir()`：遍历当前目录](<#part-5-1>)
    *   [5.2. `glob()`：使用通配符进行非递归匹配](<#part-5-2>)
    *   [5.3. `rglob()`：递归匹配所有子目录](<#part-5-3>)
6.  [**深入理解：Pure Paths vs. Concrete Paths**](#part-6)
    *   [6.1. Pure Paths：纯计算路径操作](<#part-6-1>)
    *   [6.2. Concrete Paths：与操作系统交互](<#part-6-2>)
    *   [6.3. 跨平台路径处理](<#part-6-3>)
7.  [**总结**](#part-7)

---

### <a name="part-1"></a>**1. `pathlib` 入门：告别字符串拼接**

#### <a name="part-1-1"></a>**1.1. 为什么选择 `pathlib`？**

在 `pathlib` 出现之前，Python 开发者通常使用 `os.path` 模块来处理文件路径，这依赖于大量的字符串拼接和函数调用。 这种方式存在一些问题：

*   **可读性差**：`os.path.join('dir1', 'dir2', 'file.txt')` 这样的代码相对冗长。
*   **容易出错**：手动拼接字符串时，很容易忘记或用错路径分隔符（在 Windows 上是 `\`，在 Unix-like 系统上是 `/`）。
*   **跨平台兼容性问题**：需要开发者自己注意不同操作系统的路径差异。

`pathlib` 模块通过提供一个面向对象的接口，优雅地解决了这些问题。 它将文件系统路径抽象成对象，让路径操作更加直观和安全。

#### <a name="part-1-2"></a>**1.2. 核心类：`Path` 对象简介**

`pathlib` 模块的核心是 `Path` 类。 当你实例化 `Path` 类时，它会根据你当前的操作系统自动创建一个 `PosixPath` (在 Unix-like 系统上) 或 `WindowsPath` (在 Windows 系统上) 的实例。 这种设计使得你的代码无需修改就能在不同平台上运行。

要开始使用，首先需要从 `pathlib` 模块导入 `Path` 类：

```python
from pathlib import Path
```

---

### <a name="part-2"></a>**2. 路径对象的创建**

#### <a name="part-2-1"></a>**2.1. 从字符串创建路径**

最基本的创建 `Path` 对象的方式是向其构造函数传入一个字符串路径。

```python
# 创建一个指向文件的相对路径
file_path = Path("data/union_data.csv")

# 创建一个指向目录的绝对路径 (Unix-like)
dir_path = Path("/home/user/documents")
```

#### <a name="part-2-2"></a>**2.2. 获取特殊路径**

`Path` 类提供了一些便捷的类方法来获取常用路径：

*   **`Path.cwd()`**: 获取当前工作目录。
*   **`Path.home()`**: 获取用户的主目录。

```python
# 获取当前工作目录
current_directory = Path.cwd()
print(f"当前工作目录: {current_directory}")

# 获取用户主目录
home_directory = Path.home()
print(f"用户主目录: {home_directory}")
```

#### <a name="part-2-3"></a>**2.3. 路径的拼接：优雅的 `/` 运算符**

`pathlib` 重载了斜杠 `/` 运算符，使其可以像拼接 URL 一样直观地拼接路径，这极大地提高了代码的可读性。

```python
# 使用 / 运算符拼接路径
config_path = Path.home() / "documents" / "settings.ini"
print(config_path)

# 也可以和字符串混合使用
data_file = Path("data") / "raw" / "measurements.csv"
print(data_file)
```

---

### <a name="part-3"></a>**3. 路径属性解析：轻松获取路径的各个部分**

`Path` 对象提供了丰富的属性，可以方便地提取路径的各个组成部分。

#### <a name="part-3-1"></a>**3.1. 文件名、词干和后缀**

*   **.name**: 获取路径的最后一部分，即文件名或目录名。
*   **.stem**: 获取文件名中不包含后缀的部分。
*   **.suffix**: 获取文件的扩展名（后缀）。

```python
p = Path("/home/user/data/report.pdf")

print(f"完整名称: {p.name}")  # 输出: report.pdf
print(f"词干 (不含后缀): {p.stem}")   # 输出: report
print(f"后缀: {p.suffix}")   # 输出: .pdf
```

#### <a name="part-3-2"></a>**3.2. 父目录与祖先目录**

*   **.parent**: 获取路径的直接父目录。
*   **.parents**: 返回一个包含所有祖先目录的序列。

```python
p = Path("/home/user/data/report.pdf")

print(f"父目录: {p.parent}")  # 输出: /home/user/data

# 遍历所有祖先目录
for parent in p.parents:
    print(parent)
# 输出:
# /home/user/data
# /home/user
# /home
# /
```

#### <a name="part-3-3"></a>**3.3. 其他重要属性**

*   **.anchor**: 获取路径的锚点（例如，在 Unix 上是 `/`，在 Windows 上是 `C:\`）。
*   **.is_absolute()**: 判断路径是否为绝对路径。

```python
p_win = Path("C:/Users/Test/file.txt")
print(f"锚点: {p_win.anchor}")  # 输出: C:\
print(f"是否为绝对路径: {p_win.is_absolute()}")  # 输出: True

p_nix = Path("data/file.txt")
print(f"锚点: {p_nix.anchor}")  # 输出:
print(f"是否为绝对路径: {p_nix.is_absolute()}")  # 输出: False
```

---

### <a name="part-4"></a>**4. 文件和目录操作**

`Path` 对象不仅可以表示路径，还能直接进行文件系统的操作。

#### <a name="part-4-1"></a>**4.1. 检查路径存在性和类型**

*   **.exists()**: 检查路径指向的文件或目录是否存在。
*   **.is_dir()**: 检查路径是否为一个目录。
*   **.is_file()**: 检查路径是否为一个文件。

```python
p = Path("my_project")

if p.exists():
    if p.is_dir():
        print(f"'{p}' 是一个存在的目录。")
    elif p.is_file():
        print(f"'{p}' 是一个存在的文件。")
else:
    print(f"路径 '{p}' 不存在。")
```

#### <a name="part-4-2"></a>**4.2. 读写文件**

`pathlib` 提供了便捷的方法来读写文件，无需手动 `open()` 和 `close()`。

*   **.read_text(encoding=None, errors=None)**: 以文本模式读取文件内容并返回字符串。
*   **.write_text(data, encoding=None, errors=None)**: 将字符串写入文本文件。
*   **.read_bytes()**: 以二进制模式读取文件内容并返回字节串。
*   **.write_bytes(data)**: 将字节串写入二进制文件。

```python
p = Path("greeting.txt")

# 写入文本
p.write_text("Hello, pathlib!")

# 读取文本
content = p.read_text()
print(content)  # 输出: Hello, pathlib!
```

#### <a name="part-4-3"></a>**4.3. 创建文件和目录**

*   **.mkdir(mode=0o777, parents=False, exist_ok=False)**: 创建一个新目录。
    *   `parents=True`：可以同时创建不存在的父目录。
    *   `exist_ok=True`：如果目录已存在，不会抛出错误。
*   **.touch(mode=0o666, exist_ok=True)**: 创建一个空文件，类似于 Unix 的 `touch` 命令。

```python
# 创建一个新目录
new_dir = Path("my_new_directory")
new_dir.mkdir(exist_ok=True)

# 创建多级目录
nested_dir = Path("parent_dir/child_dir")
nested_dir.mkdir(parents=True, exist_ok=True)

# 创建一个空文件
empty_file = new_dir / "empty.txt"
empty_file.touch()
```

#### <a name="part-4-4"></a>**4.4. 移动、重命名和删除**

*   **.rename(target)**: 重命名或移动文件/目录到 `target`。
*   **.replace(target)**: 与 `rename` 类似，但在 Windows 上如果目标存在，它将无条件替换。
*   **.unlink(missing_ok=False)**: 删除一个文件。
*   **.rmdir()**: 删除一个空目录。

```python
p = Path("old_name.txt")
p.touch()

# 重命名文件
target = Path("new_name.txt")
p.rename(target)

# 创建一个目录并移动文件进去
move_dir = Path("archive")
move_dir.mkdir(exist_ok=True)
target.rename(move_dir / target.name)

# 删除文件
(move_dir / "new_name.txt").unlink()

# 删除空目录
move_dir.rmdir()
```

---

### <a name="part-5"></a>**5. 遍历目录：`iterdir`, `glob`, 和 `rglob`**

`pathlib` 提供了强大的工具来遍历目录内容。

#### <a name="part-5-1"></a>**5.1. `iterdir()`：遍历当前目录**

`.iterdir()` 方法返回一个迭代器，包含路径下所有直接的子文件和子目录。

```python
p = Path.cwd()

for item in p.iterdir():
    if item.is_dir():
        print(f"目录: {item.name}")
    else:
        print(f"文件: {item.name}")
```

#### <a name="part-5-2"></a>**5.2. `glob()`：使用通配符进行非递归匹配**

`.glob(pattern)` 方法根据给定的通配符模式，在当前目录中查找匹配项。

```python
# 查找当前目录下所有的 .py 文件
for py_file in Path.cwd().glob("*.py"):
    print(py_file)
```

#### <a name="part-5-3"></a>**5.3. `rglob()`：递归匹配所有子目录**

`.rglob(pattern)` 方法与 `.glob()` 类似，但它会递归地搜索所有子目录。

```python
# 递归查找项目目录下所有的 .txt 文件
project_dir = Path("/path/to/your/project")
for txt_file in project_dir.rglob("*.txt"):
    print(txt_file)
```

---

### <a name="part-6"></a>**6. 深入理解：Pure Paths vs. Concrete Paths**

`pathlib` 的类层次结构分为两类：**纯路径 (Pure Paths)** 和 **具体路径 (Concrete Paths)**。

#### <a name="part-6-1"></a>**6.1. Pure Paths：纯计算路径操作**

`PurePath`、`PurePosixPath` 和 `PureWindowsPath` 这些类只进行路径字符串的计算操作，而不与实际的文件系统交互。 这意味着你可以在一个操作系统上操作另一种操作系统的路径格式，而不会触发任何 I/O 操作。

```python
from pathlib import PureWindowsPath

# 在 Linux/macOS 上操作 Windows 路径
win_path = PureWindowsPath("C:\\Users\\Guest\\Documents")
print(win_path / "data.csv")  # 输出: C:\Users\Guest\Documents\data.csv
```

#### <a name="part-6-2"></a>**6.2. Concrete Paths：与操作系统交互**

`Path`、`PosixPath` 和 `WindowsPath` 这些类继承自对应的 Pure Path 类，并添加了与文件系统交互的方法（如 `.exists()`, `.read_text()`, `.mkdir()` 等）。 当你需要在代码中实际读写或操作文件系统时，应该使用这些具体路径类。

#### <a name="part-6-3"></a>**6.3. 跨平台路径处理**

`pathlib` 的一个核心优势是跨平台兼容性。 `Path` 对象内部会自动处理不同操作系统的路径分隔符。例如，即使在 Windows 上，`pathlib` 在表示路径时也倾向于使用正斜杠 `/`，这被称为 POSIX 风格，但在进行实际的系统调用时，它会转换为正确的平台特定分隔符。

```python
# 这段代码在 Windows, Linux, macOS 上都能正确工作
from pathlib import Path

path = Path("config") / "app.ini"
print(path)  # 在所有系统上都可能显示为 config/app.ini
```

---

### <a name="part-7"></a>**7. 总结**

`pathlib` 模块为 Python 中的文件系统路径操作带来了革命性的变化。它用清晰、直观、面向对象的方式取代了传统的字符串操作，显著提升了代码的可读性、健壮性和跨平台兼容性。从简单的路径拼接到复杂的文件系统操作和遍历，`pathlib` 都提供了强大而优雅的解决方案。掌握 `pathlib`，将使你的文件操作代码提升到一个新的水平。