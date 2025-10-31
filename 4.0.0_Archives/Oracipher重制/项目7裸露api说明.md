好的，我们来为 `PasswordManager-Sharing-Infrastructure` (安全共享与协作后端) 这个复杂的项目设计其需要暴露的 API。

这个项目是整个密码管理器走向商业化和协作功能的**核心枢纽**。它的 API 设计必须同时兼顾**极致的安全性（零知识原则）、清晰的权限模型和多租户管理（家庭/团队）**。

这个服务的 API 将主要由 `Backend-Core` (项目1) 在内部调用，以协调和管理用户之间的访问关系。

---

### **核心安全模型：零知识共享**

在设计 API 之前，必须理解其背后的安全工作流。这套 API 的设计完全基于**非对称加密（公钥/私钥）**：

1.  **公钥分发**: 每个用户在注册时都会生成一对公钥/私钥。公钥可以安全地上传并存储在 `Backend-Core` 服务中，供其他用户查询。
2.  **共享发起**: 当用户 A（发起者）想要共享一个文件夹（其本身用一个对称的**文件夹密钥**加密）给用户 B（接收者）时：
    a.  用户 A 的客户端从 `Backend-Core` 获取用户 B 的**公钥**。
    b.  用户 A 的客户端使用用户 B 的公钥，来加密那个**文件夹密钥**。
    c.  这个**被加密后的文件夹密钥**，连同共享邀请信息，一起发送给本共享服务进行存储。
3.  **共享接受**: 当用户 B 接受邀请时：
    a.  用户 B 的客户端从本共享服务下载那个**被加密的文件夹密钥**。
    b.  用户 B 的客户端使用自己的**私钥**来解密它，从而获得明文的**文件夹密钥**。
    c.  现在，用户 B 拥有了访问该共享文件夹内所有数据的能力。

**关键点**：在这个过程中，服务器（无论是 Core 还是 Sharing）都只传递和存储了**被公钥加密过的密钥**，永远无法得知明文的文件夹密钥，从而完美地维持了零知识原则。

---

### **API 端点详解**

我们将 API 划分为三大模块：**组织 (Organizations)**、**共享集合 (Shared Collections)** 和 **邀请 (Invitations)**。

#### **A. 组织管理 (Organizations - 即家庭/团队)**

##### **`POST /organizations`**
*   **描述**: 创建一个新的组织（家庭或团队）。调用者自动成为该组织的 `owner`。
*   **调用方**: `Backend-Core` (代表某个用户)。
*   **请求体**:
    ```json
    {
      "name": "My Family",
      "owner_id": "user-uuid-of-creator"
    }
    ```
*   **成功响应 (201 Created)**:
    ```json
    {
      "org_id": "new-org-uuid",
      "name": "My Family",
      "owner_id": "user-uuid-of-creator"
    }
    ```

##### **`GET /organizations/{org_id}/members`**
*   **描述**: 获取一个组织的所有成员列表及其角色。
*   **成功响应 (200 OK)**:
    ```json
    {
      "members": [
        { "user_id": "user-uuid-1", "email": "user1@example.com", "role": "owner" },
        { "user_id": "user-uuid-2", "email": "user2@example.com", "role": "admin" },
        { "user_id": "user-uuid-3", "email": "user3@example.com", "role": "member" }
      ]
    }
    ```

##### **`PUT /organizations/{org_id}/members/{user_id}`**
*   **描述**: 更改组织内一个成员的角色（例如，将一个 `member` 提升为 `admin`）。需要 `admin` 或 `owner` 权限才能调用。
*   **请求体**:
    ```json
    {
      "role": "admin" // "admin" or "member"
    }
    ```
*   **成功响应 (200 OK)**: 返回更新后的成员信息。

##### **`DELETE /organizations/{org_id}/members/{user_id}`**
*   **描述**: 从组织中移除一个成员。需要 `admin` 或 `owner` 权限。
*   **成功响应 (204 No Content)**: 无响应体。

---

#### **B. 共享集合管理 (Shared Collections)**

“集合”是一个通用术语，可以代表一个共享文件夹或单个共享项目。

