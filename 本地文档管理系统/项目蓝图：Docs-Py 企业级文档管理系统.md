### 项目蓝图：Docs-Py 企业级文档管理系统

#### **引言与核心定位**

`Docs-Py` 是一个受 Teedy 启发，采用现代 Python 技术栈（FastAPI, SQLAlchemy Async, Pydantic）构建的企业级本地文档管理系统。我们的核心目标是创建一个**安全、高性能、可配置且易于维护**的后端服务，为企业和个人提供一个私有的、可信赖的数字文档保险箱。

---

### **第一部分：项目整体架构与设计哲学**

#### **1.1. 项目结构 (最终确认)**

我们严格遵循您规划的、关注点分离的目录结构。这确保了代码库的清晰性、可测试性和长期可维护性。

```
/docs-py/
├── alembic/                  # 数据库迁移 (Alembic)
├── app/
│   ├── api/                  # API 层 (HTTP 接口)
│   │   ├── deps.py             # 依赖注入 (获取用户、DB会话等)
│   │   ├── endpoints/        # 功能模块化的路由 (auth.py, documents.py)
│   │   └── routes.py           # API 路由聚合器
│   ├── core/                   # 核心配置与安全
│   │   ├── config.py           # 应用配置 (Pydantic-Settings)
│   │   └── security.py         # 密码、JWT、2FA 核心安全逻辑
│   ├── crud/                   # 数据访问层 (封装原始数据库操作)
│   │   ├── crud_user.py, crud_document.py
│   ├── db/                     # 数据库层
│   │   ├── base.py             # ORM 模型基类
│   │   ├── models/             # SQLAlchemy 模型定义 (user.py, document.py)
│   │   └── session.py          # 数据库引擎与会话管理
│   ├── schemas/                # 数据传输对象 (Pydantic Schemas)
│   │   ├── user.py, document.py, token.py
│   ├── services/               # 业务逻辑层 (封装复杂业务流程)
│   │   ├── document_processor.py # 文档 OCR、转换服务
│   │   └── acl_service.py      # 权限校验服务
│   ├── utils/                  # 通用工具与异常处理
│   │   ├── exceptions.py       # 自定义异常类
│   │   └── handlers.py         # 全局异常处理器
│   └── main.py                 # FastAPI 应用主入口
├── tests/                    # 单元与集成测试
├── .env.example              # 环境变量模板
├── Dockerfile                # 生产级多阶段 Dockerfile
├── docker-compose.yml        # 开发与部署编排
└── requirements.txt          # 项目依赖
```

#### **1.2. 设计哲学与亮点**

*   **安全第一 (Security First)**：所有设计决策都以安全为最高优先级。采用 Bcrypt 哈希密码、强制执行授权、提供 TOTP 两步验证，并通过 JWT 保护 API 端点。
*   **异步优先 (Async First)**：全面拥抱 `asyncio`，利用 FastAPI 和 SQLAlchemy 2.0+ 的原生异步能力，确保在高并发场景下的卓越性能，轻松应对大量 I/O 密集型操作（如文件读写、数据库查询）。
*   **配置驱动 (Configuration Driven)**：借鉴 `HeaderBasedSecurityFilter` 的灵活性，系统所有关键参数（数据库 URL、JWT 密钥、认证头）均通过环境变量配置 (`Pydantic-Settings`)，实现零代码修改部署。
*   **依赖注入 (Dependency Injection)**：深度利用 FastAPI 的 DI 系统。数据库会话、用户身份验证和权限检查都作为可依赖项注入，这极大地解耦了代码，并使得单元测试变得异常简单和高效。
*   **健壮性与容错 (Robustness & Fault Tolerance)**：模仿 `DocsPDType1Font` 在处理复杂文件时的精神，任何与外部系统（如 Tesseract OCR、文件系统）的交互都包裹在严密的错误处理中，确保单个任务的失败不会导致整个服务崩溃，并留下详细的日志。

---

### **第二部分：核心组件的实现细节**

#### **2.1. 统一异常处理 (模仿 `ClientException`, `ServerException`)**

