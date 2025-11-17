# 权威指南：Python 项目文件命名与结构的最佳实践

## 导航目录

- [权威指南：Python 项目文件命名与结构的最佳实践](#权威指南python-项目文件命名与结构的最佳实践)
  - [导航目录](#导航目录)
  - [1. 核心设计原则](#1-核心设计原则)
    - [1.1 清晰与可读性 (Clarity \& Readability)](#11-清晰与可读性-clarity--readability)
    - [1.2 一致性 (Consistency)](#12-一致性-consistency)
    - [1.3 遵循标准 (Follow Standards)](#13-遵循标准-follow-standards)
    - [1.4 模块化 (Modularity)](#14-模块化-modularity)
    - [1.5 分离关注点 (Separation of Concerns)](#15-分离关注点-separation-of-concerns)
    - [1.6 易于分发与安装 (Easy to Distribute \& Install)](#16-易于分发与安装-easy-to-distribute--install)
  - [2. 文件与代码命名规范 (PEP 8)](#2-文件与代码命名规范-pep-8)
    - [2.1 模块名 (Module Names)](#21-模块名-module-names)
    - [2.2 包名 (Package Names)](#22-包名-package-names)
    - [2.3 类名 (Class Names)](#23-类名-class-names)
    - [2.5 变量名 (Variable Names)](#25-变量名-variable-names)
    - [2.6 常量名 (Constant Names)](#26-常量名-constant-names)
    - [2.7 私有名称 (Private Names)](#27-私有名称-private-names)
    - [2.8 其他文件命名](#28-其他文件命名)
  - [3. 现代 Python 项目结构详解](#3-现代-python-项目结构详解)
    - [3.1 标准结构概览](#31-标准结构概览)
    - [3.2 根目录文件深度解析](#32-根目录文件深度解析)
      - [3.2.1 `README.md`](#321-readmemd)
      - [3.2.2 `.gitignore`](#322-gitignore)
      - [3.2.3 `LICENSE`](#323-license)
      - [3.2.4 `pyproject.toml` (现代标准)](#324-pyprojecttoml-现代标准)
      - [3.2.5 `setup.py` / `setup.cfg` (传统方式)](#325-setuppy--setupcfg-传统方式)
      - [3.2.6 `requirements.txt`](#326-requirementstxt)
    - [3.3 核心目录结构解析](#33-核心目录结构解析)
      - [3.3.1 `src/` 目录：推荐的源码布局](#331-src-目录推荐的源码布局)
      - [3.3.2 `my_package/`: 你的 Python 包](#332-my_package-你的-python-包)
      - [3.3.3 `tests/`: 测试代码](#333-tests-测试代码)
      - [3.3.4 `docs/`: 项目文档](#334-docs-项目文档)
      - [3.3.5 其他辅助目录 (`examples/`, `scripts/`, `data/`)](#335-其他辅助目录-examples-scripts-data)
  - [4. 虚拟环境：项目隔离的最佳实践](#4-虚拟环境项目隔离的最佳实践)
  - [5. 总结](#5-总结)

---

## 1. 核心设计原则

构建一个专业、可维护的 Python 项目，不仅仅是编写有效的代码，更在于如何组织这些代码。以下六大核心原则是构建清晰、健壮项目的基石。

### 1.1 清晰与可读性 (Clarity & Readability)
文件名和目录结构应如同一本书的目录，直观地反映其内容和用途，让任何开发者都能迅速理解项目的架构。

### 1.2 一致性 (Consistency)
在整个项目中，从变量命名到目录布局，都应采用统一的约定和模式。这极大地降低了团队成员的学习成本和维护难度。

### 1.3 遵循标准 (Follow Standards)
遵守 Python 社区公认的规范，如 **PEP 8**（代码风格指南）和 **PyPA**（Python Packaging Authority）的打包建议，能确保你的项目与广阔的 Python 生态系统无缝对接。

### 1.4 模块化 (Modularity)
将庞大的代码库拆分为功能独立、高内聚、低耦合的模块和包。这不仅使代码更易于管理和复用，还有助于并行开发。

### 1.5 分离关注点 (Separation of Concerns)
将项目的不同部分——如源代码、测试、文档、配置文件、脚本——隔离在各自专属的目录中。这种分离使得项目结构井然有序，易于定位和维护。

### 1.6 易于分发与安装 (Easy to Distribute & Install)
项目结构应原生支持标准的 Python 打包工具（如 Poetry, Hatch, PDM, 或传统的 setuptools），使得其他人可以通过简单的 `pip install` 命令来安装和使用你的项目。

## 2. 文件与代码命名规范 (PEP 8)

遵循 PEP 8 是 Python 开发的黄金标准。以下是关键的命名实践，并附有代码示例。

### 2.1 模块名 (Module Names)
- **规则**: 使用简短、全小写的单词，并用下划线 `_` 分隔（snake\_case）。
- **禁止**: **绝对不要**使用连字符 `-`，这会导致 `SyntaxError`。
- **示例**: `data_processor.py`, `http_utils.py`

```python
# data_processor.py

def clean_data(raw_data):
    """Cleans the input data."""
    # ... implementation ...
    return cleaned_data
```

### 2.2 包名 (Package Names)
- **规则**: 与模块名类似，使用简短、全小写的单词，用下划线 `_` 分隔。
- **禁止**: 同样**禁止**使用连字符 `-`。
- **示例**: `my_project`, `data_utils`, `web_app`

### 2.3 类名 (Class Names)
- **规则**: 使用驼峰命名法 (CamelCase)，所有单词首字母大写，不使用分隔符。
- **示例**: `MyClass`, `HttpRequestParser`, `DatabaseConnection`

```python
# http_utils.py

class HttpRequestParser:
    def __init__(self, request_string):
        self.raw_request = request_string

    def parse_headers(self):
        """Parses headers from the raw request."""
        # ... implementation ...
        return headers```

### 2.4 函数名/方法名 (Function/Method Names)
- **规则**: 使用小写字母和下划线 `_` (snake\_case)。
- **示例**: `calculate_total()`, `get_user_by_id()`

```python
# data_processor.py

def calculate_average_price(price_list):
    """Calculates the average price from a list of numbers."""
    if not price_list:
        return 0.0
    return sum(price_list) / len(price_list)
```

### 2.5 变量名 (Variable Names)
- **规则**: 与函数名相同，使用小写字母和下划线 `_` (snake\_case)。
- **示例**: `user_name`, `item_count`, `total_amount`

```python
# main.py

user_name = "Alice"
item_count = 10
price_per_item = 5.50

total_amount = item_count * price_per_item
print(f"User: {user_name}, Total: {total_amount}")
```

### 2.6 常量名 (Constant Names)
- **规则**: 使用全大写字母和下划线 `_` (UPPER\_CASE)。
- **示例**: `MAX_RETRIES`, `DEFAULT_TIMEOUT`, `API_KEY`

```python
# config.py

MAX_RETRIES = 3
DEFAULT_TIMEOUT = 10  # in seconds
API_KEY = "your-secret-api-key-here"
```

### 2.7 私有名称 (Private Names)
- **弱私有 (Internal Use)**: 以单个下划线 `_` 开头，这是一种约定，告诉其他开发者这个变量或方法是内部使用的，不应在外部直接访问。
- **名称重整 (Name Mangling)**: 以两个下划线 `__` 开头，Python 解释器会改变其名称，以避免在子类中被意外覆盖。应谨慎使用。

```python
class MyService:
    def __init__(self):
        self.public_data = "Public"
        self._internal_data = "Internal use only"
        self.__mangled_data = "Strongly private"

    def public_method(self):
        print("This is a public method.")

    def _internal_method(self):
        print("This method is for internal use.")
```

### 2.8 其他文件命名
- **规则**: 对于非 Python 代码文件（如配置、文档、脚本），通常使用小写字母，并可选择使用下划线 `_` 或连字符 `-` (kebab-case)。在项目中保持一致性是关键。
- **示例**: `requirements.txt`, `.gitignore`, `README.md`, `config.yaml`, `run-tests.sh`

## 3. 现代 Python 项目结构详解

一个精心设计的项目结构是项目成功的关键。以下是当前社区推荐的，兼顾开发、测试和分发的标准项目结构。

### 3.1 标准结构概览
```
my-awesome-project/      # 项目根目录 (仓库名, 推荐 kebab-case)
├── .gitignore           # Git 忽略规则
├── LICENSE              # 项目许可证
├── README.md            # 项目介绍、安装和使用指南 (至关重要!)
├── pyproject.toml       # [推荐] 现代打包与项目元数据标准
├── setup.py             # [传统] 打包脚本 (与 pyproject.toml 可共存或二选一)
│
├── src/                 # [推荐] 源码根目录
│   └── my_package/      # Python 包 (包名, 推荐 snake_case)
│       ├── __init__.py  # 包初始化文件，可定义公共 API
│       ├── main.py      # 主模块
│       ├── utils.py     # 工具函数模块
│       └── subpackage/  # 子包
│           ├── __init__.py
│           └── helper.py
│
├── tests/               # 测试代码目录
│   ├── __init__.py      # 标识为 Python 包
│   ├── test_main.py     # `main.py` 的单元测试
│   └── test_utils.py    # `utils.py` 的单元测试
│
├── docs/                # 文档目录 (例如使用 Sphinx 或 MkDocs)
│   ├── conf.py          # 文档生成器配置
│   └── index.rst        # 文档首页
│
├── examples/            # 示例代码
│   └── simple_usage.py
│
└── scripts/             # 辅助脚本 (部署、数据迁移等)
    └── deploy.sh
```

### 3.2 根目录文件深度解析

#### 3.2.1 `README.md`
项目的门面。一个优秀的 `README.md` 应该包含：
- **项目标题和简介**: 它是什么，解决了什么问题。
- **安装指南**: 如何安装项目及其依赖。
- **快速入门**: 一个简单的使用示例。
- **API 文档链接**: 如果有的话。
- **如何运行测试**: 指导贡献者验证他们的更改。
- **贡献指南**: 如何为项目做出贡献。

#### 3.2.2 `.gitignore`
告诉 Git 哪些文件或目录不应被纳入版本控制。一个好的起点是使用 [GitHub 的 Python .gitignore 模板](https://github.com/github/gitignore/blob/main/Python.gitignore)。
```gitignore
# Virtual Environment
.venv/
venv/
env/

# Python cache
__pycache__/
*.pyc

# Build artifacts
build/
dist/
*.egg-info/

# IDE files
.idea/
.vscode/
```

#### 3.2.3 `LICENSE`
明确项目的开源许可证（如 MIT, Apache 2.0, GPL）。如果没有许可证，代码在法律上是专有的。

#### 3.2.4 `pyproject.toml` (现代标准)
这是 PEP 518, 517, 621 定义的现代化 Python 项目配置文件。它是一个中心化的文件，用于声明：
- **项目元数据**: 名称、版本、作者等。
- **构建系统**: 使用哪个工具（如 Poetry, Hatch, PDM, setuptools）来打包项目。
- **项目依赖**: 替代 `requirements.txt`。
- **工具配置**: 如 `pytest`, `black`, `ruff` 等工具的配置。

**示例 (`pyproject.toml` 使用 Hatch)**:
```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project]
name = "my_awesome_project"
version = "0.1.0"
authors = [
  { name="Your Name", email="you@example.com" },
]
description = "A small example package"
requires-python = ">=3.8"
dependencies = [
    "requests>=2.20.0",
    "pandas",
]

[project.urls]
"Homepage" = "https://github.com/user/my-awesome-project"
```

#### 3.2.5 `setup.py` / `setup.cfg` (传统方式)
在 `pyproject.toml` 普及之前，`setup.py` 是打包的标准。虽然现代工具正在取代它，但在一些旧项目或特定场景下仍然可见。

**示例 (`setup.py`)**:
```python
from setuptools import setup, find_packages

setup(
    name="my_awesome_project",
    version="0.1.0",
    packages=find_packages(where="src"),
    package_dir={"": "src"},
    install_requires=[
        "requests>=2.20.0",
        "pandas",
    ],
)
```

#### 3.2.6 `requirements.txt`
用于声明项目的依赖项。虽然 `pyproject.toml` 正在成为新的标准，但 `requirements.txt` 仍然在许多场景下（特别是应用程序部署）被广泛使用。

**示例 (`requirements.txt`)**:
```
requests==2.28.1
pandas==1.5.3
# 使用 'pip freeze > requirements.txt' 生成固定版本
```

### 3.3 核心目录结构解析

#### 3.3.1 `src/` 目录：推荐的源码布局
将所有 Python 包代码放入 `src/` 目录下是一种被广泛推荐的最佳实践。
- **优点**:
    1.  **清晰分离**: 明确地将你的可安装代码与项目根目录下的其他文件（测试、配置、文档）分开。
    2.  **避免路径问题**: 防止在开发时意外地导入了本地文件而不是已安装的包版本，这使得测试更加可靠。
    3.  **符合 PyPA 推荐**: 这是 Python 打包官方机构推荐的现代项目布局。

#### 3.3.2 `my_package/`: 你的 Python 包
- **`__init__.py`**: 这个文件的存在告诉 Python 该目录是一个包。它可以是空的，也可以用来执行包级别的初始化或定义包的公共 API。

**示例 (`src/my_package/__init__.py`)**：
```python
# 让用户可以直接通过 'from my_package import process_data' 来导入
# 而不是 'from my_package.main import process_data'

from .main import process_data
from .utils import helper_function

__version__ = "0.1.0"

# 控制 'from my_package import *' 的行为
__all__ = ["process_data", "helper_function"]
```

#### 3.3.3 `tests/`: 测试代码
- 存放所有测试代码，其内部结构通常镜像 `src/` 目录。
- 测试框架（如 `pytest`）会自动发现并运行此目录下的测试。

**示例 (`tests/test_utils.py` 使用 pytest)**:
```python
# 导入被测试的函数
from my_package.utils import helper_function

def test_helper_function():
    """测试 helper_function 的基本功能。"""
    assert helper_function(2, 3) == 5
    assert helper_function(-1, 1) == 0
```

#### 3.3.4 `docs/`: 项目文档
使用 Sphinx 或 MkDocs 等工具生成专业文档的源文件存放地。

#### 3.3.5 其他辅助目录 (`examples/`, `scripts/`, `data/`)
- `examples/`: 提供清晰的代码示例，展示如何使用你的库。
- `scripts/`: 存放与项目管理相关的脚本，如数据库迁移、部署自动化等。
- `data/`: 存放项目所需的数据文件（如果适用）。

## 4. 虚拟环境：项目隔离的最佳实践

虽然虚拟环境不是项目结构的一部分，但它是项目开发的基础。为每个项目创建独立的虚拟环境可以避免不同项目间的依赖冲突。

**创建和激活虚拟环境**:
```bash
# 1. 在项目根目录下创建虚拟环境 (推荐命名为 .venv)
python -m venv .venv

# 2. 激活虚拟环境
# On Windows
# .\.venv\Scripts\activate
# On macOS/Linux
source .venv/bin/activate

# 3. 安装依赖
pip install -r requirements.txt
# 或者使用现代工具
# poetry install
# pdm install
```
记得将你的虚拟环境目录（如 `.venv/`）添加到 `.gitignore` 文件中。

## 5. 总结

遵循这些经过社区检验的命名和结构化实践，将为你的 Python 项目带来巨大的好处：
- **对于个人**: 提高开发效率，让项目在数月或数年后依然易于理解和维护。
- **对于团队**: 建立统一的协作标准，降低沟通成本，使代码审查更加高效。
- **对于社区**: 使你的项目易于被他人理解、使用和贡献，从而提升其影响力。

一个专业、结构清晰的项目不仅是代码的集合，更是你专业精神和工程能力的体现。