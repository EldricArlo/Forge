
## Inkscape 命令行深度指南：自动化矢量图形处理

Inkscape开源项目的github仓库地址：https://github.com/inkscape/inkscape

更多的知识文档请阅该仓库README说明

**感谢ted-gould大佬对开源社区的贡献！！！**

---

### 目录

- [Inkscape 命令行深度指南：自动化矢量图形处理](#inkscape-命令行深度指南自动化矢量图形处理)
  - [目录](#目录)
  - [引言](#引言)
  - [一、 准备工作：安装与环境配置](#一-准备工作安装与环境配置)
    - [1.1 安装 Inkscape](#11-安装-inkscape)
    - [1.2 环境变量配置与验证](#12-环境变量配置与验证)
  - [二、 Inkscape 命令行基本语法](#二-inkscape-命令行基本语法)
  - [三、 核心功能：文件格式转换与导出](#三-核心功能文件格式转换与导出)
    - [3.1 SVG 转换为 PNG (位图导出)](#31-svg-转换为-png-位图导出)
    - [3.2 导出为其他矢量格式](#32-导出为其他矢量格式)
    - [3.3 批量导出为多种格式](#33-批量导出为多种格式)
  - [四、 精确控制导出区域与尺寸](#四-精确控制导出区域与尺寸)
    - [4.1 控制导出区域](#41-控制导出区域)
    - [4.2 自定义导出尺寸](#42-自定义导出尺寸)
    - [4.3 设置背景色与透明度](#43-设置背景色与透明度)
  - [五、 导出 SVG 中的特定对象](#五-导出-svg-中的特定对象)
    - [5.1 导出指定 ID 的对象](#51-导出指定-id-的对象)
    - [5.2 仅导出特定图层或组](#52-仅导出特定图层或组)
  - [六、 使用 `--actions` 执行高级操作 (Inkscape 1.0+)](#六-使用---actions-执行高级操作-inkscape-10)
    - [6.1 查询可用动作](#61-查询可用动作)
    - [6.2 链式操作：修改与导出](#62-链式操作修改与导出)
    - [6.3 复杂的批量修改示例](#63-复杂的批量修改示例)
  - [七、 查询 SVG 文件信息](#七-查询-svg-文件信息)
  - [八、 批量处理脚本示例](#八-批量处理脚本示例)
    - [8.1 Bash 脚本：批量转换为不同分辨率](#81-bash-脚本批量转换为不同分辨率)
    - [8.2 Windows Batch 脚本：批量转换](#82-windows-batch-脚本批量转换)
  - [总结](#总结)

---

### 引言

对于习惯在终端（命令行）中工作的用户来说，Inkscape 强大的矢量编辑功能同样可以通过命令行来调用，从而实现图形处理的自动化和批量操作。无需打开图形用户界面（GUI），你就可以高效地进行格式转换、导出特定对象、执行复杂动作等。本文将详细讲解如何在终端中使用 Inkscape，重点介绍其新版本（1.0+）的强大功能。

---

### 一、 准备工作：安装与环境配置

#### 1.1 安装 Inkscape

首先，你需要在你的系统上安装 Inkscape。你可以从 [Inkscape 官网](https://inkscape.org/) 下载适合你操作系统的版本。

#### 1.2 环境变量配置与验证

安装完成后，为了能在终端中直接使用 `inkscape` 命令，你需要将 Inkscape 的可执行文件路径添加到系统的环境变量 `PATH` 中。

*   **Windows:**
    *   在系统属性中找到“环境变量”设置。
    *   编辑“系统变量”下的 `Path` 变量，添加你的 Inkscape 安装目录下的 `bin` 文件夹路径（例如 `C:\Program Files\Inkscape\bin`）。
*   **macOS & Linux:**
    *   通过包管理器（如 Homebrew, APT, YUM）安装的 Inkscape 通常会自动配置好路径。
    *   如果手动安装，你需要编辑 `~/.bash_profile`, `~/.zshrc` 或相应的 shell 配置文件，添加类似于 `export PATH="/path/to/inkscape/bin:$PATH"` 的一行。

**验证安装:**
配置完成后，打开一个新的终端窗口，输入以下命令：

```bash
inkscape --version
```
如果能正确显示版本号（推荐 1.0 或更高版本），则说明配置成功。

---

### 二、 Inkscape 命令行基本语法

Inkscape 命令行的基本结构非常简洁且强大：

```bash
inkscape [选项] [输入文件1] [输入文件2] ...
```

你可以同时提供一个或多个输入文件。选项用于控制 Inkscape 执行的具体操作，例如导出格式、导出区域、执行的动作序列等。

---

### 三、 核心功能：文件格式转换与导出

命令行最常见的用途就是进行文件格式的批量转换。

#### 3.1 SVG 转换为 PNG (位图导出)

这是最常用的操作之一。在新版本 (1.0+) 中，使用 `-o` 或 `--export-filename` 选项指定输出文件名是标准做法。

```bash
# 将 input.svg 导出为 output.png
inkscape input.svg -o output.png
```

**兼容旧版本 (0.92.x 及更早版本) 的方法:**
如果你仍在使用旧版本，需要使用 `-e` 或 `--export-png`。

```bash
inkscape input.svg -e output.png
```

#### 3.2 导出为其他矢量格式

Inkscape 支持导出为多种矢量格式，如 PDF, EPS, PS, EMF/WMF 等。你可以通过 `--export-type` 选项来指定。

*   **导出为 PDF:**
    ```bash
    inkscape input.svg --export-type="pdf" -o output.pdf
    ```

*   **导出为 EPS (封装的 PostScript):**
    ```bash
    inkscape input.svg --export-type="eps" -o output.eps
    ```

#### 3.3 批量导出为多种格式

你可以同时指定多个导出类型，Inkscape 会根据输入文件名自动生成相应后缀的输出文件。

```bash
# 自动生成 input.png, input.pdf, input.eps
inkscape input.svg --export-type="png,pdf,eps"
```

---

### 四、 精确控制导出区域与尺寸

默认情况下，Inkscape 导出的是整个页面。通过以下选项可以实现对导出范围和尺寸的精确控制。

#### 4.1 控制导出区域

| 选项 | 描述 | 示例 |
| :--- | :--- | :--- |
| `-C, --export-area-page` | 导出整个页面（默认行为）。 | `inkscape file.svg -o out.png -C` |
| `-D, --export-area-drawing` | 仅导出所有**可见**对象的最小边界框区域。 | `inkscape file.svg -o out.png -D` |
| `-a, --export-area=x0:y0:x1:y1` | 指定一个自定义的矩形区域（像素为单位）。 | `inkscape file.svg -o out.png -a 0:0:500:500` |
| `--export-area-snap` | 使导出区域捕捉到最近的整数像素边界，避免抗锯齿模糊。 | `inkscape file.svg -o out.png -D --export-area-snap` |

#### 4.2 自定义导出尺寸

你可以通过 DPI、宽度或高度来控制位图的导出尺寸。

*   **指定 DPI：**
    ```bash
    # 导出 300 DPI 的位图 (默认 96 DPI)
    inkscape input.svg -o output.png -d 300
    ```

*   **指定宽度或高度（等比缩放）：**
    ```bash
    # 导出宽度为 1080 像素的 PNG，高度自动计算
    inkscape input.svg -o output.png -w 1080

    # 导出高度为 500 像素的 PNG，宽度自动计算
    inkscape input.svg -o output.png -h 500
    ```
    **注意：** 仅指定宽度或高度时，Inkscape 会自动等比例缩放以保持宽高比。

#### 4.3 设置背景色与透明度

主要用于 PNG 导出。

*   **设置背景色（`-b, --export-background=COLOR`）：**
    颜色值可以是 Hex 代码（如 `#ff0000`）、颜色名称（如 `white`）。
    ```bash
    # 导出一个背景为蓝色的 PNG
    inkscape input.svg -o output.png -b "blue"
    ```

*   **设置背景不透明度（`-y, --export-background-opacity=VALUE`）：**
    `VALUE` 范围从 0.0 (完全透明) 到 1.0 (完全不透明)。
    ```bash
    # 导出一个白色背景、50% 透明度的 PNG
    inkscape input.svg -o output.png -b "#ffffff" -y 0.5
    ```

---

### 五、 导出 SVG 中的特定对象

如果你需要从复杂的 SVG 文件中提取特定的元素，你需要依赖该元素在 SVG 文件中设置的 ID。

#### 5.1 导出指定 ID 的对象

*   **`-i, --export-id=OBJECT_ID`:** 指定要导出的对象的 ID。
*   **`-j, --export-id-only`:** 仅导出指定的对象，隐藏页面上的其他所有内容。

**示例：**
```bash
# 导出一个 ID 为 "icon_main" 的对象，仅导出该对象本身
inkscape drawing.svg -o icon.png --export-id="icon_main" --export-id-only -d 200
```

#### 5.2 仅导出特定图层或组

图层在 Inkscape 中本质上也是具有 ID 的组（`<g>` 元素）。你可以像导出对象一样导出图层。

**示例：**
假设有一个 ID 为 `layer1` 的图层。
```bash
inkscape multi_layer.svg -o layer1_only.png --export-id="layer1" --export-id-only
```
这对于从一个主文件中生成多个图标或资产非常有用。

---

### 六、 使用 `--actions` 执行高级操作 (Inkscape 1.0+)

从 Inkscape 1.0 版本开始，引入了功能强大的 `--actions` 选项，它允许你将一系列操作链接起来，在单个命令中执行，极大地增强了命令行操作的灵活性。

#### 6.1 查询可用动作

在设计脚本前，建议先查询可用的所有动作列表：

```bash
inkscape --action-list
```
输出会包含动作的名称（如 `select-all`）、描述和可选参数。

#### 6.2 链式操作：修改与导出

`--actions` 的语法是将多个动作以分号 `;` 分隔，并按顺序执行。

*   **选中所有对象，旋转90度，然后导出为 PNG:**
    ```bash
    inkscape input.svg --actions="select-all;object-rotate-90-cw;export-do" -o output_rotated.png
    ```
    *   `select-all`: 选中画布上所有对象。
    *   `object-rotate-90-cw`: 顺时针旋转 90 度。
    *   `export-do`: 执行导出操作（取决于 `-o` 或 `--export-type` 设置）。

*   **选中特定对象，应用滤镜，然后保存 SVG:**
    ```bash
    inkscape input.svg --actions="select-by-id:path123;dialog-filter-gallery;file-save;file-close" --batch-process
    ```
    **注意：** `--batch-process` 选项可以在处理结束后自动关闭 Inkscape 窗口，对于执行需要弹出 GUI 窗口（如 `dialog-filter-gallery`）的动作时非常重要。

#### 6.3 复杂的批量修改示例

*   **修改文档尺寸为内容边界，然后保存:**
    ```bash
    inkscape original.svg --actions="select-all;fit-canvas-to-selection;file-save" -o modified_canvas.svg
    ```
    *   `fit-canvas-to-selection`: 将页面的尺寸调整为当前选中的内容大小。

*   **将所有文本转换为路径，以确保字体兼容性:**
    ```bash
    inkscape original.svg --actions="select-all;object-to-path;file-save" -o path_only.svg
    ```
    *   `object-to-path`: 将选中的文本、形状等转换为路径对象。

---

### 七、 查询 SVG 文件信息

Inkscape 命令行还可以用来查询 SVG 文件中的对象信息，而无需导出。

*   **`--query-all`:** 列出文件中所有对象的 ID 及其位置和尺寸。
    ```bash
    inkscape drawing.svg --query-all
    ```
    **输出格式示例：** `rect1,10.0,20.0,100.0,50.0` (ID, X, Y, Width, Height)

*   **`--query-id`:** 查询指定 ID 对象的位置和尺寸。
    ```bash
    # 查询 ID 为 "logo" 的对象的宽度和高度
    inkscape drawing.svg --query-id="logo" --query-width --query-height
    ```
    此命令将返回 "logo" 对象的宽度和高度值。

---

### 八、 批量处理脚本示例

结合 shell 脚本，你可以轻松地对整个目录的文件进行批量处理，这正是命令行自动化的精髓所在。

#### 8.1 Bash 脚本：批量转换为不同分辨率

```bash
#!/bin/bash
# 脚本名称: batch_convert_svg_to_png.sh

# 定义输出目录
mkdir -p png_output

# 定义需要导出的 DPI 列表
DPI_LIST="96 300 600"

# 遍历当前目录下所有的 .svg 文件
for file in *.svg
do
  echo "正在处理文件: $file"
  FILENAME_NO_EXT="${file%.svg}"

  # 遍历 DPI 列表进行导出
  for dpi in $DPI_LIST
  do
    OUTPUT_FILE="png_output/${FILENAME_NO_EXT}_${dpi}dpi.png"
    echo "  导出: $OUTPUT_FILE"
    
    # 核心 Inkscape 命令: 仅导出绘图区域，指定 DPI，并导出为 PNG
    inkscape "$file" -o "$OUTPUT_FILE" -D -d "$dpi"
  done
done

echo "所有 SVG 文件已批量转换完成！"
```

#### 8.2 Windows Batch 脚本：批量转换

```bat
:: 脚本名称: batch_convert.bat

@echo off
set OUTPUT_DIR=png_output
if not exist %OUTPUT_DIR% mkdir %OUTPUT_DIR%

echo 正在批量转换 SVG 文件...

:: 遍历当前目录下所有的 .svg 文件
for %%f in (*.svg) do (
    echo 正在转换: %%f
    :: 移除 .svg 后缀，创建输出文件名
    set FILENAME_NO_EXT=%%f
    call set FILENAME_NO_EXT=%%FILENAME_NO_EXT:.svg=%%

    :: 执行 Inkscape 转换 (导出为 300 DPI)
    inkscape "%%f" -o "%OUTPUT_DIR%\%FILENAME_NO_EXT%.png" -d 300
)

echo 批量转换完成！请查看 %OUTPUT_DIR% 目录。
pause
```

通过掌握这些命令，你可以将 Inkscape 无缝集成到你的自动化工作流程中，极大地提升处理矢量图形的效率。

---

### 总结

Inkscape 命令行是自动化和批量处理矢量图形的强大工具。从简单的格式转换到复杂的链式操作，`inkscape` 命令提供了足够的灵活性来满足专业工作流的需求。特别是 **`--actions`** 机制的引入，使得用户可以在不依赖 GUI 的情况下，实现以前只有在图形界面中才能完成的复杂编辑操作。