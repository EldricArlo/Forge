# Areas_Of_Knowledge\Frameworks_And_Tools\PyQt6\PyQt6_Framework_Guide.md

### **Python PyQt6 框架语法详解**

#### <a name="toc"></a>目录

- [Areas\_Of\_Knowledge\\Frameworks\_And\_Tools\\PyQt6\\PyQt6\_Framework\_Guide.md](#areas_of_knowledgeframeworks_and_toolspyqt6pyqt6_framework_guidemd)
    - [**Python PyQt6 框架语法详解**](#python-pyqt6-框架语法详解)
      - [目录](#目录)
      - [1. PyQt6 简介与安装](#1-pyqt6-简介与安装)
        - [**什么是 PyQt6？**](#什么是-pyqt6)
        - [**安装 PyQt6**](#安装-pyqt6)
      - [2. 创建第一个 PyQt6 应用](#2-创建第一个-pyqt6-应用)
        - [**基本结构**](#基本结构)
        - [**代码解析**](#代码解析)
      - [3. 常用控件 (Widgets)](#3-常用控件-widgets)
        - [**窗口 (QWidget, QMainWindow, QDialog)**](#窗口-qwidget-qmainwindow-qdialog)
        - [**标签 (QLabel)**](#标签-qlabel)
        - [**按钮 (QPushButton)**](#按钮-qpushbutton)
        - [**输入框 (QLineEdit)**](#输入框-qlineedit)
        - [**其他常用控件**](#其他常用控件)
      - [4. 布局管理 (Layout Management)](#4-布局管理-layout-management)
        - [**垂直布局 (QVBoxLayout)**](#垂直布局-qvboxlayout)
        - [**水平布局 (QHBoxLayout)**](#水平布局-qhboxlayout)
        - [**网格布局 (QGridLayout)**](#网格布局-qgridlayout)
        - [**表单布局 (QFormLayout)**](#表单布局-qformlayout)
      - [5. 信号与槽 (Signals and Slots)](#5-信号与槽-signals-and-slots)
        - [**核心机制**](#核心机制)
        - [**内置信号与槽的连接**](#内置信号与槽的连接)
        - [**自定义槽函数**](#自定义槽函数)
      - [6. 事件处理 (Event Handling)](#6-事件处理-event-handling)
        - [**事件模型**](#事件模型)
        - [**重写事件处理器**](#重写事件处理器)
      - [7. 使用 Qt 样式表 (QSS) 美化界面](#7-使用-qt-样式表-qss-美化界面)
        - [**QSS 简介**](#qss-简介)
        - [**应用 QSS 的方式**](#应用-qss-的方式)
        - [**QSS 语法示例**](#qss-语法示例)
      - [8. 完整示例：带目录跳转的应用](#8-完整示例带目录跳转的应用)

---

#### <a name="introduction"></a>1. PyQt6 简介与安装

##### <a name="what-is-pyqt6"></a>**什么是 PyQt6？**

PyQt6 是一个功能强大且成熟的跨平台 GUI 框架 Qt 6 的 Python 绑定。 它允许开发者使用 Python 语言创建具有丰富用户界面的桌面应用程序，这些应用程序可以运行在 Windows、macOS 和 Linux 等多种操作系统上。

##### <a name="install-pyqt6"></a>**安装 PyQt6**

安装 PyQt6 非常简单，可以使用 pip 命令进行安装。同时，建议安装 `pyqt6-tools`，其中包含了 Qt Designer 等有用的工具，可以帮助你通过拖拽的方式设计界面。

```bash
pip install PyQt6
pip install pyqt6-tools
```

---

#### <a name="first-application"></a>2. 创建第一个 PyQt6 应用

##### <a name="basic-structure"></a>**基本结构**

每一个 PyQt6 应用都必须创建一个 `QApplication` 对象。 窗口和控件都属于 `QWidget` 的子类。下面是一个创建简单窗口的示例：

```python
import sys
from PyQt6.QtWidgets import QApplication, QWidget, QLabel

# 1. 创建 QApplication 实例
app = QApplication(sys.argv)

# 2. 创建一个窗口
window = QWidget()
window.setWindowTitle('我的第一个 PyQt6 应用')
window.setGeometry(100, 100, 400, 300) # x, y, width, height

# 3. 在窗口中添加一个标签控件
label = QLabel('你好, PyQt6!', parent=window)
label.move(150, 130)

# 4. 显示窗口
window.show()

# 5. 运行应用的主循环
sys.exit(app.exec())
```

##### <a name="code-explanation"></a>**代码解析**

*   `QApplication(sys.argv)`: 每个 PyQt6 应用都需要一个 `QApplication` 实例，`sys.argv` 允许你从命令行传递参数。
*   `QWidget`: 是所有用户界面对象的基类。 没有父对象的 `QWidget` 就是一个独立的窗口。
*   `setWindowTitle()`: 设置窗口的标题。
*   `setGeometry()`: 设置窗口在屏幕上的位置和大小。
*   `QLabel`: 用于显示文本或图片的控件。
*   `window.show()`: 显示窗口。
*   `sys.exit(app.exec())`: 启动应用程序的事件循环，等待用户的操作。

---

#### <a name="common-widgets"></a>3. 常用控件 (Widgets)

##### <a name="windows"></a>**窗口 (QWidget, QMainWindow, QDialog)**

*   `QWidget`: 是所有可绘制控件的基类。
*   `QMainWindow`: 是一个主窗口，通常包含菜单栏、工具栏、状态栏等。
*   `QDialog`: 是对话框窗口的基类，用于与用户进行简短的交互。

##### <a name="qlabel"></a>**标签 (QLabel)**

用于显示静态文本或图像。可以设置对齐方式、富文本等。

```python
label = QLabel('这是一个标签')
label.setAlignment(Qt.AlignmentFlag.AlignCenter) # 设置文本居中
```

##### <a name="qpushbutton"></a>**按钮 (QPushButton)**

最常见的控件之一，用于触发一个动作。

```python
button = QPushButton('点击我')
button.clicked.connect(on_button_click) # 连接点击信号到槽函数
```

##### <a name="qlineedit"></a>**输入框 (QLineEdit)**

用于单行文本输入。

```python
line_edit = QLineEdit()
line_edit.setPlaceholderText('请输入用户名') # 设置提示文本
text = line_edit.text() # 获取输入的文本
line_edit.textChanged.connect(on_text_changed) # 文本改变时触发信号
```

##### <a name="other-widgets"></a>**其他常用控件**

*   `QCheckBox`: 复选框。
*   `QRadioButton`: 单选按钮。
*   `QComboBox`: 下拉列表框。
*   `QSlider`: 滑块。
*   `QProgressBar`: 进度条。
*   `QTextEdit`: 多行富文本编辑器。

---

#### <a name="layout-management"></a>4. 布局管理 (Layout Management)

布局管理器用于自动排列窗口中的控件，当窗口大小改变时，控件会自动调整位置和大小。这是组织控件的首选方式。

##### <a name="qvboxlayout"></a>**垂直布局 (QVBoxLayout)**

将控件从上到下垂直排列。

```python
from PyQt6.QtWidgets import QVBoxLayout

layout = QVBoxLayout()
layout.addWidget(QLabel('第一行'))
layout.addWidget(QPushButton('第二行'))
self.setLayout(layout)
```

##### <a name="qhboxlayout"></a>**水平布局 (QHBoxLayout)**

将控件从左到右水平排列。

```python
from PyQt6.QtWidgets import QHBoxLayout

layout = QHBoxLayout()
layout.addWidget(QLabel('左侧'))
layout.addWidget(QPushButton('右侧'))
self.setLayout(layout)
```

##### <a name="qgridlayout"></a>**网格布局 (QGridLayout)**

将控件放置在网格中，通过行和列的索引来指定位置。这是最常用的布局类。

```python
from PyQt6.QtWidgets import QGridLayout

layout = QGridLayout()
layout.addWidget(QPushButton('按钮1'), 0, 0) # 第0行，第0列
layout.addWidget(QPushButton('按钮2'), 0, 1) # 第0行，第1列
layout.addWidget(QPushButton('按钮3'), 1, 0, 1, 2) # 第1行，第0列，占1行2列
self.setLayout(layout)
```

##### <a name="qformlayout"></a>**表单布局 (QFormLayout)**

适用于创建表单，由两列组成：标签列和输入控件列。

```python
from PyQt6.QtWidgets import QFormLayout

layout = QFormLayout()
layout.addRow('姓名:', QLineEdit())
layout.addRow('年龄:', QLineEdit())
self.setLayout(layout)
```

---

#### <a name="signals-and-slots"></a>5. 信号与槽 (Signals and Slots)

信号与槽是 Qt 的核心机制，也是 PyQt6 中处理事件和实现组件间通信的基础。

##### <a name="core-mechanism"></a>**核心机制**

*   **信号 (Signal)**: 当控件的状态发生改变时（例如按钮被点击），会发出一个信号。
*   **槽 (Slot)**: 是一个函数或方法，用于响应特定的信号。
*   **连接 (Connect)**: 使用 `connect()` 方法将一个信号连接到一个槽，当信号发出时，与之连接的槽会自动被调用。

##### <a name="built-in-connection"></a>**内置信号与槽的连接**

许多控件都有内置的信号和槽。例如，`QPushButton` 有一个 `clicked` 信号，可以连接到一个函数上。

```python
def on_button_click():
    print('按钮被点击了!')

button = QPushButton('点我')
button.clicked.connect(on_button_click)
```

##### <a name="custom-slot"></a>**自定义槽函数**

任何 Python 函数都可以作为槽函数。如果信号带有参数，槽函数也需要定义相应的参数来接收。

```python
slider = QSlider()
label = QLabel('0')

def on_value_changed(value):
    label.setText(str(value))

slider.valueChanged.connect(on_value_changed)
```

---

#### <a name="event-handling"></a>6. 事件处理 (Event Handling)

GUI 应用是事件驱动的。 除了信号与槽，还可以通过重写控件的事件处理器 (Event Handler) 来响应事件。

##### <a name="event-model"></a>**事件模型**

事件由用户操作（如鼠标点击、键盘输入）或系统（如定时器）产生。 PyQt6 的事件循环会捕获这些事件并分发给相应的对象进行处理。

##### <a name="reimplementing-handlers"></a>**重写事件处理器**

可以通过继承一个控件类并重写其特定的事件处理函数来实现自定义的事件响应。

*   `keyPressEvent(event)`: 处理键盘按下事件。
*   `mousePressEvent(event)`: 处理鼠标按下事件。
*   `mouseMoveEvent(event)`: 处理鼠标移动事件。
*   `closeEvent(event)`: 处理窗口关闭事件。

```python
from PyQt6.QtWidgets import QWidget
from PyQt6.QtGui import QKeyEvent

class MyWidget(QWidget):
    def keyPressEvent(self, event: QKeyEvent):
        if event.key() == Qt.Key.Key_Escape:
            self.close() # 按下 Esc 键关闭窗口
```

---

#### <a name="qss"></a>7. 使用 Qt 样式表 (QSS) 美化界面

##### <a name="qss-introduction"></a>**QSS 简介**

Qt Style Sheets (QSS) 是一种用于自定义 PyQt6 应用外观的机制，其语法和用法与网页开发的 CSS 非常相似。

##### <a name="applying-qss"></a>**应用 QSS 的方式**

可以通过 `setStyleSheet()` 方法将 QSS 应用到单个控件、整个窗口或整个应用程序。

```python
# 应用于单个按钮
button.setStyleSheet("background-color: red; color: white;")

# 应用于整个窗口 (及其所有子控件)
self.setStyleSheet("""
    QPushButton {
        font-size: 16px;
        background-color: #4CAF50;
        color: white;
    }
    QPushButton:hover {
        background-color: #45a049;
    }
""")
```

##### <a name="qss-examples"></a>**QSS 语法示例**

*   **选择器 (Selector)**: 指定样式将应用于哪种类型的控件 (如 `QPushButton`, `QLabel`)。
*   **属性 (Property)**: `color`, `background-color`, `font-size`, `border` 等。
*   **伪状态 (Pseudo-states)**: 如 `:hover` (鼠标悬停), `:pressed` (按下), `:checked` (选中)。

---

#### <a name="full-example"></a>8. 完整示例：带目录跳转的应用

这个示例将演示如何使用 `QListWidget` 和 `QStackedWidget` 创建一个带有可点击目录的应用程序。

```python
import sys
from PyQt6.QtWidgets import (
    QApplication, QMainWindow, QWidget, QVBoxLayout, QHBoxLayout,
    QListWidget, QStackedWidget, QLabel, QPushButton
)
from PyQt6.QtCore import Qt

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()

        self.setWindowTitle("PyQt6 语法详解示例")
        self.setGeometry(100, 100, 800, 600)

        # 创建主布局
        main_widget = QWidget()
        main_layout = QHBoxLayout(main_widget)
        self.setCentralWidget(main_widget)

        # 左侧目录
        self.toc_list = QListWidget()
        self.toc_list.setFixedWidth(200)
        self.toc_list.addItem("1. 简介与安装")
        self.toc_list.addItem("2. 常用控件")
        self.toc_list.addItem("3. 布局管理")
        self.toc_list.addItem("4. 信号与槽")

        # 右侧内容区
        self.stacked_widget = QStackedWidget()

        # 添加页面
        self.stacked_widget.addWidget(self.create_page_introduction())
        self.stacked_widget.addWidget(self.create_page_widgets())
        self.stacked_widget.addWidget(self.create_page_layouts())
        self.stacked_widget.addWidget(self.create_page_signals_slots())

        # 将目录和内容区添加到主布局
        main_layout.addWidget(self.toc_list)
        main_layout.addWidget(self.stacked_widget)

        # 连接目录点击信号到内容切换槽
        self.toc_list.currentRowChanged.connect(self.stacked_widget.setCurrentIndex)
        self.toc_list.setCurrentRow(0)

    def create_page_introduction(self):
        page = QWidget()
        layout = QVBoxLayout(page)
        label = QLabel("<h2>PyQt6 简介</h2><p>PyQt6 是 Qt 6 框架的 Python 绑定，用于创建跨平台的桌面应用。</p>")
        label.setWordWrap(True)
        layout.addWidget(label)
        layout.setAlignment(Qt.AlignmentFlag.AlignTop)
        return page

    def create_page_widgets(self):
        page = QWidget()
        layout = QVBoxLayout(page)
        layout.addWidget(QLabel("<h3>常用控件示例</h3>"))
        layout.addWidget(QPushButton("这是一个 QPushButton"))
        layout.addWidget(QLabel("这是一个 QLabel"))
        layout.setAlignment(Qt.AlignmentFlag.AlignTop)
        return page

    def create_page_layouts(self):
        page = QWidget()
        layout = QVBoxLayout(page)
        label = QLabel("<b>布局管理</b><p>包括 QVBoxLayout, QHBoxLayout, QGridLayout 等。</p>")
        label.setWordWrap(True)
        layout.addWidget(label)
        layout.setAlignment(Qt.AlignmentFlag.AlignTop)
        return page

    def create_page_signals_slots(self):
        page = QWidget()
        layout = QVBoxLayout(page)
        label = QLabel("<b>信号与槽</b><p>是 PyQt6 的核心机制，用于对象间的通信。</p>")
        label.setWordWrap(True)
        
        button = QPushButton("点击测试信号")
        self.click_label = QLabel("等待点击...")
        
        button.clicked.connect(self.on_test_button_click)

        layout.addWidget(label)
        layout.addWidget(button)
        layout.addWidget(self.click_label)
        layout.setAlignment(Qt.AlignmentFlag.AlignTop)
        return page

    def on_test_button_click(self):
        self.click_label.setText("按钮被点击了！信号已触发。")


if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = MainWindow()
    window.show()
    sys.exit(app.exec())
```