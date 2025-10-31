好的，我们来为 `PasswordManager-Emergency-Access-Service` (紧急访问后端) 设计 API。

这个微服务的 API 设计**极其注重安全流程和状态管理**。它本质上是一个**数字遗产和紧急委托的执行引擎**。其交互模式是异步的、状态驱动的，并且必须在流程的每个关键步骤都进行严格的验证和通知。

与共享服务类似，它也必须遵守**零知识原则**。服务器本身永远无法直接访问用户的密码库，它只是一个**安全的时间锁和授权中介**。

---

### **核心安全模型：有时间延迟的密钥交换**

1.  **设置阶段 (用户主动操作)**:
    a.  用户 A（委托人）选择用户 B（紧急联系人）。
    b.  用户 A 的客户端获取用户 B 的**公钥**。
    c.  用户 A 的客户端使用用户 B 的公钥，来加密用户 A 自己的**主密码**或一个专门的**恢复密钥**。
    d.  这个**被加密的密钥**被发送到本服务存储，进入“待激活”状态。
2.  **激活阶段 (紧急联系人操作)**:
    a.  用户 B 发起紧急访问请求。
    b.  本服务启动一个**预设的等待期**（例如 7 天）。
    c.  服务**立即通知**用户 A（委托人）：“有人正试图访问你的账户！如果你能看到此消息，请立即取消此请求。”
3.  **授权阶段**:
    a.  如果在等待期内，用户 A **没有取消**该请求。
    b.  等待期结束后，本服务将状态标记为“已授权”。
    c.  现在，用户 B 可以从本服务下载那个**被加密的密钥**。
    d.  用户 B 用自己的**私钥**解密它，从而获得访问用户 A 账户的权限。

这个流程确保了只有在委托人长时间无响应的情况下，访问权限才会被授予，给予了委托人充分的否决机会。

---

### **API 端点详解**

我们将 API 划分为三个主要部分：**联系人管理 (Contacts)**、**访问请求 (Access Requests)** 和 **响应 (Responses)**。

#### **A. 紧急联系人管理 (由委托人操作)**

##### **`POST /contacts`**
*   **描述**: 委托人发起一个邀请，指定某人为自己的紧急联系人。
*   **调用方**: `Backend-Core` (代表委托人)。
*   **请求体**:
    ```json
    {
      "grantor_user_id": "user-A-uuid", // 委托人ID
      "grantee_email": "user-B-email@example.com", // 被邀请的紧急联系人邮箱
      "wait_period_days": 7, // 委托人设定的等待期
      "encrypted_recovery_key": "base64-string-key-encrypted-with-grantee-public-key"
    }
    ```
*   **成功响应 (201 Created)**: 返回邀请记录，状态为 `pending_acceptance`。

##### **`GET /contacts/granted-by-me`**
*   **描述**: 委托人查看自己设置的所有紧急联系人及其状态。
*   **成功响应 (200 OK)**:
    ```json
    [
      {
        "contact_id": "contact-uuid-1",
        "grantee_email": "user-B-email@example.com",
        "status": "active", // "pending_acceptance", "active", "revoked"
        "wait_period_days": 7,
        "created_at": "..."
      }
    ]
    ```

##### **`DELETE /contacts/{contact_id}`**
*   **描述**: 委托人撤销一个紧急联系人的授权。可以随时进行。
*   **成功响应 (204 No Content)**: 无响应体。

---

#### **B. 邀请响应 (由被邀请人操作)**

##### **`GET /contacts/granted-to-me`**
*   **描述**: 被邀请人查看自己收到的所有紧急联系人邀请。
*   **成功响应 (200 OK)**:
    ```json
    [
      {
        "invitation_id": "contact-uuid-1",
        "grantor_email": "user-A-email@example.com",
        "status": "pending_acceptance",
        "wait_period_days": 7
      }
    ]
    ```

