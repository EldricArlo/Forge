## JavaScript 核心概念与 DOM 编程指南

---

### **目录**

[toc]

---

## 一、基础概览与核心概念

### 1.1 JavaScript 定义与特点

*   **定义：** JavaScript (JS) 是一种**轻量级 (lightweight)** 的、**解释型 (interpreted)** 的脚本语言，常被用作**嵌入式 (embedded)** 语言，尤其在浏览器环境中，它赋予了网页交互能力。
*   **核心功能：** JS 本身提供的核心算法和 I/O 能力较少。它主要用于执行数学和逻辑运算，而其强大的功能（如操作网页、处理文件）则依赖于其运行环境（如浏览器或 Node.js）提供的 API。

### 1.2 JS 与 ECMAScript (ES) 的关系

*   **ECMAScript (ES)：** 负责定义 JS 语言的**标准和规范**，是 JS 的规格说明书。
*   **JavaScript (JS)：** 是 ECMAScript 标准的**实现和落地**。
    *   *例如：* ECMAScript 2015 (ES6) 是一个标准，而 V8 引擎中的 JavaScript 实现就是遵循这个标准的具体产品。

### 1.3 程序的执行单位

JS 程序的执行单位是**语句 (Statement)**。通常，一个语句占据一行，并以**分号 (`;`)** 结尾。
虽然现代 JS 引擎支持自动分号插入 (ASI)，但在正式的编程实践中，建议保留分号以避免潜在的解析错误。

```js
// 这是一个语句
var num = 10; 

// 这是一个语句，虽然没有分号，但在大多数情况下会被自动添加
let message = "Hello World" 

// 两个语句写在一行，必须用分号分隔
let a = 1; let b = 2;
```

## 二、语法基础与变量声明

### 2.1 标识符 (Identifier)

**标识符**是用于识别各种值的合法名称，最常见的标识符是变量名、函数名和属性名。

*   **命名规则：**
    1.  必须以字母（A-Z, a-z）、下划线（`_`）或美元符号（`$`）开头。
    2.  不能以**数字**作为开头。
    3.  标识符可以包含数字，但不能是纯数字。
    4.  不能使用 JS 的**保留字**或**关键字**（如 `for`, `if`, `function`, `let`, `const` 等）。
*   **规范建议：**
    1.  不推荐使用**中文**作为标识符。
    2.  推荐使用 **驼峰命名法 (camelCase)**，如 `firstName`。
    3.  常量推荐使用全大写加下划线，如 `MAX_COUNT`。

### 2.2 变量的声明与赋值

在 ES6 之后，我们主要使用 `let` 和 `const` 来声明变量。`var` 已经被视为历史遗留产物。

| 关键字 | 作用域 | 是否可重新赋值 | 变量提升 (Hoisting) | 用途 |
| :--- | :--- | :--- | :--- | :--- |
| `var` | 函数作用域 | 可以 | 存在（声明和初始化一起提升） | 已弃用 |
| `let` | 块级作用域 | 可以 | 存在（只提升声明，进入**暂时性死区**） | 声明普通变量 |
| `const` | 块级作用域 | **不可以** | 存在（只提升声明，进入**暂时性死区**） | 声明常量或不变的引用 |

#### 2.2.1 变量提升 (Hoisting)

**变量提升**是 JavaScript 引擎在执行代码前，将所有变量的**声明**（不包括赋值）提升到当前作用域顶部的行为。

**使用 `var` 时的提升：** 声明被提升，但赋值保留在原位。

```js
console.log(num); // 输出: undefined
var num = 10;
// 引擎实际执行的代码：
/*
var num;
console.log(num);
num = 10;
*/
```

**使用 `let` 和 `const` 时的提升 (暂时性死区 TDZ)：**

`let` 和 `const` 也会被提升，但它们在被声明之前是**不可访问**的，这片区域称为**暂时性死区 (Temporal Dead Zone, TDZ)**。试图访问会导致 `ReferenceError`。

```js
console.log(age); // 抛出: ReferenceError: Cannot access 'age' before initialization
let age = 30;
```

#### 2.2.2 常量声明 (`const`)

使用 `const` 声明的变量，一旦被赋值，就不能再改变。

```js
const URL = "https://baidu.com";
// URL = "https://google.com"; // 错误: Assignment to constant variable.

// 注意：const 确保的是变量指向的内存地址不变。
// 如果const指向的是一个对象，对象的属性是可以修改的。
const USER = { name: "Alice" };
USER.name = "Bob"; // 这是合法的
// USER = { name: "Charlie" }; // 这是非法的
```