##### **`POST /collections`**
*   **描述**: 注册一个新的共享集合，并指定初始成员及其访问权限和加密密钥。
*   **请求体**:
    ```json
    {
      "org_id": "org-uuid-if-it-belongs-to-an-org", // 可选
      "creator_id": "user-uuid-of-creator",
      "encrypted_collection_blob_id": "uuid-of-data-in-core-db", // 指向核心数据库中加密数据的ID
      "initial_members": [
        {
          "user_id": "user-uuid-of-recipient",
          "permission": "editor", // "readonly", "editor"
          "encrypted_collection_key": "base64-string-key-encrypted-with-recipient-public-key"
        }
      ]
    }
    ```
*   **成功响应 (201 Created)**: 返回新创建的集合的元数据。

##### **`GET /users/{user_id}/collections`**
*   **描述**: 获取一个用户有权访问的所有共享集合的列表。
*   **成功响应 (200 OK)**:
    ```json
    [
      {
        "collection_id": "collection-uuid-1",
        "name": "Project Phoenix Encrypted Data", // 名称是加密存储在blob里的，这里可能是个占位符
        "permission": "editor",
        "encrypted_collection_key": "my-encrypted-key-for-this-collection"
      }
    ]
    ```

##### **`PUT /collections/{collection_id}/members/{user_id}`**
*   **描述**: 修改某个用户对特定共享集合的访问权限。
*   **请求体**:
    ```json
    {
      "permission": "readonly"
    }
    ```
*   **成功响应 (200 OK)**: 返回更新后的权限信息。

##### **`DELETE /collections/{collection_id}/members/{user_id}`**
*   **描述**: 撤销某个用户对特定共享集合的访问权限。
*   **成功响应 (204 No Content)**: 无响应体。

---

#### **C. 邀请管理 (Invitations)**

##### **`POST /invitations`**
*   **描述**: 发出一个邀请。可以是邀请用户加入一个组织，也可以是邀请用户访问一个共享集合。
*   **请求体**:
    ```json
    {
      "inviter_id": "inviter-user-uuid",
      "invitee_email": "new_member@example.com",
      "target_type": "organization", // or "collection"
      "target_id": "org-uuid-or-collection-uuid",
      "permission_details": { // 仅当 target_type 为 "collection" 时需要
        "permission": "readonly",
        "encrypted_collection_key": "key-encrypted-for-invitee"
      }
    }
    ```
*   **成功响应 (201 Created)**: 返回新创建的邀请信息。

##### **`GET /invitations/pending`**
*   **描述**: 获取当前登录用户所有待处理的邀请。
*   **调用方**: `Backend-Core` (代表当前用户)。
*   **成功响应 (200 OK)**:
    ```json
    [
      {
        "invitation_id": "invitation-uuid-1",
        "inviter_name": "Alice",
        "target_type": "organization",
        "target_name": "My Family"
      }
    ]
    ```

##### **`POST /invitations/{invitation_id}/respond`**
*   **描述**: 响应一个待处理的邀请（接受或拒绝）。
*   **请求体**:
    ```json
    {
      "response": "accept" // or "reject"
    }
    ```
*   **成功响应 (200 OK)**:
    ```json
    {
      "status": "accepted",
      "message": "Successfully joined 'My Family'."
    }
    ```

### **总结**

| API 模块 | 端点示例 | HTTP 方法 | 用途 |
| :--- | :--- | :--- | :--- |
| **组织管理** | `/organizations` | POST | 创建一个新的团队/家庭 |
| | `/organizations/{id}/members` | GET, PUT, DELETE | 管理团队成员及其角色 |
| **共享集合** | `/collections` | POST | 创建一个共享文件夹/项目 |
| | `/users/{id}/collections` | GET | 获取我能访问的所有共享内容 |
| | `/collections/{id}/members/{id}` | PUT, DELETE | 管理单个共享内容的成员权限 |
| **邀请管理** | `/invitations` | POST | 发送加入团队或共享的邀请 |
| | `/invitations/pending` | GET | 查看我收到的所有待处理邀请 |
| | `/invitations/{id}/respond` | POST | 接受或拒绝一个邀请 |

这个 API 结构清晰地将团队管理和具体项目的共享分离开来，同时通过传递 `encrypted_collection_key` 的核心机制，确保了整个协作过程的端到端加密和零知识特性。