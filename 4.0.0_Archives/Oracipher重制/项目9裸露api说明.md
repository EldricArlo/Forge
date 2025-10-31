好的，我们来为 `PasswordManager-MaskedEmail-Service` (邮件别名服务) 设计 API。

这个微服务的目标是为用户提供一次性或持久的随机电子邮件地址，以保护他们的真实邮箱。它的 API 设计需要简洁明了，专注于别名的**生成、管理和状态控制**。

我们将主要设计**自建服务**的 API，因为代理第三方服务的 API 会高度依赖于第三方本身。自建服务的 API 会更加灵活和可控。

这个 API 的主要消费者是 `Backend-Core` (项目1)，它会根据用户的订阅状态来决定是否允许调用此服务。

---

### **核心概念**

*   **别名 (Alias)**: 一个随机生成的、属于我们服务域名的邮箱地址，例如 `random-string-123@mask.mypasswordapp.com`。
*   **目标 (Destination)**: 用户的真实邮箱地址，例如 `user-real-email@gmail.com`。
*   **转发 (Forwarding)**: 将发送到“别名”的邮件，投递到“目标”邮箱的过程。

---

### **API 端点详解**

API 分为两个部分：**别名管理 (Alias Management)** 和 **邮件接收 Webhook (Inbound Email Webhook)**。

#### **A. 别名管理 API (由 `Backend-Core` 调用)**

**安全与授权:**
同样，所有请求都需要包含一个服务间的秘密 API 密钥。

##### **`POST /aliases`**
*   **描述**: 为指定用户创建一个新的邮件别名。
*   **调用方**: `Backend-Core` (代表某个用户)。
*   **请求体**:
    ```json
    {
      "user_id": "user-uuid-in-core-db",
      "destination_email": "user-real-email@gmail.com", // 用户的真实邮箱
      "description": "Netflix Subscription", // 用户为这个别名添加的描述
      "prefix": "netflix-", // (可选) 用户自定义的前缀
      "active": true // (可选, 默认true) 创建后是否立即激活
    }
    ```
*   **成功响应 (201 Created)**: 返回新创建的别名的完整信息。
    ```json
    {
      "alias_id": "alias-uuid-123",
      "user_id": "user-uuid-in-core-db",
      "alias_email": "netflix-randomstring@mask.mypasswordapp.com",
      "description": "Netflix Subscription",
      "destination_email": "user-real-email@gmail.com",
      "active": true,
      "forward_count": 0,
      "reply_count": 0,
      "created_at": "..."
    }
    ```

##### **`GET /users/{user_id}/aliases`**
*   **描述**: 获取一个用户拥有的所有邮件别名列表。
*   **成功响应 (200 OK)**:
    ```json
    {
      "aliases": [
        {
          "alias_id": "alias-uuid-123",
          "alias_email": "netflix-randomstring@mask.mypasswordapp.com",
          "description": "Netflix Subscription",
          "active": true,
          "forward_count": 15,
          "created_at": "..."
        },
        {
          "alias_id": "alias-uuid-456",
          "alias_email": "temporary-forum-signup@mask.mypasswordapp.com",
          "description": "Forum Signup",
          "active": false,
          "forward_count": 3,
          "created_at": "..."
        }
      ]
    }
    ```

##### **`GET /aliases/{alias_id}`**
*   **描述**: 获取单个别名的详细信息。
*   **成功响应 (200 OK)**: 返回与 `POST /aliases` 响应体结构相同的详细信息。

##### **`PATCH /aliases/{alias_id}`**
*   **描述**: 更新一个别名的属性，最常见的操作是**激活/禁用**转发。
*   **请求体**:
    ```json
    {
      "description": "Updated Description", // (可选)
      "active": false // (可选) 设为 false 可临时禁用此别名的邮件转发
    }
    ```
*   **成功响应 (200 OK)**: 返回更新后的别名完整信息。

##### **`DELETE /aliases/{alias_id}`**
*   **描述**: 永久删除一个邮件别名。被删除后，所有发往该地址的邮件都将被拒绝。
*   **成功响应 (204 No Content)**: 无响应体。

---

#### **B. 邮件接收 Webhook (由邮件基础设施调用)**

这部分不是一个传统的 REST API，而是一个需要暴露给底层邮件处理系统（如 Amazon SES, Mailgun, Postmark）的 **Webhook 端点**。当我们的域名 (`@mask.mypasswordapp.com`) 收到一封邮件时，邮件系统会向这个端点发送一个包含邮件所有内容的 POST 请求。

##### **`POST /webhooks/inbound-email`**
*   **描述**: 接收一封发送到我们别名域名的邮件。
*   **调用方**: 底层邮件服务 (e.g., Mailgun)。
*   **请求体 (示例 - 格式由邮件服务商定义)**:
    ```json
    {
      "sender": "some-sender@example.net",
      "recipient": "netflix-randomstring@mask.mypasswordapp.com", // 这是关键的别名地址
      "subject": "Your receipt from Netflix",
      "body-plain": "...",
      "body-html": "...",
      "attachments": [...]
      // ... Mailgun 会提供非常多关于邮件的元数据
    }
    ```
    *   **处理逻辑 (服务内部)**:
    1.  验证请求是否来自可信的邮件服务商（通常通过签名验证）。
    2.  从 `recipient` 字段中解析出别名地址。
    3.  查询数据库，找到该别名对应的 `destination_email` 和 `user_id`。
    4.  检查别名的状态是否为 `active`。
    5.  如果激活，则调用邮件服务商的 API，将这封邮件**重新发送**给 `destination_email`。在转发时，可能会修改邮件头，以便用户可以轻松地**回复**。
    6.  更新数据库中该别名的 `forward_count` 计数器。
    7.  向邮件服务商返回 `200 OK`，表示邮件已成功接收和处理。
*   **成功响应 (200 OK)**:
    ```json
    { "status": "ok", "message": "Email received and queued for forwarding." }
    ```

### **总结**

| API 模块 | 端点示例 | HTTP 方法 | 用途 | 调用方 |
| :--- | :--- | :--- | :--- | :--- |
| **别名管理** | `/aliases` | POST | 创建一个新的邮件别名 | `Backend-Core` |
| | `/users/{id}/aliases` | GET | 获取用户的所有别名 | `Backend-Core` |
| | `/aliases/{id}` | PATCH | 启用/禁用或更新一个别名 | `Backend-Core` |
| | `/aliases/{id}` | DELETE | 永久删除一个别名 | `Backend-Core` |
| **邮件接收** | `/webhooks/inbound-email` | POST | 接收发往别名的邮件 | 邮件基础设施 (e.g., Mailgun) |

这个 API 设计使得别名服务的职责非常集中：**管理别名生命周期**和**执行邮件转发逻辑**。它与核心用户系统解耦，可以独立部署和扩展，甚至在未来可以轻松地替换底层的邮件发送服务，而无需更改其对外的 API 契约。