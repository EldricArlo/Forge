非常好。对于 `PasswordManager-App-Main` 这个项目，我们讨论的 "API" 不再是网络接口或库接口，而是**应用内部的架构分层和组件接口**。

这个项目是 `Frontend-SDK` (项目2) 的直接消费者。它的主要任务是构建用户界面 (UI) 并响应用户操作。因此，它的 "API" 可以理解为**UI层（Widgets）与业务逻辑层（ViewModels/Controllers/State Notifiers）之间的契约**。

我们将采用一个现代的、响应式的架构模式（如 MVVM、BLoC 或 Riverpod），这在 Flutter 开发中是标准实践。在这个模式下，UI 只负责展示数据和发送事件，而 ViewModel (或类似角色的类) 负责处理这些事件、与 SDK 交互并管理 UI 状态。

以下是 `PasswordManager-App-Main` 项目中，各个功能模块需要暴露给 UI 层的 ViewModel/Notifier 的 "API"。

---

### **核心架构**

*   **UI (Widgets)**: Flutter 的界面元素，负责渲染。
*   **ViewModels / State Notifiers**: UI 的状态持有者和逻辑处理器。这是我们定义的 "API" 所在层。
*   **Service Layer**: 我们的 **`PasswordManager-Frontend-SDK` (项目 2)**，由 ViewModels 调用。

---

### **各功能模块的 ViewModel "API"**

#### 1. 认证流程 (`AuthViewModel`)
*   **职责**: 管理登录、注册、解锁应用的整个流程。
*   **状态 (UI 可以监听的数据)**:
    *   `isLoading`: `bool` - 是否正在进行网络请求（如登录）。
    *   `errorMessage`: `String?` - 如果发生错误，显示的错误信息。
*   **暴露的方法 (UI 可以调用的操作)**:
    *   `Future<void> login(String email, String masterPassword)`: 处理登录逻辑。
    *   `Future<void> register(String email, String masterPassword)`: 处理注册逻辑。
    *   `Future<void> logout()`: 处理登出逻辑。

#### 2. 主密码库列表 (`VaultListViewModel`)
*   **职责**: 显示所有密码条目，并处理搜索、过滤和排序。
*   **状态**:
    *   `items`: `List<VaultItem>` - 从 SDK 获取并展示的所有条目。
    *   `filteredItems`: `List<VaultItem>` - 根据搜索词过滤后的条目。
    *   `isLoading`: `bool` - 是否正在从 SDK 加载数据。
    *   `searchQuery`: `String` - 当前的搜索关键词。
*   **暴露的方法**:
    *   `void search(String query)`: 更新搜索关键词并刷新 `filteredItems`。
    *   `Future<void> deleteItem(String itemId)`: 请求删除一个条目。

#### 3. 条目详情与编辑器 (`ItemEditorViewModel`)
*   **职责**: 创建、查看和编辑单个密码库条目（登录、银行卡、笔记等）。
*   **状态**:
    *   `currentItem`: `VaultItem?` - 正在编辑或查看的条目。
    *   `isEditing`: `bool` - 当前是编辑模式还是查看模式。
    *   `isNew`: `bool` - 是否正在创建一个新条目。
    *   `canSave`: `bool` - 表单是否有效，能否进行保存。
*   **暴露的方法**:
    *   `void loadItem(String? itemId)`: 加载一个已存在的条目，或准备创建一个新条目。
    *   `void updateField({String fieldName, dynamic value})`: 当用户在表单中输入时，更新 `currentItem` 的字段。
    *   `Future<void> saveItem()`: 保存更改（调用 SDK 的 `addItem` 或 `updateItem`）。
    *   `String generatePassword()`: 调用密码生成器工具，返回一个强密码。

#### 4. TOTP (二次验证码) 管理 (`TotpViewModel`)
*   **职责**: 为单个条目显示和管理动态的二次验证码。
*   **状态**:
    *   `currentCode`: `String` - 当前生成的6位数字码。
    *   `secondsRemaining`: `int` - 当前验证码的剩余有效秒数。
    *   `progress`: `double` - 0.0 到 1.0 的进度，用于 UI 上的圆形进度条。
