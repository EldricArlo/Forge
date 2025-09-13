GitHub 徽章是嵌入在项目 `README.md` 文件中的小型图片，它们能够直观地展示项目的当前状态、代码质量、社区活跃度等关键信息。一个专业且维护良好的项目，通常会使用一系列徽章来提供项目的快照，这不仅增强了项目的可信度，也方便了潜在用户和贡献者的快速评估。

## 徽章的核心生成服务：Shields.io

几乎所有动态或静态的徽章都可以通过 [**Shields.io**](https://shields.io/) 这个强大的服务生成。它提供了稳定、一致的徽章样式，并能从数百个服务（如 GitHub, PyPI, NPM, Codecov 等）中动态拉取数据。

#### **自定义徽章基础**

Shields.io 的基础 URL 格式非常简单：
`https://img.shields.io/badge/<标签>-<信息>-<颜色>`

*   **`<标签>` (LABEL)**: 显示在徽章左侧的文本。
*   **`<信息>` (MESSAGE)**: 显示在徽章右侧的文本。
*   **`<颜色>` (COLOR)**: 可以是预设的颜色名 (`brightgreen`, `green`, `yellow`, `orange`, `red`, `blue`) 或十六进制颜色代码 (如 `ff69b4`)。

**高级用法：添加图标和样式**

你还可以在 URL 末尾添加查询参数来丰富徽章：

*   `?logo=<logo-name>`: 加入一个图标。图标名称可以在 [**Simple Icons**](https://simpleicons.org/) 上查找。
*   `?style=<style>`: 改变徽章样式，可选值有 `flat`, `flat-square`, `plastic`, `for-the-badge`, `social`。
*   `?logoColor=<color>`: 指定图标的颜色。

**示例：**
创建一个标签为 "Framework"，信息为 "React"，样式为 `for-the-badge` 并带有 React 图标的徽章。

`https://img.shields.io/badge/Framework-React-blue?style=for-the-badge&logo=react`

预览：![Framework-React](https://img.shields.io/badge/Framework-React-blue?style=for-the-badge&logo=react)

***

## 徽章分类详解

### 类别一：构建与持续集成 (Build & CI/CD)

这类徽章是项目专业性的基本体现，它向外界表明你的代码有自动化的构建和测试流程，保证了代码的可用性。

| 徽章预览 | 标签名称 | 用途说明 | 常见服务/生成方式 |
| :--- | :--- | :--- | :--- |
| ![GitHub Actions](https://img.shields.io/github/actions/workflow/status/actions/starter-workflows/python-application.yml) | Build Status | **（强烈推荐）** 显示项目在 GitHub Actions 上的最新构建和测试流程是否通过。 | [GitHub Actions](https://github.com/features/actions) 会为你的工作流自动生成徽章链接。 |
| ![Travis CI](https://img.shields.io/travis/com/travis-ci/travis-web.svg) | Travis CI Status | 类似 GitHub Actions，显示在 Travis CI 上的构建状态。 | [Travis CI](https://www.travis-ci.com/) |
| ![CircleCI](https://img.shields.io/circleci/build/gh/CircleCI-Public/circleci-cli.svg) | CircleCI Status | 类似 GitHub Actions，显示在 CircleCI 上的构建状态。 | [CircleCI](https://circleci.com/) |
| ![Jenkins](https://img.shields.io/jenkins/build?jobUrl=https%3A%2F%2Fci.jenkins.io%2Fjob%2FCore%2Fjob%2Fjenkins%2Fjob%2Fmaster%2F) | Jenkins Build | 如果你使用自托管的 Jenkins，可以用它来显示构建状态。 | [Shields.io](https://shields.io/) (需要 Jenkins 插件) |

### 类别二：代码质量与分析 (Code Quality & Analysis)

关注代码质量是优秀开发者的标志。这类徽章展示了项目在可维护性、测试覆盖率和代码规范性方面的投入。

| 徽章预览 | 标签名称 | 用途说明 | 常见服务/生成方式 |
| :--- | :--- | :--- | :--- |
| ![Codecov](https://img.shields.io/codecov/c/github/codecov/example-python) | Code Coverage | **（强烈推荐）** 显示单元测试覆盖了多少百分比的代码，是代码质量的关键指标。 | [Codecov](https://about.codecov.io/), [Coveralls](https://coveralls.io/) |
| ![SonarCloud](https://img.shields.io/sonar/quality_gate/sonarsource/sonarqube) | Quality Gate | SonarCloud 提供的综合质量门禁状态（Bugs, Vulnerabilities, Code Smells）。 | [SonarCloud](https://sonarcloud.io/) |
| ![Go Report Card](https://goreportcard.com/badge/github.com/gin-gonic/gin) | Go Report Card | 针对 Go 语言项目，从代码格式、拼写、复杂度等方面进行静态分析并评分。 | [Go Report Card](https://goreportcard.com/) |
| ![Code Climate](https://img.shields.io/codeclimate/maintainability/codeclimate/codeclimate) | Maintainability | 由 Code Climate 分析的代码可维护性评级 (A-F)。 | [Code Climate](https://codeclimate.com/) |
| ![Code Style: Black](https://img.shields.io/badge/code%20style-black-000000.svg) | Code Style | 表明项目遵循特定的代码风格规范，如 Black, Prettier, StandardJS 等。 | [Shields.io](https://shields.io/) (手动创建) |

### 类别三：包与分发 (Package & Distribution)

这类徽章为项目的使用者提供了直接的版本、下载和平台信息，是衡量项目流行度和可用性的重要标志。

| 徽章预览 | 标签名称 | 用途说明 | 常见服务/生成方式 |
| :--- | :--- | :--- | :--- |
| ![PyPI Version](https://img.shields.io/pypi/v/requests) | PyPI Version | 显示项目在 Python 包索引 (PyPI) 上的最新版本。 | [Shields.io](https://shields.io/) |
| ![NPM Version](https://img.shields.io/npm/v/react) | NPM Version | 显示项目在 Node.js 包管理器 (NPM) 上的最新版本。 | [Shields.io](https://shields.io/) |
| ![Crates.io](https://img.shields.io/crates/v/serde) | Crates.io Version | 显示项目在 Rust 包管理器 (Crates.io) 上的最新版本。 | [Shields.io](https://shields.io/) |
| ![PyPI Downloads](https://img.shields.io/pypi/dm/requests) | Downloads | 显示包的月/周/日下载量，是衡量流行度的重要指标。 | [Shields.io](https://shields.io/) |
| ![Docker Pulls](https://img.shields.io/docker/pulls/library/ubuntu) | Docker Pulls | 显示你的 Docker 镜像被拉取了多少次。 | [Shields.io](https://shields.io/) |

### 类别四：项目状态与活跃度 (Project Status & Activity)

这类徽章动态地反映了项目的生命周期和维护情况，让用户知道项目是否仍在积极更新。

| 徽章预览 | 标签名称 | 用途说明 | 常见服务/生成方式 |
| :--- | :--- | :--- | :--- |
| ![Project Status: Active](https://img.shields.io/badge/status-active-success.svg) | Project Status | 表明项目正处于积极开发 (`active`), 仅维护 (`maintained`), 或已归档 (`archived`)。 | [Shields.io](https://shields.io/) (手动创建) |
| ![GitHub last commit](https://img.shields.io/github/last-commit/microsoft/vscode) | Last Commit | **(推荐)** 动态显示主分支的最近一次提交时间，证明项目是“活的”。 | [Shields.io](https://shields.io/) / GitHub |
| ![GitHub release](https://img.shields.io/github/v/release/microsoft/vscode) | Latest Release | 显示项目最新的发行版 (Release) 版本号。 | [Shields.io](https://shields.io/) / GitHub |
| ![GitHub issues](https://img.shields.io/github/issues/microsoft/vscode) | Open Issues | 显示当前开启的 Issue 数量，反映社区的活跃度和项目的响应情况。 | [Shields.io](https://shields.io/) / GitHub |
| ![GitHub contributors](https://img.shields.io/github/contributors/microsoft/vscode) | Contributors | 显示为项目贡献过代码的开发者数量。 | [Shields.io](https://shields.io/) |

### 类别五：社区与社交 (Community & Social)

社区是开源项目的灵魂。这类徽章是项目的社交证明，并为新成员提供了加入社区的入口。

| 徽章预览 | 标签名称 | 用途说明 | 常见服务/生成方式 |
| :--- | :--- | :--- | :--- |
| ![GitHub Stars](https://img.shields.io/github/stars/microsoft/vscode?style=social) | GitHub Stars | **(必备)** 动态显示项目的 Star 数量，是项目受欢迎程度的最直接体现。 | [Shields.io](https://shields.io/) / GitHub |
| ![GitHub Forks](https://img.shields.io/github/forks/microsoft/vscode?style=social) | GitHub Forks | 动态显示项目的 Fork 数量，反映了项目的可扩展性和社区参与度。 | [Shields.io](https://shields.io/) / GitHub |
| ![Discord](https://img.shields.io/discord/502590858676436992?logo=discord&label=chat&color=5865F2) | Chat | 提供一个直接链接到项目的社区聊天室，如 Discord, Slack, Gitter, Telegram 等。 | [Shields.io](https://shields.io/) |
| ![Twitter Follow](https://img.shields.io/twitter/follow/github?style=social) | Twitter | 鼓励用户关注项目或作者的 Twitter 账号，用于发布项目更新和公告。 | [Shields.io](https://shields.io/) |
| ![Contributions welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg) | Contributions | **(强烈推荐)** 友好地告诉其他开发者，欢迎他们为项目做贡献，是建立开放社区的重要标志。 | [Shields.io](https://shields.io/) (手动创建) |

### 类别六：许可证与依赖 (License & Dependencies)

清晰地说明项目的使用规则和技术栈，是项目规范化的重要组成部分。

| 徽章预览 | 标签名称 | 用途说明 | 常见服务/生成方式 |
| :--- | :--- | :--- | :--- |
| ![License](https://img.shields.io/github/license/microsoft/vscode) | License | **(必备)** 动态显示项目的开源许可证，明确他人使用、修改和分发代码的权利和义务。 | [Shields.io](https://shields.io/) / GitHub |
| ![Python Version](https://img.shields.io/pypi/pyversions/requests) | Python Version | 动态显示项目支持的 Python 版本范围 (从 PyPI 读取)。 | [Shields.io](https://shields.io/) |
| ![Dependencies](https://img.shields.io/librariesio/github/requests/requests) | Dependencies | 显示项目的依赖关系是否都是最新的。 | [Libraries.io](https://libraries.io/) |

### 类别七：平台、框架与自定义 (Platform, Frameworks & Custom)

展示项目所支持的平台、使用的核心框架，或通过自定义徽章来表达个性和趣味信息。

| 徽章预览 | 标签名称 | 用途说明 | 常见服务/生成方式 |
| :--- | :--- | :--- | :--- |
| ![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20macOS%20%7C%20Linux-blue?logo=windows&logoColor=white) | Platform | 表明项目支持的操作系统。 | [Shields.io](https://shields.io/) (手动创建) |
| ![Framework](https://img.shields.io/badge/Framework-Flask-black?logo=flask) | Framework | 表明项目使用的主要框架或技术。 | [Shields.io](https://shields.io/) (手动创建) |
| ![Made with Love](https://img.shields.io/badge/Made%20with-Love-red?logo=heart) | Fun/Custom | 创建完全自定义的徽章来表达个性、感谢或任何有趣的信息。 | [Shields.io](https://shields.io/#your-badge) |
| ![QQ](https://img.shields.io/badge/QQ%20Group-123456789-1EBAFC?logo=tencentqq&logoColor=white) | QQ Group | 为国内用户提供加入 QQ 群的入口。 | [Shields.io](https://shields.io/) (手动创建) |

***

## 常用徽章实用示例

下表提供了一些可以直接复制和修改的徽章 Markdown 代码段。

| 用途 | 徽章预览 | Markdown 代码 |
| :--- | :--- | :--- |
| **许可证 (MIT)** | [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](https://opensource.org/licenses/MIT) | `[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](https://opensource.org/licenses/MIT)` |
| **Python 版本** | [![Python 3.8+](https://img.shields.io/badge/python-3.8+-3776AB.svg?style=flat-square&logo=python&logoColor=white)](https://www.python.org/) | `[![Python 3.8+](https://img.shields.io/badge/python-3.8+-3776AB.svg?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)` |
| **最新Release** | [![Latest Release](https://img.shields.io/github/v/release/your-username/your-repo?style=flat-square&logo=github)](https://github.com/your-username/your-repo/releases) | `[![Latest Release](https://img.shields.io/github/v/release/your-username/your-repo?style=flat-square&logo=github)](https://github.com/your-username/your-repo/releases)` |
| **加入 Discord** | [![Join Discord](https://img.shields.io/badge/Discord-Join%20Us-5865F2?style=flat-square&logo=discord&logoColor=white)](https://discord.gg/YOUR_INVITE_CODE) | `[![Join Discord](https://img.shields.io/badge/Discord-Join%20Us-5865F2?style=flat-square&logo=discord&logoColor=white)](https://discord.gg/YOUR_INVITE_CODE)` |
| **加入 Telegram** | [![Join Telegram](https://img.shields.io/badge/Telegram-Join%20Chat-2CA5E0?style=flat-square&logo=telegram&logoColor=white)](https://t.me/your_channel) | `[![Join Telegram](https://img.shields.io/badge/Telegram-Join%20Chat-2CA5E0?style=flat-square&logo=telegram&logoColor=white)](https://t.me/your_channel)` |
| **代码测试覆盖率** | [![codecov](https://codecov.io/gh/gin-gonic/gin/branch/master/graph/badge.svg)](https://codecov.io/gh/gin-gonic/gin) | `[![codecov](https://codecov.io/gh/gin-gonic/gin/branch/master/graph/badge.svg)](https://codecov.io/gh/gin-gonic/gin)` |
| **构建状态** | [![Build Status](https://github.com/gin-gonic/gin/actions/workflows/gin.yml/badge.svg)](https://github.com/gin-gonic/gin/actions/workflows/gin.yml) | `[![Build Status](https://github.com/gin-gonic/gin/actions/workflows/gin.yml/badge.svg)](https://github.com/gin-gonic/gin/actions/workflows/gin.yml)` |