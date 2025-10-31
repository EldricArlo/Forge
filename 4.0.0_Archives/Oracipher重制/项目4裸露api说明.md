好的，我们来剖析 `PasswordManager-Browser-Extension` 项目的 "API"。

这个项目的 "API" 是所有子项目中**最复杂、也最独特**的。因为它不是一个单一的应用，而是由多个独立但需要互相通信的组件构成的集合。它的 API 本质上是**一套定义了这些组件如何互相交谈的消息传递协议和接口**。

我们将这个项目看作一个微型分布式系统，其核心是一个**后台服务工作线程 (Background Service Worker)**，作为所有通信的中央枢G纽。

---

### **核心架构: 一个 "远程控制"**

最重要的一点是：为了极致的安全，浏览器扩展本身应该被设计成一个**“哑客户端” (Dumb Client)** 或**“远程控制”**。它**不应该**直接包含 `Frontend-SDK` (项目2) 或在浏览器中处理任何敏感的加密/解密操作。

所有的核心逻辑都委托给已经安装的、更安全的 `PasswordManager-App-Main` (项目3) 桌面应用来执行。

这种架构的 "API" 就是连接以下三个部分的桥梁：
1.  **UI 层 (Flutter Web)**: 用户看到的弹出窗口。
2.  **内容脚本 (Content Script - JavaScript)**: 注入到网页中，负责与页面交互。
3.  **后台服务 (Background Service Worker - JavaScript)**: 扩展的大脑，管理状态和通信。
4.  **原生应用 (Native App - 项目3)**: 最终的逻辑执行者。

---

### **暴露的 API (消息传递接口)**

#### 1. UI 层 (Flutter) ⇌ 后台服务 (JS) 的 API

这是 Flutter UI 与扩展大脑之间的通信。

*   **实现方式**: 在 Flutter 端创建一个 `BrowserApiBridge.dart` 类，使用 `dart:js` 包来调用 `chrome.runtime.sendMessage()`，并监听 `chrome.runtime.onMessage`。

*   **`BrowserApiBridge` 暴露给 Flutter UI 的方法**:
    *   `Future<bool> isUnlocked()`: 检查密码库当前是否已解锁。
    *   `Future<void> unlock(String masterPassword)`: 发送主密码给后台，请求原生应用解锁。
    *   `Future<void> lock()`: 请求锁定密码库。
    *   `Future<List<VaultItem>> getItemsForCurrentTab()`: 获取与当前浏览器标签页URL匹配的登录凭证。
    *   `Future<List<VaultItem>> searchItems(String query)`: 搜索整个密码库。
    *   `Future<String> generatePassword()`: 请求生成一个新密码。
    *   `Future<void> requestFill(VaultItem item)`: 请求后台让内容脚本填充选定的项目。
    *   `Stream<bool> onLockStateChanged()`: 监听锁定状态的变化（例如，超时自动锁定）。

#### 2. 内容脚本 (JS) ⇌ 后台服务 (JS) 的 API

这是注入到网页的脚本与扩展大脑之间的通信。

*   **内容脚本 -> 后台服务 (报告与请求)**:
    *   `reportLoginFormPresence({ formDetails })`: 当在页面上检测到登录表单时，向后台报告表单的结构信息。
    *   `requestCredentialsForFill()`: 当用户点击输入框中的小图标时，请求后台提供可用于填充的凭证。
    *   `reportCredentialsSubmitted({ username, password })`: 当用户提交了一个表单后，向后台报告用户输入的用户名和密码，用于触发“是否保存密码”的提示。

*   **后台服务 -> 内容脚本 (命令)**:
    *   `fillCredentials({ username, password })`: 命令内容脚本将指定的用户名和密码填充到页面表单中。
    *   `displaySavePasswordPrompt({ username })`: 命令内容脚本在页面顶部显示一个“是否为此网站保存密码？”的UI横幅。
    *   `displayInlineIcon({ field })`: 命令内容脚本在指定的输入框旁边显示我们应用的小图标。

#### 3. 后台服务 (JS) ⇌ 原生应用 (项目3) 的 API

这是最关键的安全边界，通过**原生消息传递 (Native Messaging)** 实现。后台服务将所有敏感操作都委托给原生桌面应用。

*   **后台服务 -> 原生应用 (委托任务)**:
    *   `{ "command": "unlock", "payload": { "masterPassword": "..." } }`: 发送主密码请求解锁。
    *   `{ "command": "lock" }`: 请求锁定。
    *   `{ "command": "getStatus" }`: 获取当前状态（锁定/解锁）。
    *   `{ "command": "getVaultData" }`: 请求获取完整的（仍是加密的）密码库数据。
    *   `{ "command": "updateVaultData", "payload": { "encryptedVault": "..." } }`: 发送更新后的加密数据 blob。
    *   `{ "command": "generatePassword" }`: 请求生成密码。
    *   `{ "command": "getMatchingItems", "payload": { "url": "https://..." } }`: 请求原生应用返回与某个URL匹配的条目。

*   **原生应用 -> 后台服务 (返回结果)**:
    *   `{ "status": "success", "result": { "unlocked": true } }`
    *   `{ "status": "error", "message": "Invalid master password" }`
    *   `{ "status": "success", "result": { "items": [...] } }`
    *   `{ "event": "lockStateChanged", "payload": { "isUnlocked": false } }`: 这是原生应用主动推送的事件，例如当桌面应用被用户手动锁定时，可以通知浏览器扩展。

### **总结**

这个项目的 "API" 不是单一的类库，而是一个定义了**职责**和**通信协议**的架构。

| 通信方 | 接口/协议 | 核心职责 |
| :--- | :--- | :--- |
| **Flutter UI (Popup)** | `BrowserApiBridge.dart` | **展示数据**和**发送用户意图**给后台服务。 |
| **JS 内容脚本** | 消息协议 (e.g., `reportLoginFormPresence`) | **感知网页** (DOM)，并作为后台服务在页面上的**手和眼**。 |
| **JS 后台服务** | 消息路由和 Native Messaging 协议 | **中央协调器**。接收来自UI和内容脚本的请求，并安全地将核心任务**委托**给原生应用。 |
| **原生应用 (项目3)** | Native Messaging JSON 协议 | **安全大脑**。执行所有加密、解密和数据管理操作，并将结果返回给后台服务。 |

通过这种方式，我们将风险最高的浏览器环境中的代码逻辑减到最少，所有信任和安全都建立在与原生桌面应用的隔离通信之上，这是构建安全浏览器扩展的最佳实践。