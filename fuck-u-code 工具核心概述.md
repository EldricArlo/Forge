### `fuck-u-code` 工具核心概述

`fuck-u-code` 是一款代码质量分析工具，它以一种犀利、幽默的方式揭示和评估代码的质量问题，核心目标是帮助开发者识别出项目中的“屎山”代码。

#### **核心理念**

*   **屎山指数 (Shit Mountain Index)**：该工具独创一个 0-100 分的评分体系。与传统认知相反，**分数越高，代表代码质量越差**。
*   **本地运行**：所有分析都在本地完成，不会上传任何代码，确保了项目的代码安全。

---

### **主要功能特性**

*   **多语言支持**：能够分析多种主流编程语言，包括 Go、JavaScript/TypeScript、Python、Java 和 C/C++。
*   **七维度检测**：从七个关键维度全面评估代码质量：
    *   代码复杂度
    *   函数长度
    *   注释率
    *   错误处理
    *   命名规范
    *   代码重复度
    *   项目结构
*   **多样化报告输出**：
    *   **彩色终端报告**：在命令行中以直观、多彩的方式展示分析结果。
    *   **Markdown 报告**：可以生成 `report.md` 文件，非常适合与 AI 工具集成进行深度分析、用于 CI/CD 流程、生成文档或在团队间共享。
*   **高度灵活的配置**：
    *   支持摘要模式和详细模式。
    *   可自定义报告语言（中文/英文）。
    *   能够灵活排除不需要分析的目录和文件。

---

### **安装与使用**

#### **安装方法**

提供了三种便捷的安装方式：

1.  **Go 环境安装 (推荐)**：
    ```bash
    go install github.com/Done-0/fuck-u-code/cmd/fuck-u-code@latest
    ```
2.  **源码构建**：
    ```bash
    git clone https://github.com/Done-0/fuck-u-code.git
    cd fuck-u-code && go build -o fuck-u-code ./cmd/fuck-u-code
    ```
3.  **Docker 构建与运行**：
    ```bash
    # 构建镜像
    docker build -t fuck-u-code .
    # 运行分析
    docker run --rm -v "/your/project/path:/build" fuck-u-code analyze
    ```

#### **基本用法**

分析项目非常简单，直接在命令后跟上项目路径即可。

```bash
# 分析指定目录
fuck-u-code analyze /path/to/your/project

# 分析当前目录
fuck-u-code analyze
```

#### **常用命令选项**

| 选项 | 简写 | 描述 |
| :--- | :--- | :--- |
| `--verbose` | `-v` | 显示包含代码问题的详细报告。 |
| `--top N` | `-t` | 仅显示问题最严重的前 N 个文件。 |
| `--issues N` | `-i` | 每个文件最多显示 N 个具体问题。 |
| `--summary` | `-s` | 只输出最终的总结评分，隐藏过程。 |
| `--markdown` | `-m` | 将报告以 Markdown 格式输出。 |
| `--lang` | `-l` | 设置报告的语言，如 `zh-CN` 或 `en-US`。 |
| `--exclude` | `-e` | 排除指定的目录或文件（例如 `"**/test/**"`）。 |
| `--skipindex` | `-x` | 分析时跳过 `index.js` 或 `index.ts` 文件。 |

**示例命令：**
```bash
# 生成一份详细的英文 Markdown 报告，只看最烂的 10 个文件
fuck-u-code analyze --markdown --top 10 --lang en-US > report.md
```

---

### **高级功能与注意事项**

*   **与 AI 集成**：通过 `--markdown` 生成的报告，可以直接交给 AI 进行解读、总结和提供进一步的重构建议。
*   **自动化 (CI/CD)**：可以将此工具集成到 CI/CD 流水线中，自动检测代码质量，防止“屎山”代码合入主分支。
*   **默认排除路径**：工具已默认排除常见的依赖、构建产物和日志目录，如 `node_modules`, `vendor`, `dist`, `target` 等。
*   **环境配置**：如果遇到 `command not found` 错误，需要将 Go 的 `bin` 目录（`$(go env GOPATH)/bin`）添加到系统的 `PATH` 环境变量中。