*   **目标**：为所有 API 响应提供一致、可预测的错误结构，方便前端处理。
*   **实现**：
    1.  **`app/utils/exceptions.py`**：定义具体的业务异常类。
        ```python
        class ClientException(HTTPException):
            def __init__(self, status_code: int, detail: Any = None):
                super().__init__(status_code=status_code, detail=detail)

        class NotFoundException(ClientException):
            def __init__(self, resource: str):
                super().__init__(status_code=404, detail=f"{resource} not found")

        class ForbiddenException(ClientException):
            def __init__(self, detail: str = "Permission denied"):
                super().__init__(status_code=403, detail=detail)
        ```
    2.  **`app/utils/handlers.py`**：定义全局异常处理器。
        ```python
        from fastapi import Request
        from fastapi.responses import JSONResponse
        # ... logger setup ...

        async def generic_exception_handler(request: Request, exc: Exception):
            logger.error(f"Unhandled exception for {request.url}: {exc}", exc_info=True)
            return JSONResponse(
                status_code=500,
                content={"type": "InternalServerError", "message": "An unexpected error occurred."},
            )
        ```
    3.  **`app/main.py`**：在应用启动时注册处理器。
        `app.add_exception_handler(Exception, generic_exception_handler)`

#### **2.2. 请求生命周期与数据库事务 (模仿 `RequestContextFilter`)**

*   **目标**：确保每个请求都使用独立的数据库会话，并实现自动化的事务管理与连接释放。
*   **实现 (`app/api/deps.py`)**：
    ```python
    from app.db.session import async_session

    async def get_db() -> AsyncGenerator[AsyncSession, None]:
        async with async_session() as session:
            try:
                yield session
                await session.commit()
            except Exception:
                await session.rollback()
                raise
    ```    *   **事务管理**：上面的 `get_db` 实现了“每个请求一个事务”的模式。对于更精细的控制，在服务层方法中依然可以嵌套使用 `async with session.begin():` 来创建保存点或子事务。

#### **2.3. 灵活的认证与授权系统 (模仿 `SecurityFilter` 体系)**

*   **目标**：构建一个可插拔、多模式的认证系统，并与精细的权限校验无缝集成。
*   **实现 (`app/api/deps.py`)**：
    ```python
    from app.core.config import settings
    from app.core.security import decode_jwt
    from app.crud import crud_user
    # ... other imports ...

    oauth2_scheme = OAuth2PasswordBearer(tokenUrl="/api/auth/login")

    async def get_current_user(
        token: str = Security(oauth2_scheme),
        db: AsyncSession = Depends(get_db),
        request: Request
    ) -> User:
        # 1. 优先处理 Header-Based Auth (模仿 HeaderBasedSecurityFilter)
        if settings.HEADER_AUTH_ENABLED:
            header_user = request.headers.get(settings.HEADER_AUTH_USER_HEADER)
            if header_user:
                logger.warning(f"Authenticating via trusted header: {settings.HEADER_AUTH_USER_HEADER}")
                user = await crud_user.get_by_username(db, username=header_user)
                if user and user.is_active:
                    return user

        # 2. 标准 Token-Based Auth (JWT)
        try:
            payload = decode_jwt(token)
            user_id = payload.get("sub")
            if user_id is None:
                raise ForbiddenException("Invalid token")
        except JWTError:
            raise ForbiddenException("Could not validate credentials")

        user = await crud_user.get(db, id=user_id)
        if not user or not user.is_active:
            raise ForbiddenException("User not found or inactive")

        return user
    ```
    *   **权限校验**：我们将创建一个依赖工厂来实现。
        ```python
        def require_permission(permission: str):
            async def permission_checker(
                current_user: User = Depends(get_current_user),
                acl_service: AclService = Depends() # 依赖注入服务
            ):
                if not await acl_service.has_global_permission(current_user, permission):
                    raise ForbiddenException()
            return permission_checker

        # 在路由中使用:
        # @router.post("/", dependencies=[Depends(require_permission("user:create"))])
        ```

#### **2.4. 两步验证 (2FA/TOTP) (模仿 `GoogleAuthenticator`)**

*   **目标**：基于 TOTP 为账户提供额外的安全保障。
*   **实现 (`app/core/security.py` 和 `app/api/endpoints/users.py`)**：
    1.  **库**：`pyotp` 和 `qrcode`。
    2.  **启用流程 (`POST /users/me/2fa/setup`)**：
        *   生成 `pyotp.random_base32()` 密钥。
        *   **安全存储**：使用一个独立的密钥（从配置中读取）通过 Fernet 对 TOTP 密钥进行**加密**，然后存入用户数据库。
        *   生成 `provisioning_uri` 并用 `qrcode` 库将其转换为 Base64 编码的 PNG 图片返回给前端。
    3.  **验证流程**：
        *   在登录的第二阶段，用户提交 TOTP 码。
        *   从数据库取出加密的密钥，解密。
        *   `totp = pyotp.TOTP(decrypted_secret); totp.verify(code, valid_window=1)`。`valid_window` 参数完美复现了 Teedy `checkCode` 的时间容错逻辑。
    4.  **备用码**：启用时，生成 10 个一次性备用码，**将其 Bcrypt 哈希值**存入数据库，供用户紧急情况下使用。