## 三、JS 与 HTML 的关联

JS 代码可以通过以下三种方式与 HTML 页面关联和执行：

### 3.1 嵌入 HTML 文件 (内联)

将 JS 代码直接写在 `<script>` 标签内部。

```html
<body>
    <h1>欢迎学习 JavaScript</h1>
    <script>
        var age = 20;
        console.log("我的年龄是：" + age);
    </script>
</body>
```

### 3.2 引入本地独立 JS 文件 (外部)

通过 `<script>` 标签的 `src` 属性引入本地的 `.js` 文件。

```html
<!-- HTML 文件 -->
<script src="./index.js"></script> 
```

**引入位置建议：**

*   **传统最佳实践：** 建议将 `<script src="...">` 放在 `<body>` 标签的**末尾**。
    *   **原因：** 浏览器加载 HTML 是从上到下的。如果 JS 文件较大，放在头部会阻塞 DOM 的构建和渲染，导致用户看到空白页面的时间延长。放在末尾可以确保 DOM 元素先加载完成。
*   **现代解决方案：** 在 `<head>` 中引入时，可以添加 `defer` 或 `async` 属性。
    *   `<script src="index.js" defer></script>`: 脚本异步加载，但会等到 HTML 解析完毕后、`DOMContentLoaded` 事件之前执行（保持执行顺序）。推荐用于不依赖其他脚本的普通脚本。
    *   `<script src="index.js" async></script>`: 脚本异步加载和执行（不保证执行顺序）。推荐用于独立、不依赖 DOM 或其他脚本的工具或统计脚本。

### 3.3 引入网源来源文件 (CDN)

使用 **内容分发网络 (CDN, Content Delivery Network)** 提供的资源链接，实现网络加速和资源共享。

```html
<script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.7.1/jquery.js"></script>
```

## 四、数据类型 (Data Types)

JavaScript 有八种内置数据类型。

| 类型分类 | 数据类型 | 描述 | 示例 |
| :--- | :--- | :--- | :--- |
| **原始类型** (Primitive) | **String** (字符串) | 文本数据，用于表示字符序列。 | `"Hello"`, `'JS'` |
| | **Number** (数值) | 双精度 64 位浮点数，包括整数和浮点数。 | `10`, `3.14`, `NaN`, `Infinity` |
| | **Boolean** (布尔值) | 逻辑实体，只有两个值：`true` 和 `false`。 | `true`, `false` |
| | **Null** (空值) | 表示一个空对象指针，只有一个值 `null`。 | `var x = null;` |
| | **Undefined** (未定义) | 变量已声明但未赋值时的默认值，只有一个值 `undefined`。 | `var y;` |
| | **Symbol** (符号) | **(ES6新增)** 独一无二的值，常用于对象的唯一属性键。 | `Symbol('id')` |
| | **BigInt** (大整数) | **(ES11新增)** 用于表示和操作大于 `2^53 - 1` 的整数。 | `12345678901234567890n` |
| **合成类型** (Composite) | **Object** (对象) | 键值对的集合，以及函数的集合。包括普通对象、数组、函数等。 | `{}`, `[]`, `new Date()` |

#### 原始类型 vs 合成类型

*   **原始类型 (基础类型)：** 数据直接存储在**栈 (Stack)** 中，值是不可变的。赋值时是**值拷贝**。
*   **合成类型 (复合类型)：** 数据存储在**堆 (Heap)** 中，而栈中存储的是指向堆中数据的**引用地址**。赋值时是**引用拷贝**。

#### 对象 (Object) 示例

对象是键值对的无序集合。

```js
var user = {
    name: "it",
    age: 10,
    flag: false
};

// 访问属性
console.log(user.name); // "it"
console.log(user["age"]); // 10
```

## 五、数组操作 (Array Operations)

数组是对象的一种，是一种有序的、键为数字的集合。

### 5.1 数组排序 (Array Sorting)

使用数组的 `sort()` 方法。默认情况下，`sort()` 会将元素转为字符串并按 Unicode 码点进行排序。要实现数字排序，必须传入一个**比较函数**。

```js
const numbers = [40, 1, 5, 200];

// 默认排序 (按字符串排序)
numbers.sort();
console.log(numbers); // [1, 200, 40, 5]

// 数字升序排序
numbers.sort((a, b) => a - b);
console.log(numbers); // [1, 5, 40, 200]
```

### 5.2 数组去重 (Array Deduplication)

最现代且简洁的方法是利用 ES6 的 **`Set`** 数据结构，它只允许存储唯一值。