*   **暴露的方法**:
    *   `void start(String totpSecret)`: 使用条目的 TOTP 密钥开始生成和计时。
    *   `void copyToClipboard()`: 将 `currentCode` 复制到剪贴板。

#### 5. 安全仪表盘 (`SecurityDashboardViewModel`)
*   **职责**: 分析整个密码库，并以可视化的方式展示安全报告。
*   **状态**:
    *   `securityScore`: `int` - 0到100的整体安全分数。
    *   `weakPasswords`: `List<VaultItem>` - 存在弱密码的条目列表。
    *   `reusedPasswords`: `List<VaultItem>` - 存在重复使用密码的条目列表。
    *   `leakedPasswords`: `List<VaultItem>` - 出现在已知泄露事件中的条目列表。
    *   `isLoading`: `bool` - 是否正在分析密码库。
*   **暴露的方法**:
    *   `Future<void> analyzeVault()`: 触发一次完整的密码库分析。

#### 6. 数据工具 (`DataToolsViewModel`)
*   **职责**: 处理数据的导入和导出。
*   **状态**:
    *   `importProgress`: `double` - 导入过程的进度（0.0 - 1.0）。
    *   `exportStatus`: `String` - "Idle", "Exporting", "Success", "Error"。
    *   `statusMessage`: `String` - 显示给用户的当前状态信息。
*   **暴露的方法**:
    *   `Future<void> importFromCsv()`: 触发文件选择器，并开始 CSV 导入流程。
    *   `Future<void> importFromBrowser(BrowserType browser)`: 从特定浏览器导出的文件导入。
    *   `Future<void> exportToCsv()`: 将整个密码库导出为加密或明文的 CSV 文件。

#### 7. 设置 (`SettingsViewModel`)
*   **职责**: 管理应用的所有用户可配置项。
*   **状态**:
    *   `currentTheme`: `ThemeMode` - 当前的主题（亮色/暗色/跟随系统）。
    *   `isBiometricsEnabled`: `bool` - 是否启用了生物识别解锁。
    *   `selfDestructAttempts`: `int` - 触发自毁的连续错误尝试次数。
    *   `isTravelModeEnabled`: `bool` - 是否激活了旅行模式。
*   **暴露的方法**:
    *   `void setTheme(ThemeMode theme)`: 更改应用主题。
    *   `Future<void> setBiometrics(bool enabled)`: 开启或关闭生物识别。
    *   `void setSelfDestruct(int attempts)`: 设置自毁模式的阈值。
    *   `void toggleTravelMode()`: 切换旅行模式。

### **总结**

这个项目的 "API" 是一个组织良好、职责分离的内部架构。UI层通过这些 ViewModels 来驱动，实现了界面与逻辑的解耦。

| UI 组件 / 屏幕 | 对应的 ViewModel | 核心交互 |
| :--- | :--- | :--- |
| 登录/注册页 | `AuthViewModel` | 调用 `login()`, `register()` 方法，监听 `isLoading` 状态 |
| 密码列表主页 | `VaultListViewModel` | 监听 `filteredItems` 来构建列表，调用 `search()` 方法 |
| 新建/编辑密码页 | `ItemEditorViewModel` | 调用 `loadItem()`, `saveItem()`，并双向绑定表单数据 |
| 密码详情页的2FA码 | `TotpViewModel` | 调用 `start()`，并监听 `currentCode` 和 `progress` 来更新UI |
| 安全中心页 | `SecurityDashboardViewModel`| 调用 `analyzeVault()`，并根据分析结果 `weakPasswords` 等展示数据 |
| 导入/导出工具页 | `DataToolsViewModel` | 调用 `import...()` 或 `export...()` 方法，监听进度 |
| 设置页 | `SettingsViewModel` | 读取状态来显示开关，调用 `set...()` 方法来更新设置 |