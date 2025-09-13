
## 构建一个基于 Python 的企业级本地文档管理系统

#### **引言与角色设定**

你现在将扮演一名资深的 Python 后端架构师。你的任务是根据以下极为详尽的需求文档，生成一个完整的、生产级别的本地文档管理系统后端项目。这个项目深受开源项目 Teedy 的启发，但我们将使用现代 Python 技术栈来实现，并融入更多最佳实践。

**核心技术栈:**
*   **Web 框架:** FastAPI
*   **数据校验与序列化:** Pydantic
*   **数据库 ORM:** SQLAlchemy (使用异步 `asyncio` 扩展)
*   **数据库迁移:** Alembic
*   **身份认证:** JWT (JSON Web Tokens) & 两步验证 (TOTP)
*   **密码处理:** Bcrypt
*   **异步任务:** （可选，但为架构做好准备）Celery 或 Arq
*   **配置管理:** Pydantic-Settings

---

### **第一部分：项目整体架构与设计哲学**

#### **1.1 项目结构 (目录布局)**

请严格按照以下结构组织代码，这体现了关注点分离的核心原则：

```
/docs-py/
├── alembic/                  # Alembic 数据库迁移目录
├── app/
│   ├── api/                  # API 层: 所有的 FastAPI 路由
│   │   ├── deps.py             # 依赖注入函数 (如获取当前用户, 数据库会话)
│   │   ├── endpoints/        # 按功能模块划分的路由文件
│   │   │   ├── auth.py
│   │   │   ├── documents.py
│   │   │   └── users.py
│   │   └── routes.py           # 组合所有 endpoint 的主路由器
│   ├── core/                   # 核心配置与启动逻辑
│   │   ├── config.py           # 应用配置 (使用 Pydantic-Settings)
│   │   └── security.py         # 密码哈希, JWT, 2FA 核心逻辑
│   ├── crud/                   # CRUD 层 (数据访问): 封装了对数据库模型的直接操作
│   │   ├── crud_user.py
│   │   └── crud_document.py
│   ├── db/                     # 数据库层
│   │   ├── base.py             # 声明式的 ORM Base 和模型基类
│   │   ├── models/             # SQLAlchemy 模型定义
│   │   │   ├── user.py
│   │   │   ├── document.py
│   │   │   └── acl.py
│   │   └── session.py          # 数据库引擎和会话创建
│   ├── schemas/                # Pydantic Schema 层: API 的数据传输对象 (DTOs)
│   │   ├── user.py
│   │   ├── document.py
│   │   └── token.py
│   ├── services/               # 服务层: 封装核心业务逻辑
│   │   ├── document_processor.py # 文档处理服务 (OCR, 转换)
│   │   └── acl_service.py      # 权限校验服务
│   ├── utils/                  # 通用工具
│   │   └── exceptions.py       # 自定义异常类
│   │   └── handlers.py         # 自定义异常处理器
│   └── main.py                 # FastAPI 应用实例和主入口
├── tests/                    # 测试目录
├── .env                      # 环境变量文件 (示例)
├── Dockerfile                # Docker 配置文件
├── docker-compose.yml        # Docker Compose 配置文件
└── requirements.txt          # Python 依赖列表
```

#### **1.2 设计哲学与亮点 (Highlights)**

*   **安全第一 (Security First):** 所有设计都必须优先考虑安全性。密码绝不以明文存储，所有敏感操作都需授权，并提供 2FA 支持。
*   **健壮性与容错 (Robustness & Fault Tolerance):** 模仿 `DocsPDType1Font` 的精神，在处理可能出错的外部依赖（如文件处理、OCR）时，必须有优雅的错误处理机制，记录错误但不应让整个应用崩溃。
*   **配置驱动 (Configuration Driven):** 模仿 `HeaderBasedSecurityFilter` 的可配置性，应用的关键行为（如数据库连接、密钥、认证方式）必须通过环境变量进行配置，而不是硬编码。
*   **异步优先 (Async First):** 充分利用 FastAPI 和 SQLAlchemy 2.0 的异步能力，以获得高并发性能。
*   **依赖注入 (Dependency Injection):** 深度使用 FastAPI 的依赖注入系统来管理数据库会话、用户认证和权限校验，这使得代码解耦且极易测试。

---

### **第二部分：核心组件的详细实现**

#### **2.1 统一异常处理 (模仿 `ClientException`, `ServerException`)**