```js
const arrayWithDuplicates = [1, 2, 2, 'a', 'a', 3, 1];

// 1. 使用 Set 去重
const uniqueArray = [...new Set(arrayWithDuplicates)]; 
console.log(uniqueArray); // [1, 2, 'a', 3]

// 2. 也可以使用 Array.from()
// const uniqueArray = Array.from(new Set(arrayWithDuplicates));
```

## 六、DOM 编程 (Document Object Model)

**DOM** 是 JavaScript 操作 HTML/XML 网页的**编程接口 (API)**。它将整个页面解析成一个由节点 (Node) 组成的**树形结构**。

### 6.1 Node 的七种主要类型

在 DOM 树中，所有的组成部分都是节点 (Node)。最常见的节点类型有：

1.  **Document 节点 (文档节点)：** 整个文档的根节点。通过它来获取其他所有元素。
2.  **Element 节点 (元素节点)：** 对应 HTML 标签，如 `<body>`, `<div>`, `<p>` 等。
3.  **Text 节点 (文本节点)：** 标签内部的文本内容。
4.  **Attribute 节点 (属性节点)：** 标签的属性，如 `id="main"`, `class="btn"`。
5.  **Comment 节点 (注释节点)：** HTML 注释。

### 6.2 Element 元素节点

**Element 对象**对应网页中的 HTML 元素。每一个 HTML 元素，在 DOM 树上都会转换成一个 Element 节点对象。

#### 获取元素

| 方法 | 描述 | 示例 |
| :--- | :--- | :--- |
| `document.getElementById()` | 通过元素的 `id` 属性获取单个元素。 | `document.getElementById('header')` |
| `document.querySelector()` | 通过 CSS 选择器获取**第一个**匹配的元素。 | `document.querySelector('.container p')` |
| `document.querySelectorAll()` | 通过 CSS 选择器获取**所有**匹配的元素（返回 NodeList）。 | `document.querySelectorAll('ul li')` |

#### 6.2.1 节点内容操作属性

| 属性 | 描述 | 差异 |
| :--- | :--- | :--- |
| **`textContent`** | 返回当前节点及其所有后代节点的**纯文本内容**。 | 不包含 HTML 标签，性能最优。 |
| **`innerHTML`** | 返回或设置当前节点及其所有后代节点的**全部 HTML 内容**。 | 包含 HTML 标签，可用于设置新的 HTML 结构。 |

```js
/* 假设 HTML 结构为：<div id="box"><span>Hello</span> World!</div> */
const box = document.getElementById('box');

console.log(box.textContent); // "Hello World!"
console.log(box.innerHTML);   // "<span>Hello</span> World!"

// 设置新的内容
box.innerHTML = '<strong>New Content</strong>';
```

### 6.3 Attribute 属性操作接口

`Element` 节点提供了一组统一的接口来操作 HTML 属性。

1.  **`getAttribute(name)`：** 获取指定属性的值。
2.  **`setAttribute(name, value)`：** 设置指定属性的值。如果属性不存在，则创建。
3.  **`hasAttribute(name)`：** 检查元素是否包含指定属性。
4.  **`removeAttribute(name)`：** 移除指定属性。

```js
const btn = document.querySelector('button');

// 设置属性
btn.setAttribute('data-id', '123');

// 获取属性
const id = btn.getAttribute('data-id'); 
console.log(id); // "123"

// 移除属性
btn.removeAttribute('data-id');
```

## 七、DOM 与 CSS 样式操作

JS 主要通过两种方式操作元素的样式：

### 7.1 操作内联样式 (`.style`)

直接修改元素的 `style` 属性。适用于少量、动态的样式修改。

```js
const element = document.getElementById('myDiv');

// 注意：CSS 属性的连字符命名（如 background-color）需转换为驼峰命名（backgroundColor）。
element.style.backgroundColor = 'blue';
element.style.fontSize = '18px';
```

### 7.2 操作类名 (`.classList`)

通过修改元素的 `class` 属性来批量修改样式，这是最推荐的做法。

| 方法 | 描述 | 示例 |
| :--- | :--- | :--- |
| **`add()`** | 添加一个或多个类名。 | `element.classList.add('active', 'highlight')` |
| **`remove()`** | 移除一个或多个类名。 | `element.classList.remove('active')` |
| **`toggle()`** | 切换类名（有则移除，无则添加）。 | `element.classList.toggle('hidden')` |
| **`contains()`** | 检查元素是否包含指定类名。 | `element.classList.contains('active')` |

## 八、事件处理程序