#### **2.5. 与外部工具的健壮集成 (模仿 `InputStreamReaderThread`)**

*   **目标**：安全、无阻塞地调用外部命令行工具（如 Tesseract OCR）。
*   **实现 (`app/services/document_processor.py`)**：
    ```python
    import asyncio

    async def run_ocr(file_path: str) -> str:
        command = ["tesseract", str(file_path), "stdout", "-l", "eng"]
        proc = await asyncio.create_subprocess_exec(
            *command,
            stdout=asyncio.subprocess.PIPE,
            stderr=asyncio.subprocess.PIPE
        )

        # 并发读取 stdout 和 stderr，避免任何一方缓冲区填满导致死锁
        stdout, stderr = await proc.communicate()

        if proc.returncode != 0:
            error_message = stderr.decode()
            logger.error(f"OCR process failed for {file_path}: {error_message}")
            raise ServerException("OCR processing failed")

        return stdout.decode()
    ```

---

### **第三部分：核心业务工作流**

#### **3.1. 文档上传与异步处理**

1.  **API (`POST /documents/upload`)**：接收 `UploadFile`，将其流式写入临时安全目录。在数据库中创建一条状态为 `PROCESSING` 的文档记录。
2.  **异步触发**：立即返回 `202 Accepted` 响应给客户端，并使用 `background_tasks.add_task(process_document, doc_id)` 来触发后台处理。对于生产环境，这里将替换为向 Celery 或 Arq 推送任务。
3.  **处理任务 (`process_document`)**：
    *   调用 `document_processor` 服务。
    *   服务内部进行文件类型检测、内容提取（OCR 或文本解析）、元数据生成。
    *   将结果更新回数据库，状态改为 `COMPLETED`。
    *   将文件从临时目录移动到按 `年/月/日` 组织的归档目录。
    *   若失败，更新状态为 `FAILED` 并记录详尽错误。

#### **3.2. 权限控制 (ACL)**

1.  **模型 (`acl.py`)**：`ACL` 表包含 `document_id`, `principal_type` (Enum: USER, GROUP), `principal_id`, `permission` (Enum: READ, WRITE, DELETE)。
2.  **服务 (`acl_service.py`)**：
    *   `check_permission(user, document, permission)`：该服务会一次性查询用户自身以及其所属所有组的权限，进行高效校验。
    *   `get_permissions_for_document(user, document)`：返回一个字典，如 `{'can_read': True, 'can_write': True, 'can_delete': False}`，用于在 API 响应中附加给前端，这完美复现了 `AclUtil.addAcls` 的功能。
3.  **集成**：在文档相关的 API 端点中，通过依赖注入调用 `acl_service` 来保护资源。

---

### **第四部分：最终交付物清单**

*   **完整的 Python 代码库**：遵循上述结构和实现细节的完整项目代码。
*   **`requirements.txt`**：包含 `fastapi`, `uvicorn`, `sqlalchemy[asyncio]`, `alembic`, `pydantic`, `pydantic-settings`, `python-jose[cryptography]`, `passlib[bcrypt]`, `pyotp`, `qrcode`, `python-multipart` 等所有依赖。
*   **`Dockerfile`**：一个优化的、多阶段构建的 Dockerfile，最终镜像只包含运行时依赖，体积小且安全。
*   **`docker-compose.yml`**：用于一键启动整个开发环境，包含 `docs-py` 服务、PostgreSQL 数据库，以及可选的 Redis 和 Celery worker。
*   **`.env.example`**：一个详尽的环境变量模板，解释了每个配置项的作用（`DATABASE_URL`, `SECRET_KEY`, `JWT_ALGORITHM`, `HEADER_AUTH_ENABLED` 等）。
*   **`README.md`**：一份清晰的说明文档，指导开发者如何设置环境、运行迁移、启动服务和执行测试。