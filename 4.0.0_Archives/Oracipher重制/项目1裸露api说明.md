

以下是 `PasswordManager-Backend-Core` 项目可能需要暴露的 API 列表，遵循了 RESTful 设计原则和零知识安全模型。

---

### **通用原则 / 安全考量**

1.  **强制 HTTPS**: 所有 API 通信必须通过 HTTPS (TLS) 进行加密，以防止中间人攻击。
2.  **零知识 (Zero-Knowledge)**: 服务器 **绝不** 接收或存储用户的明文主密码或任何可以解密用户数据的密钥。所有与密码相关的字段（如 `master_password_hash`）都是客户端经过密钥派生函数（如 Argon2）处理后的结果。
3.  **身份验证**: 除注册和登录等少数公共端点外，所有 API 请求都必须在 HTTP Header 中包含一个有效的 `Authorization: Bearer <JWT_ACCESS_TOKEN>`。
4.  **JSON 格式**: 所有请求体 (Request Body) 和响应体 (Response Body) 都使用 JSON 格式。

---

### **API 端点详解**

我们将 API 分为两大类：**账户与认证 (Auth & Accounts)** 和 **密码库 (Vault)**。

#### **1. 账户与认证 (Auth & Accounts)**

这些端点负责管理用户身份。

##### **`POST /auth/register`**
*   **描述**: 创建一个新用户账户。
*   **请求体**:
    ```json
    {
      "email": "user@example.com",
      "master_password_hash": "argon2_hashed_string_from_client", // 用于验证的哈希
      "encryption_salt": "random_salt_for_key_derivation" // 用于派生加密密钥的盐值
    }
    ```
*   **授权**: 无需。
*   **成功响应 (201 Created)**:
    ```json
    {
      "message": "User created successfully. Please log in."
    }
    ```
*   **错误响应**:
    *   `400 Bad Request`: 如果请求体格式错误或缺少字段。
    *   `409 Conflict`: 如果该 `email` 已被注册。

##### **`POST /auth/login`**
*   **描述**: 用户登录，获取用于后续 API 请求的访问令牌。
*   **请求体**:
    ```json
    {
      "email": "user@example.com",
      "master_password_hash": "argon2_hashed_string_from_client"
    }
    ```
*   **授权**: 无需。
*   **成功响应 (200 OK)**:
    ```json
    {
      "access_token": "your_jwt_access_token",
      "refresh_token": "your_jwt_refresh_token"
    }
    ```
*   **错误响应**:
    *   `401 Unauthorized`: 如果 `email` 或 `master_password_hash` 不匹配。

##### **`POST /auth/token/refresh`**
*   **描述**: 使用一个长期有效的刷新令牌 (refresh token) 来获取一个新的访问令牌 (access token)，避免用户频繁重新登录。
*   **请求体**:
    ```json
    {
      "refresh_token": "your_jwt_refresh_token"
    }
    ```
*   **授权**: 无需。
*   **成功响应 (200 OK)**:
    ```json
    {
      "access_token": "your_new_jwt_access_token"
    }
    ```
*   **错误响应**:
    *   `401 Unauthorized`: 如果 `refresh_token` 无效或已过期。

##### **`GET /accounts/me`**
*   **描述**: 获取当前已登录用户的账户信息。
*   **请求体**: 无。
*   **授权**: **需要** (`Bearer <access_token>`)。
*   **成功响应 (200 OK)**:
    ```json
    {
      "email": "user@example.com",
      "created_at": "2025-10-26T10:00:00Z",
      "encryption_salt": "random_salt_for_key_derivation" // 客户端在新设备上登录时需要此盐值来重新生成加密密钥
    }
    ```

##### **`DELETE /accounts/me`**
*   **描述**: 永久删除当前用户的账户及其所有数据。这是一个高风险操作。
*   **请求体**: 无。
*   **授权**: **需要** (`Bearer <access_token>`)。
*   **成功响应 (204 No Content)**: 无响应体。
*   **错误响应**:
    *   `401 Unauthorized`: Token无效。

---

#### **2. 密码库 (Vault)**

这些端点负责对用户的加密数据进行操作。服务器将这些数据视为一个无法解密的“二进制大对象 (blob)”。

##### **`GET /vault`**
*   **描述**: 获取用户完整的加密密码库。
*   **请求体**: 无。
*   **授权**: **需要** (`Bearer <access_token>`)。
*   **成功响应 (200 OK)**:
    ```json
    {
      "encrypted_vault": "a_very_long_base64_encoded_encrypted_string",
      "version": 1672531200 // 一个时间戳或版本号，用于处理同步冲突
    }
    ```
*   **错误响应**:
    *   `404 Not Found`: 如果用户还没有创建任何数据。

##### **`PUT /vault`**
*   **描述**: 更新（替换）用户完整的加密密码库。这是最简单和最安全的同步策略：客户端下载、在本地解密、修改、重新加密整个库，然后上传替换旧的。
*   **请求体**:
    ```json
    {
      "encrypted_vault": "new_base64_encoded_encrypted_string",
      "version": 1672531200 // 客户端必须提供它所基于的旧版本号
    }
    ```
*   **授权**: **需要** (`Bearer <access_token>`)。
*   **成功响应 (200 OK)**:
    ```json
    {
      "message": "Vault updated successfully.",
      "new_version": 1672531225 // 返回一个新的版本号
    }
    ```
*   **错误响应**:
    *   `400 Bad Request`: 请求体格式错误。
    *   `409 Conflict`: 如果请求中提供的 `version` 与服务器上当前的 `version` 不匹配，说明在本地修改期间，已有其他设备更新了密码库。客户端需要重新 `GET` 最新数据，合并更改后重试。

### **总结**

这个核心后端的 API 设计非常精简，因为它严格遵守了零知识原则。服务器的职责被简化为两个核心：

1.  **“守门人”**: 通过账户系统确认用户的身份。
2.  **“保险箱”**: 负责安全地保管一个它自己打不开的加密箱子 (`encrypted_vault`)。

所有复杂的业务逻辑，如解析密码条目、加解密、TOTP 生成等，都被推到了客户端（**项目 2: `Frontend-SDK`**）去完成，这正是零知识架构安全性的根基所在。