##### **`POST /contacts/{contact_id}/respond`**
*   **描述**: 被邀请人接受或拒绝成为紧急联系人。
*   **请求体**:
    ```json
    {
      "response": "accept" // or "reject"
    }
    ```*   **成功响应 (200 OK)**:
    ```json
    {
      "status": "active",
      "message": "You are now an emergency contact for user A."
    }
    ```

---

#### **C. 紧急访问请求流程 (由紧急联系人操作)**

##### **`POST /access-requests`**
*   **描述**: 紧急联系人（用户 B）正式发起对委托人（用户 A）账户的紧急访问请求。这是整个流程的起点。
*   **请求体**:
    ```json
    {
      "grantor_user_id": "user-A-uuid" // 我要请求访问谁的账户
    }
    ```
*   **成功响应 (202 Accepted)**: 表示请求已收到，等待期已开始。服务会立即向委托人发送警报邮件/推送。
    ```json
    {
      "request_id": "request-uuid-123",
      "status": "waiting_period",
      "grantor_user_id": "user-A-uuid",
      "wait_period_ends_at": "timestamp-7-days-from-now"
    }
    ```
*   **错误响应**:
    *   `403 Forbidden`: 如果你不是该用户的已激活紧急联系人。
    *   `409 Conflict`: 如果已存在一个正在进行的请求。

##### **`GET /access-requests/{request_id}`**
*   **描述**: 紧急联系人查询一个访问请求的当前状态。
*   **成功响应 (200 OK)**:
    ```json
    {
      "request_id": "request-uuid-123",
      "status": "waiting_period", // "waiting_period", "denied_by_grantor", "approved", "expired"
      "wait_period_ends_at": "..."
    }
    ```

##### **`POST /access-requests/{request_id}/claim`**
*   **描述**: 在等待期结束后，紧急联系人调用此接口来**领取**访问权限。
*   **成功响应 (200 OK)**: **这是最关键的一步**，服务返回之前存储的、被加密的恢复密钥。
    ```json
    {
      "status": "claimed",
      "encrypted_recovery_key": "the-same-base64-string-from-the-setup-phase"
    }
    ```
*   **错误响应**:
    *   `403 Forbidden`: 如果等待期尚未结束，或请求已被委托人拒绝。

---

#### **D. 否决流程 (由委托人操作)**

##### **`GET /access-requests/initiated-on-me`**
*   **描述**: 委托人查看当前是否有人正在尝试访问自己的账户。
*   **成功响应 (200 OK)**:
    ```json
    {
      "active_request": {
        "request_id": "request-uuid-123",
        "requester_email": "user-B-email@example.com",
        "requested_at": "...",
        "wait_period_ends_at": "..."
      }
    }
    ```

##### **`POST /access-requests/{request_id}/deny`**
*   **描述**: 委托人**否决**一个正在进行的紧急访问请求。
*   **成功响应 (200 OK)**:
    ```json
    {
      "status": "denied_by_grantor",
      "message": "The access request has been successfully denied."
    }
    ```

### **总结**

| API 模块 | 端点示例 | HTTP 方法 | 用途 | 调用者角色 |
| :--- | :--- | :--- | :--- | :--- |
| **联系人管理** | `/contacts` | POST | 设置一个紧急联系人 | 委托人 |
| | `/contacts/{id}` | DELETE | 撤销一个紧急联系人 | 委托人 |
| **邀请响应** | `/contacts/{id}/respond`| POST | 接受/拒绝成为联系人 | 被邀请人 |
| **访问请求** | `/access-requests` | POST | 发起紧急访问 | 紧急联系人 |
| | `/access-requests/{id}/claim` | POST | 在等待期后领取权限 | 紧急联系人 |
| **否决流程** | `/access-requests/{id}/deny` | POST | 否决对我的账户的访问 | 委托人 |

这个 API 集合完整地定义了一个安全、可控、透明的紧急访问流程，确保了在最坏的情况下，用户的数据可以被信任的人安全地接管。