**事件**是浏览器或用户执行的动作，如点击、加载页面、按下键盘等。JS 通过**事件处理程序 (Event Handler)** 对这些事件做出响应。

### 8.1 事件模型：DOM Level 0 vs DOM Level 2

#### 8.1.1 DOM Level 0 事件 (DOM0)

通过 HTML 属性或 JS 对象的属性直接赋值。

*   **特点：** 简单，但**每个事件类型只能注册一个处理函数**，新的赋值会覆盖旧的。

```js
// 1. 通过 HTML 属性
// <button onclick="alert('Clicked!')">Click Me</button>

// 2. 通过 JS 对象属性
const button = document.getElementById('btn');
button.onclick = function() {
    console.log('DOM0 - First Handler');
};
// 再次赋值会覆盖上一个
button.onclick = function() {
    console.log('DOM0 - Second Handler'); // 只有这个会被执行
};
```

#### 8.1.2 DOM Level 2 事件 (DOM2)

使用 `addEventListener()` 和 `removeEventListener()` 方法。

*   **特点：** 允许多个处理函数注册到同一个事件上，**不会相互覆盖**。支持**事件捕获 (capture)** 和**事件冒泡 (bubble)**。

```js
const button = document.getElementById('btn');

// addEventListener(type, listener, useCapture)
button.addEventListener('click', function() {
    console.log('DOM2 - First Handler');
}, false); // false 代表在冒泡阶段触发

button.addEventListener('click', function() {
    console.log('DOM2 - Second Handler'); // 两个都会被执行
}, false);
```

### 8.2 常见事件类型

| 事件分类 | 描述 | 常见事件 |
| :--- | :--- | :--- |
| **鼠标事件** | 用户使用鼠标执行的动作。 | `click` (点击), `dblclick` (双击), `mouseover` (移入), `mouseout` (移出) |
| **键盘事件** | 用户操作键盘时的动作。 | `keydown` (键按下), `keyup` (键松开), `keypress` (键按住，已弃用) |
| **表单事件** | 用户与表单控件交互时的动作。 | `submit` (提交), `change` (值改变), `focus` (获取焦点), `blur` (失去焦点) |
| **触摸事件** | 在支持触摸屏的设备上的动作。 | `touchstart`, `touchmove`, `touchend` |
| **文档/窗口事件** | 浏览器或文档状态的改变。 | `load` (页面加载完成), `scroll` (滚动), `resize` (窗口大小改变) |

## 九、事件代理与事件委托

### 9.1 原理分析

**事件冒泡 (Event Bubbling)：** 当一个元素上的事件被触发时，该事件会从该元素（目标元素）开始，沿着 DOM 树向上传播到其父元素、祖父元素，直至 `document` 或 `window`。

**事件捕获 (Event Capturing)：** 事件从 `window` 开始，沿着 DOM 树向下传播到目标元素。

### 9.2 事件代理/事件委托 (Event Delegation)

**定义：** 事件委托是利用**事件冒泡**的机制，将子元素（如列表项 `<li>`）的事件监听器注册到它们的**父元素**（如列表容器 `<ul>`）上。

**优势：**

1.  **性能优化：** 只需要注册一个监听器，而不是为每个子元素注册一个，大大减少了内存开销。
2.  **动态元素：** 对于后来通过 JS 动态添加到 DOM 中的子元素，无需重新绑定事件，因为父元素的监听器已经存在并能捕获到新元素的事件。

### 9.3 示例代码

假设有一个包含大量列表项的列表，我们想为每个列表项添加点击事件。

```html
<!-- HTML 结构 -->
<ul id="myList">
    <li data-id="1">Item 1</li>
    <li data-id="2">Item 2</li>
    <li data-id="3">Item 3</li>
</ul>
```

#### 传统方式 (为每个元素绑定)

```js
// 缺点：如果列表有1000个项，就要注册1000个监听器
const items = document.querySelectorAll('#myList li');
items.forEach(item => {
    item.addEventListener('click', function() {
        console.log('Clicked:', this.textContent);
    });
});
```

#### 事件代理 (推荐方式)

```js
const list = document.getElementById('myList');

// 只需要为父元素绑定一个监听器
list.addEventListener('click', function(event) {
    // 1. 检查事件的实际目标 (event.target) 是否是我们感兴趣的元素 (li)
    if (event.target.tagName === 'LI') {
        const itemId = event.target.getAttribute('data-id');
        console.log('代理点击成功，目标是 Item ID:', itemId);
        
        // 可执行的操作，如跳转或数据请求
    }
});
```