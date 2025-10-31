# Python 包发布完全指南：从代码到 PyPI

## 目录

- [Python 包发布完全指南：从代码到 PyPI](#python-包发布完全指南从代码到-pypi)
  - [目录](#目录)
  - [1. 核心流程概览](#1-核心流程概览)
  - [2. 准备工作 (Prerequisites)](#2-准备工作-prerequisites)
    - [注册 PyPI 和 TestPyPI 账户](#注册-pypi-和-testpypi-账户)
    - [API Token：推荐的认证方式](#api-token推荐的认证方式)
    - [安装必要的打包工具](#安装必要的打包工具)
  - [3. 项目结构 (Project Structure)](#3-项目结构-project-structure)
  - [4. 配置打包文件 (`pyproject.toml`) 详解](#4-配置打包文件-pyprojecttoml-详解)
    - [第二部分：`[project]`（项目元数据）](#第二部分project项目元数据)
    - [第三部分：`[project.dependencies]`（项目依赖）](#第三部分projectdependencies项目依赖)
  - [5. 构建分发包 (Building Distributions)](#5-构建分发包-building-distributions)
    - [构建命令](#构建命令)
    - [生成的包类型](#生成的包类型)
  - [6. 上传至 TestPyPI (演练与安全)](#6-上传至-testpypi-演练与安全)
    - [上传命令](#上传命令)
    - [使用 API Token 认证](#使用-api-token-认证)
    - [验证 TestPyPI 安装](#验证-testpypi-安装)
  - [7. 上传至官方 PyPI (正式发布)](#7-上传至官方-pypi-正式发布)
    - [正式上传命令](#正式上传命令)
    - [最终验证](#最终验证)
  - [8. 发布新版本与更新 (Updating)](#8-发布新版本与更新-updating)
    - [版本号的重要性](#版本号的重要性)
    - [更新步骤](#更新步骤)
    - [语义化版本 (Semantic Versioning)](#语义化版本-semantic-versioning)
  - [9. 速查表 (Quick Reference)](#9-速查表-quick-reference)

---

## 1. 核心流程概览

将 Python 包发布到 PyPI（Python Package Index）是一个标准化的过程，目的是让全球的开发者都能通过 `pip install` 命令方便地安装和使用你的代码。

| 步骤 | 目的 | 使用工具/文件 |
| :--- | :--- | :--- |
| **准备** | 账户注册与工具安装 | `PyPI`, `TestPyPI`, `build`, `twine` |
| **结构** | 组织源代码和文档 | `src/`, `README.md`, `LICENSE` |
| **配置** | 定义项目元数据和依赖 | `pyproject.toml` |
| **构建** | 生成可上传的分发文件 | `python -m build`, `dist/` |
| **测试上传** | 确认配置无误，避免污染主库 | `twine upload --repository testpypi` |
| **正式上传**| 面向全球发布你的包 | `twine upload dist/*` |
| **更新** | 发布新的特性或修复 | `pyproject.toml` 中的 `version` 字段 |

## 2. 准备工作 (Prerequisites)

### 注册 PyPI 和 TestPyPI 账户

由于 PyPI 不允许删除包，强烈建议**先在 TestPyPI** 上进行完整的测试和演练。

*   **官方 PyPI**: [https://pypi.org/account/register/](https://pypi.org/account/register/)
*   **测试 TestPyPI**: [https://test.pypi.org/account/register/](https://test.pypi.org/account/register/)

> **重要提示**: 这两个网站的账户是完全独立的，即使使用相同的用户名和密码，也需要分别注册。

### API Token：推荐的认证方式

传统的用户名/密码认证方式不够安全，因为每次上传都需要输入密码。**API Token** 是 PyPI 官方推荐的、更安全的认证方式。

1.  **登录 PyPI/TestPyPI**。
2.  进入 **"Account settings"**（账户设置）。
3.  找到 **"API tokens"** 部分，点击 **"Add API token"**。
4.  **Scope（范围）**：选择 **"Entire account"** 或仅选择你当前要上传的包名（如果已存在）。
5.  创建后，**立即复制生成的 Token**（格式为 `pypi-` 开头）。请注意：Token 只会显示一次！

> **最佳实践**: 每次上传时，在终端提示输入密码时，直接粘贴这个完整的 API Token。你也可以将 Token 配置在 `~/.pypirc` 文件中，实现非交互式上传。

### 安装必要的打包工具

确保你的 Python 环境中安装了最新的打包和上传工具。

```bash
# 确保 pip 是最新版本
python -m pip install --upgrade pip

# 安装 build：负责将项目构建成 Wheel 和 sdist
# 安装 twine：负责将构建好的包安全地上传到 PyPI
python -m pip install --upgrade build twine
```

## 3. 项目结构 (Project Structure)

遵循现代 Python 打包标准，推荐使用 `src` 布局来组织项目结构。

```
my_awesome_project/  # 项目根目录
├── src/             # 源代码目录 (推荐的布局)
│   └── my_package/  # 这是你的 Python 包名 (用户 import 的名字)
│       ├── __init__.py  # 必须文件，将目录标记为一个 Python 包
│       └── main.py      # 你的模块代码
├── tests/           # 单元测试文件 (可选)
├── LICENSE          # 许可证文件 (必不可少，决定了用户如何使用你的代码)
├── pyproject.toml   # 核心配置文件 (新标准，取代 setup.py/setup.cfg)
└── README.md        # 项目说明文件 (PyPI 页面展示的主要内容)
```

**`src` 布局的优势**:
1.  **避免导入错误**: 当你在项目根目录测试时，Python 可能会导入根目录下的 `my_package` 目录，而不是安装后的包。使用 `src` 布局可以避免这种错误的局部导入。
2.  **清晰隔离**: 将包的代码与项目配置、文档等文件清晰地隔离开来。

## 4. 配置打包文件 (`pyproject.toml`) 详解

`pyproject.toml` 是 PEP 517 和 PEP 621 引入的现代 Python 打包配置文件。它定义了项目的构建系统和所有元数据。

```toml
# pyproject.toml
[build-system]
requires = ["hatchling"] # 告诉构建工具需要使用 hatchling 来构建
build-backend = "hatchling.build" # 具体的构建后端

[project]
# --- 必填字段 ---
name = "my-awesome-package-username" # 必须在 PyPI 上唯一，推荐包含用户名或明确的组织前缀
version = "0.0.1"                    # 遵循语义化版本 (SemVer) 原则
authors = [
    { name="Your Name", email="your.email@example.com" },
]
description = "A small example package for PyPI deployment."

# --- 推荐字段 ---
readme = "README.md"               # 通常指向 Markdown 文件
requires-python = ">=3.8"          # 指定最低兼容的 Python 版本
license = { file="LICENSE" }       # 指向项目根目录下的许可证文件
keywords = ["example", "tutorial", "packaging", "python"]
classifiers = [                    # 标准化标签，方便用户和 PyPI 索引查找
    "Development Status :: 4 - Beta", # 开发状态
    "Intended Audience :: Developers", # 目标用户
    "License :: OSI Approved :: MIT License", # 许可证类型
    "Programming Language :: Python :: 3",
    "Programming Language :: Python :: 3.8",
    "Programming Language :: Python :: 3.9",
    # ... 指定所有支持的版本
]

# --- 额外配置 (针对 src 布局) ---
[tool.hatch.build.targets.wheel]
packages = ["src/my_package"] # 明确告诉构建工具源代码在哪里
```

### 第二部分：`[project]`（项目元数据）

| 字段 | 说明 | 示例 |
| :--- | :--- | :--- |
| `name` | 包的唯一名称。 | `"requests-by-kennethreitz"` |
| `version`| 包的版本号。**每次更新必须递增**。 | `"1.0.0"`, `"2.1.5"` |
| `authors`| 作者信息。可以指定多个。 | `[{name="...", email="..."}]` |
| `description`| 包的简短描述（一行）。 | `"A minimalist HTTP client."` |
| `readme` | 指向 README 文件。 | `"README.md"` |
| `requires-python`| 指定支持的 Python 版本范围。 | `">=3.8, <4.0"` |
| `classifiers` | 标准化的分类标签列表。 | `"License :: OSI Approved :: MIT License"` |

### 第三部分：`[project.dependencies]`（项目依赖）

列出你的包运行时需要安装的其他 PyPI 包。

```toml
[project.dependencies]
requests = ">=2.20.0" # 依赖 requests 库，版本必须大于等于 2.20.0
numpy = ""           # 依赖最新的 numpy
```

## 5. 构建分发包 (Building Distributions)

构建步骤将你的源代码和元数据转换为两种标准的分发格式：源码包和 Wheel 包。

### 构建命令

确保终端位于 `my_awesome_project/` 项目根目录，运行：

```bash
python -m build
```

成功后，会生成一个名为 `dist/` 的新目录。

### 生成的包类型

| 文件类型 | 后缀 | 描述 |
| :--- | :--- | :--- |
| **Wheel 包** | `.whl` | **预编译的二进制分发格式**。`pip` 优先安装它，因为它包含了所有所需文件，无需在用户机器上执行构建过程，安装速度最快。 |
| **源码包** | `.tar.gz` | **Source Distribution (sdist)**。包含源代码和构建脚本。当 Wheel 包不可用时，`pip` 会下载 sdist 并在本地执行构建。 |

## 6. 上传至 TestPyPI (演练与安全)

在 TestPyPI 上测试你的上传和安装是至关重要的。

### 上传命令

使用 `twine` 上传工具，并通过 `--repository` 参数指定目标为 TestPyPI。

```bash
python -m twine upload --repository testpypi dist/*
```

**注意**: `dist/*` 表示上传 `dist/` 目录下所有生成的文件（sdist 和 .whl）。

### 使用 API Token 认证

运行上传命令后，Twine 会提示输入用户名和密码：

*   **用户名**: 输入你在 **TestPyPI** 上注册的用户名。
*   **密码**: **粘贴完整的 TestPyPI API Token**。

### 验证 TestPyPI 安装

上传成功后，可以通过以下命令来验证你的包是否能够被正确安装：

```bash
# --index-url 告诉 pip 从 TestPyPI 而不是官方 PyPI 寻找包
python -m pip install --index-url https://test.pypi.org/simple/ my-awesome-package-username
```

## 7. 上传至官方 PyPI (正式发布)

确认 TestPyPI 流程无误后，进行正式发布。

### 正式上传命令

这次省略 `--repository` 参数，`twine` 默认上传到官方 PyPI。

```bash
python -m twine upload dist/*
```

*   **用户名**: 输入你在**官方 PyPI** 上注册的用户名。
*   **密码**: **粘贴完整的 官方 PyPI API Token**。

### 最终验证

你的包现已正式发布！
1.  访问项目页面：`https://pypi.org/project/你的包名/`
2.  通知全世界！任何人都可以通过以下命令安装你的包：
    ```bash
    python -m pip install 你的包名
    ```

## 8. 发布新版本与更新 (Updating)

### 版本号的重要性

PyPI **不允许**覆盖或删除已发布的同版本号的包。因此，每次发布更新（即使只是一个微小的修改），都**必须**递增版本号。

### 更新步骤

1.  **修改版本号**: 在 `pyproject.toml` 文件中，递增 `[project].version` 字段的值（例如从 `"0.0.1"` 改为 `"0.0.2"`）。
2.  **清理旧文件**: 删除旧的构建目录 `dist/`。
3.  **重新构建**: 再次运行 `python -m build`。
4.  **重新上传**: 再次运行 `python -m twine upload dist/*`。

### 语义化版本 (Semantic Versioning)

推荐遵循以下版本命名规范：**`主版本号.次版本号.修订号` (MAJOR.MINOR.PATCH)**

*   **PATCH (修订号)**: 修复了 Bug，但不影响 API (例如 `0.0.1` -> `0.0.2`)。
*   **MINOR (次版本号)**: 增加了向后兼容的新功能 (例如 `0.1.0` -> `0.2.0`)。
*   **MAJOR (主版本号)**: 进行了不向后兼容的重大 API 更改 (例如 `1.0.0` -> `2.0.0`)。

## 9. 速查表 (Quick Reference)

| 操作 | 命令/配置 |
| :--- | :--- |
| **安装工具** | `python -m pip install --upgrade build twine` |
| **核心配置** | `pyproject.toml` |
| **构建命令** | `python -m build` |
| **上传至 TestPyPI** | `python -m twine upload --repository testpypi dist/*` |
| **上传至官方 PyPI** | `python -m twine upload dist/*` |
| **验证 TestPyPI 安装** | `pip install --index-url https://test.pypi.org/simple/ 包名` |
| **更新关键** | 每次上传前必须修改 `pyproject.toml` 中的 `version` 字段 |