*   **目标:** 创建一个标准化的、对前端友好的错误响应格式。
*   **实现细节:**
    1.  在 `app/utils/exceptions.py` 中，定义自定义异常类：
        *   `ClientException(status_code=400, type="ValidationError", message="...")`
        *   `ForbiddenException(status_code=403, type="ForbiddenError", message="...")`
        *   `ServerException(status_code=500, type="InternalError", message="...")`
    2.  在 `app/utils/handlers.py` 中，为 FastAPI 应用注册异常处理器。
        *   `client_exception_handler` 捕获 `ClientException` 并返回其 `status_code` 和一个包含 `type` 和 `message` 的 JSON 体。
        *   `generic_exception_handler` 捕获所有未处理的 `Exception`，将其记录到日志中（**关键点**），并返回一个通用的 500 错误 JSON 响应，避免泄露内部实现细节。
    3.  在 `app/main.py` 中，将这些处理器应用到 FastAPI 实例上。

#### **2.2 请求生命周期与数据库事务 (模仿 `RequestContextFilter`)**

*   **目标:** 为每个 API 请求提供独立的数据库会话，并自动管理事务。
*   **实现细节:**
    1.  在 `app/api/deps.py` 中，创建一个依赖函数 `get_db()`。
    2.  此函数必须使用 `yield` 语句。
    3.  `try` 块:
        *   从 `app/db/session.py` 获取一个异步数据库会话 `AsyncSession`。
        *   `yield session` 将会话提供给路径操作函数。
    4.  `finally` 块:
        *   请求处理完毕后（无论成功或失败），执行 `await session.close()` 来释放连接。
    5.  **事务管理:** 在服务层的方法中，显式地使用 `async with session.begin():` 来包裹数据库操作，这能确保在该块内所有操作要么全部成功提交，要么在发生异常时全部回滚。

#### **2.3 灵活的认证与授权系统 (模仿 `SecurityFilter` 体系)**

*   **目标:** 实现一个强大、可扩展的认证系统，支持多种认证方式，并能清晰地管理用户身份。
*   **实现细节:**
    1.  **用户身份 (`Principal`) 模型:**
        *   在 `app/schemas/user.py` 中定义 `UserPrincipal` Pydantic 模型，包含 `id`, `username`, `email`, `groups`, `permissions` 等信息。
        *   创建一个代表匿名用户的对象或`None`值。
    2.  **核心认证依赖 (`get_current_user`):**
        *   在 `app/api/deps.py` 中创建依赖函数 `get_current_user(token: str = Security(oauth2_scheme), db: AsyncSession = Depends(get_db))`。
        *   **Token-Based Auth (模仿 `TokenBasedSecurityFilter`):**
            *   函数首先解析 `token` (JWT)。如果解析失败或过期，抛出 `ForbiddenException`。
            *   从 JWT 的载荷中获取 `user_id`。
            *   使用 `crud_user.py` 从数据库中查询该用户。
            *   **细节点:** 必须检查用户是否被禁用或删除，就像 Teedy 的 `injectUser` 逻辑一样。
            *   如果用户有效，查询其所属的用户组和权限，填充并返回 `UserPrincipal` 对象。
    3.  **Header-Based Auth (模仿 `HeaderBasedSecurityFilter`):**
        *   在 `get_current_user` 函数的**开始处**添加逻辑：
        *   从 `app/core/config.py` 读取一个配置项 `HEADER_AUTH_ENABLED` 和 `HEADER_AUTH_USER_HEADER`。
        *   如果启用，则检查请求头中是否存在指定的用户名字段。
        *   如果存在，则信任该用户名，直接从数据库查询用户，跳过 Token 验证。**必须在日志中记录一条警告，表明正在使用基于头的信任认证。**
    4.  **权限校验:**
        *   创建另一个依赖函数，例如 `def has_permission(permission: str) -> Callable:`。
        *   这个依赖函数会内部依赖 `get_current_user`，然后检查返回的 `UserPrincipal` 是否包含所需的 `permission`。
        *   在需要特定权限的 API 端点上使用 `@app.get("/some-route", dependencies=[Depends(has_permission("document:read"))])`。

#### **2.4 两步验证 (2FA/TOTP) (模仿 `GoogleAuthenticator`)**

*   **目标:** 为用户账户提供额外的安全层。
*   **实现细节:**
    1.  **不重复造轮子:** 使用成熟的 Python 库 `pyotp` 和 `qrcode`。
    2.  **启用流程:**
        *   创建一个 API 端点 `POST /users/me/2fa/enable`。
        *   内部调用 `pyotp.random_base32()` 生成一个 TOTP 密钥。**将此密钥加密后**存储在用户的数据库记录中。
        *   使用 `pyotp.totp.TOTP(secret).provisioning_uri(name=user.email, issuer_name="DocsPy")` 生成一个 URI。
        *   使用 `qrcode` 库将此 URI 转换为 SVG 或 PNG 图像的 Base64 字符串，并返回给前端显示。
    3.  **验证流程:**
        *   在登录逻辑的第二步，要求用户提供 6 位数 TOTP 码。
        *   从数据库中解密并获取用户的 TOTP 密钥。
        *   使用 `pyotp.TOTP(secret).verify(code)` 进行验证。
        *   **细节点:** 模仿 `checkCode` 的“时间窗口”逻辑，`pyotp` 默认支持此功能，确保配置 `valid_window` 参数。
    4.  **备用码:** 在启用 2FA 时，生成并显示一组一次性的备用码，并将其哈希值存储在数据库中。

