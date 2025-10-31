好的，我们来详细解析 `PasswordManager-BreachWatch-Service` (暗网泄露监控服务) 需要暴露的 API。

这个项目是一个典型的**后端微服务**。它的主要特点是：
*   **非用户直接调用**: 它的 API 不是给前端 Flutter 应用直接调用的，而是给另一个后端服务——`PasswordManager-Backend-Core` (项目1)——在内部调用的。
*   **异步和后台驱动**: 它的核心功能是定时执行的后台任务，而不是实时响应请求。

因此，它的 API 主要分为两部分：一是用于**接收管理任务**的内部接口，二是在完成任务后**向外发送通知**的机制。

---

### **API 设计详解**

#### **A. 内部管理 API (Internal Management API)**

这是 `Backend-Core` 用来命令 `BreachWatch-Service` “监视谁”和“监视什么内容”的接口。

**安全与授权:**
所有请求都必须包含一个仅在后端服务之间共享的秘密 **API 密钥 (API Key)** 或 **服务间认证令牌 (Service-to-Service Token)**，通过 `Authorization` 或 `X-API-Key` HTTP Header 传递，以确保请求来自可信的内部服务。

---

##### **`POST /monitor/accounts`**
*   **描述**: 注册一个新的用户账户，让本服务开始监控与其关联的凭证（如邮箱）。这通常在用户订阅高级功能或首次开启此功能时由 `Backend-Core` 调用。
*   **请求体**:
    ```json
    {
      "user_id": "uuid-for-user-in-core-db",
      "items_to_monitor": [
        { "type": "email", "value": "user@example.com" },
        { "type": "username", "value": "user123" }
      ]
    }
    ```
*   **授权**: **需要** (服务间 API 密钥)。
*   **成功响应 (202 Accepted)**: 返回 202 表示“请求已接受，将进行异步处理”。服务会立即返回响应，然后在后台将这些信息加入监控队列。
    ```json
    {
      "status": "pending",
      "message": "Account registered for monitoring."
    }
    ```
*   **错误响应**:
    *   `400 Bad Request`: 请求体格式错误。
    *   `409 Conflict`: 该 `user_id` 已经被注册。

##### **`PUT /monitor/accounts/{user_id}`**
*   **描述**: 更新一个已存在用户的监控列表。例如，当用户更改了他的主邮箱时，`Backend-Core` 会调用此接口。
*   **请求体**:
    ```json
    {
      "items_to_monitor": [
        { "type": "email", "value": "new_user_email@example.com" },
        { "type": "username", "value": "user123" }
      ]
    }
    ```
*   **授权**: **需要** (服务间 API 密钥)。
*   **成功响应 (202 Accepted)**:
    ```json
    {
      "status": "pending_update",
      "message": "Account monitoring list submitted for update."
    }
    ```

##### **`DELETE /monitor/accounts/{user_id}`**
*   **描述**: 停止监控一个用户的所有凭证，并从本服务的数据中移除。当用户取消订阅或删除账户时调用。
*   **授权**: **需要** (服务间 API 密钥)。
*   **成功响应 (204 No Content)**: 无响应体，表示成功删除。

---

#### **B. 警报与通知 API (Alert & Notification Webhook)**

这是 `BreachWatch-Service` 在发现泄露事件后，**主动调用**其他服务（如 `Backend-Core` 或一个专门的通知服务）的接口。这是一种**Webhook**模式。

`BreachWatch-Service` 的后台任务会定期执行以下操作：
1.  从自己的数据库中获取所有需要监控的凭证。
2.  调用第三方泄露情报服务商（如 HIBP）的 API 进行查询。
3.  如果发现匹配的泄露事件，它会**向一个预先配置好的 URL 地址发送一个 POST 请求**。这个 URL 就是它暴露的“通知 API”。

##### **`POST /internal/alerts/breach-detected` (由 BreachWatch-Service 调用)**
*   **描述**: 这是 `BreachWatch-Service` 用来通知 `Backend-Core` “某个用户的数据泄露了”的 Webhook 端点。这个端点实际上是在 `Backend-Core` 项目中实现的。
*   **请求体 (由 BreachWatch-Service 发送)**:
    ```json
    {
      "user_id": "uuid-for-user-in-core-db",
      "breached_item": {
        "type": "email",
        "value": "user@example.com"
      },
      "breach_details": {
        "source": "HaveIBeenPwned",
        "breach_name": "ExampleComBreach2025",
        "breach_date": "2025-08-15",
        "compromised_data": ["Email", "Password", "Username"],
        "details_url": "https://haveibeenpwned.com/PwnedWebsites#ExampleCom"
      }
    }
    ```
*   **授权**: 这个请求也需要携带一个秘密的 API 密钥，以证明它确实是由 `BreachWatch-Service` 发出的。
*   **`Backend-Core` 的响应**: `Backend-Core` 接收到这个请求后，会负责接下来的所有逻辑，比如在用户的安全仪表盘中添加一条记录、发送邮件、推送移动端通知等。它只需向 `BreachWatch-Service` 返回一个 `200 OK` 或 `204 No Content`，表示“通知已收到”。

### **总结**

这个微服务的 API 设计非常清晰和专注：

| API 类型 | 端点示例 | 调用方 | 接收方 | 核心职责 |
| :--- | :--- | :--- | :--- | :--- |
| **内部管理 API** | `POST /monitor/accounts` | `Backend-Core` | `BreachWatch` | **接收任务**：告诉监控服务要监视谁。 |
| **通知 Webhook** | `POST /internal/alerts/breach-detected` | `BreachWatch` | `Backend-Core` | **报告结果**：告诉核心服务发现了什么问题。 |

这种解耦的设计使得 `BreachWatch-Service` 可以独立地进行开发、部署和扩展，而不会影响到主系统的其他部分，是构建健壮微服务系统的标准实践。