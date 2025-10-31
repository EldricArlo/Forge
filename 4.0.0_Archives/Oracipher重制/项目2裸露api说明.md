好的，这是一个非常核心的问题。为 `PasswordManager-Frontend-SDK` 设计 "API" 与后端不同，这里的 "API" 指的是它向应用层（项目3 和 项目4）暴露的**公共类、方法和属性**。这是一个软件开发工具包 (SDK)，而不是一个网络服务。

这个 SDK 的设计目标是：**为 UI 开发者提供一个极其简单、类型安全的接口，同时将所有复杂的加密、状态管理、本地存储和网络通信逻辑完全封装在内部**。UI 开发者调用 `sdk.login()` 时，不应该关心背后发生了多少复杂的加密和网络步骤。

以下是 `PasswordManager-Frontend-SDK` 可能需要暴露的、经过精心设计的公共 API。

---

### **核心设计原则**

1.  **单一入口 (Facade Pattern)**: UI 层应该只与一个主服务类（例如 `PasswordManagerService`）进行交互，这个主服务类会协调内部的其他子系统。
2.  **响应式 (Reactive)**: SDK 的状态变化（如登录成功、数据更新、同步开始/结束）应该通过 **Streams** 或 **Notifiers** (如 `ChangeNotifier`) 暴露出来，以便 Flutter UI 可以自动响应并刷新。
3.  **异步操作**:所有可能耗时（网络、加密、磁盘I/O）的操作都必须是异步的，返回 `Future`。
4.  **强类型数据模型**: 所有密码库中的条目都应该是具体的 Dart 类，而不是松散的 `Map`。

---

### **暴露的公共 API (Public Interface)**

我们将 API 分为几个逻辑部分：主服务、认证、密码库操作、状态和数据模型。

#### **1. 主服务入口 (`PasswordManagerService`)**

这是 UI 层唯一需要直接实例化的类。

```dart
class PasswordManagerService {
  // --- 子服务 (对UI层隐藏，但由主服务管理) ---
  // final AuthService _authService;
  // final VaultService _vaultService;
  // final SyncService _syncService;

  // --- 公共的 Getter ---
  // 方便UI直接访问各个功能模块
  AuthService get auth => _authService;
  VaultService get vault => _vaultService;
  SyncService get sync => _syncService;

  /// 初始化SDK
  /// 必须在应用启动时调用一次。
  /// 它会尝试从本地安全存储加载数据和登录状态。
  Future<void> initialize();

  /// 销毁SDK实例，清除所有敏感数据
  /// 在用户退出应用时调用。
  Future<void> dispose();
}
```

---

#### **2. 认证服务 API (`AuthService`)**

负责所有与用户身份验证相关的操作。

```dart
abstract class AuthService {
  /// 监听认证状态的变化 (e.g., LoggedIn, LoggedOut)
  Stream<AuthState> get onAuthStateChanged;

  /// 获取当前的认证状态
  AuthState get currentState;

  /// 检查用户是否已登录
  bool get isLoggedIn;

  /// 用户注册
  /// 内部处理：生成盐值、派生密钥、调用后端 API
  Future<void> register({required String email, required String masterPassword});

  /// 用户登录
  /// 内部处理：调用登录API、获取token、获取并解密云端密码库、存入本地
  Future<void> login({required String email, required String masterPassword});

  /// 用户登出
  /// 内部处理：清除所有敏感数据（token, 解密的密码库, 加密密钥）
  Future<void> logout();
}
```

---

#### **3. 密码库服务 API (`VaultService`)**

负责对**已解密**的密码库数据进行本地的 CRUD 操作。

```dart
abstract class VaultService {
  /// 监听密码库的变化
  /// 每当有条目增、删、改时，这个流就会发出一个包含所有条目的新列表。
  /// UI层可以监听这个流来自动刷新列表。
  Stream<List<VaultItem>> get onVaultChanged;

  /// 获取当前所有的密码库条目
  List<VaultItem> getAllItems();

  /// 根据ID获取单个条目
  VaultItem? getItemById(String id);

  /// 添加一个新的条目到密码库
  /// 内部处理：更新内存状态、写入本地数据库、标记为需要同步
  Future<void> addItem(VaultItem item);

  /// 更新一个已存在的条目
  Future<void> updateItem(VaultItem item);

  /// 根据ID删除一个条目
  Future<void> deleteItem(String id);
}
```

---

#### **4. 同步服务 API (`SyncService`)**

负责本地数据与云端服务器的同步。

```dart
abstract class SyncService {
  /// 监听同步状态的变化 (e.g., Idle, Syncing, Synced, Error)
  Stream<SyncState> get onSyncStateChanged;

  /// 获取当前的同步状态
  SyncState get currentState;

  /// 手动触发一次与云端的同步
  /// 内部处理：
  /// 1. 加密整个本地密码库。
  /// 2. 调用后端 PUT /vault API。
  /// 3. 处理成功或冲突(409)的情况。
  /// 4. 如果有冲突，则自动拉取、合并、再尝试推送。
  Future<void> sync();
}
```

---

#### **5. 公共数据模型 (Data Models)**

这些是 UI 层会直接使用到的数据结构。

```dart
// 状态枚举
enum AuthState { LoggedIn, LoggedOut, Unknown }
enum SyncState { Idle, Syncing, Synced, Error }

// 密码库条目的基类
abstract class VaultItem {
  final String id;
  String name;
  DateTime createdAt;
  DateTime updatedAt;
  // ... 其他公共字段
}

// 具体的条目类型
class LoginItem extends VaultItem {
  String username;
  String password; // 明文密码只存在于内存中
  String website;
  String? totpSecret; // 用于生成二次验证码的密钥
  // ...
}

class CardItem extends VaultItem {
  String cardholderName;
  String cardNumber;
  String expiryMonth;
  String expiryYear;
  String cvv;
  // ...
}

class NoteItem extends VaultItem {
  String content;
  // ...
}
```

### **如何使用 (UI 层的视角)**

UI 开发者在 Flutter Widget 中的使用体验会非常简单：

```dart
// 1. 获取SDK实例 (通常通过依赖注入，如 Riverpod 或 Provider)
final passwordManager = provider.read(passwordManagerServiceProvider);

// 2. 登录
try {
  await passwordManager.auth.login(email: '...', masterPassword: '...');
  // 导航到主页
} catch (e) {
  // 显示错误信息
}

// 3. 在主页的ListView中显示数据
Widget build(BuildContext context) {
  return StreamBuilder<List<VaultItem>>(
    stream: passwordManager.vault.onVaultChanged, // 监听数据变化
    builder: (context, snapshot) {
      if (snapshot.hasData) {
        final items = snapshot.data!;
        return ListView.builder(
          itemCount: items.length,
          itemBuilder: (context, index) {
            return ListTile(title: Text(items[index].name));
          },
        );
      }
      return CircularProgressIndicator();
    },
  );
}

// 4. 添加一个新项目
void onAddNewItem() {
  final newItem = LoginItem(
    id: Uuid().v4(),
    name: 'Google',
    username: 'user@gmail.com',
    password: 'very_secure_password_123'
    //...
  );
  await passwordManager.vault.addItem(newItem);
  // UI 会通过 StreamBuilder 自动刷新，无需手动调用 setState
}
```

这个 SDK 设计将所有复杂性都隐藏了起来，为上层应用提供了一个干净、强大且易于使用的接口。