#### **2.5 与外部工具的健壮集成 (模仿 `InputStreamReaderThread`)**

*   **目标:** 安全、高效地调用外部命令行工具（如 Tesseract OCR）进行文档处理。
*   **实现细节:**
    1.  **使用 `asyncio.subprocess`:** 这是 Python 中处理子进程的最佳方式。
    2.  在 `app/services/document_processor.py` 中，创建一个函数 `async def run_ocr(file_path: str) -> str:`。
    3.  在此函数中，使用 `asyncio.create_subprocess_exec` 来启动 Tesseract 进程。
    4.  **关键点 (避免死锁):**
        *   将 `stdout` 和 `stderr` 都设置为 `asyncio.subprocess.PIPE`。
        *   使用 `await asyncio.gather(proc.stdout.read(), proc.stderr.read())` 来**并发地**读取标准输出和标准错误流，直到它们关闭。这完美地复现了 `InputStreamReaderThread` 的核心功能——防止缓冲区填满导致进程阻塞。
    5.  **错误处理:** 检查进程的返回码 `proc.returncode`。如果非零，从 `stderr` 读取错误信息，记录详细日志，并抛出一个自定义的 `ServerException`，内容为“OCR处理失败”。

---

### **第三部分：核心业务工作流实现**

#### **3.1 文档上传与处理流程**

1.  **API 端点:** `POST /documents/upload`，接收文件上传。
2.  **初步处理:**
    *   文件被保存到一个临时的、安全的存储位置。
    *   在数据库的 `files` 表中创建一条记录，状态为 `PROCESSING`。
3.  **异步处理 (关键):**
    *   （理想情况下）将文件 ID 和路径推送到一个异步任务队列（如 Celery）。
    *   （简化实现）或者，直接在后台 `asyncio.create_task()` 中运行处理任务，但要注意应用重启会导致任务丢失。
4.  **处理任务 (`document_processor.py`):**
    *   **类型检测:** 判断文件类型。
    *   **内容提取:**
        *   如果是图片或扫描的 PDF，调用 `run_ocr` 函数。
        *   如果是原生 PDF，使用 `PyMuPDF` (fitz) 提取文本。
        *   如果是 Office 文档，可调用 LibreOffice 的命令行接口进行转换。
    *   **元数据提取:** 提取如页数、作者等信息。
    *   **数据库更新:** 将提取的文本内容、元数据存入数据库，并将文件状态更新为 `COMPLETED`。
    *   **文件归档:** 将临时文件移动到最终的、按结构组织的存储目录（例如，按年/月/日）。
    *   **错误处理:** 如果任何步骤失败，将文件状态更新为 `FAILED`，并在错误日志中记录详细原因。

#### **3.2 权限控制 (ACL)**

1.  **数据库模型 (`acl.py`):** 创建一个 `ACL` 表，包含 `resource_id` (文档/文件夹ID), `principal_type` ('USER' 或 'GROUP'), `principal_id` (用户ID或组ID), `permission` ('READ', 'WRITE', 'DELETE')。
2.  **服务层 (`acl_service.py`):**
    *   `check_permission(user: UserPrincipal, resource_id: str, required_permission: str) -> bool`。
    *   该函数会检查用户自身以及用户所属的所有组是否在 `ACL` 表中对该资源拥有所需权限。
    *   **细节点:** 查询应是高效的，最好能一次性查询用户和其所有组的权限。
3.  **API 集成:**
    *   在所有需要访问文档的 API 端点（如 `GET /documents/{doc_id}`）中，创建一个依赖 `Depends(check_permission_dependency)`，该依赖内部调用 `acl_service.check_permission`。如果校验失败，则抛出 `ForbiddenException`。
    *   在返回文档信息的 Pydantic schema 中，动态添加一个字段 `permissions: Dict[str, bool]`，用于告诉前端当前用户对该文档具体有哪些权限（如 `{'can_edit': True, 'can_delete': False}`），这模仿了 `AclUtil.addAcls` 的设计。

---

### **第四部分：最终交付物要求**

*   **完整的 Python 代码库:** 包含上述所有模块和逻辑。
*   **`requirements.txt`:** 列出所有必要的 Python 依赖。
*   **`Dockerfile` 和 `docker-compose.yml`:** 用于轻松地将应用、数据库（PostgreSQL）和可选的 Redis（用于 Celery）容器化运行。
*   **`.env.example`:** 一个示例环境变量文件，说明所有可配置的选项。
*   **README.md:** 一份简洁的文档，说明如何设置、配置和运行此项目。

请开始生成这个项目。如果任何需求不明确，请以最符合安全和可扩展性原则的方式进行实现。