# Areas_Of_Knowledge\DevOps_And_Infrastructure\Docker\Docker_Complete_Guide.md

## Docker 语法全解析：从入门到精通

Docker 已经成为现代软件开发和运维不可或缺的工具。它通过容器化技术，将应用及其依赖打包，实现了“一次构建，处处运行”的理念。为了帮助您更深刻地理解和高效地使用 Docker，本文将详细梳理 Docker 的核心语法，涵盖 Dockerfile、Docker CLI 和 Docker Compose，并提供一个支持跳转的完整目录，方便您快速查阅。

### **目录**

- [Areas\_Of\_Knowledge\\DevOps\_And\_Infrastructure\\Docker\\Docker\_Complete\_Guide.md](#areas_of_knowledgedevops_and_infrastructuredockerdocker_complete_guidemd)
  - [Docker 语法全解析：从入门到精通](#docker-语法全解析从入门到精通)
    - [**目录**](#目录)
    - [1. Dockerfile：构建镜像的蓝图](#1-dockerfile构建镜像的蓝图)
      - [1.1 核心指令详解](#11-核心指令详解)
        - [**FROM：指定基础镜像**](#from指定基础镜像)
        - [**RUN：执行命令**](#run执行命令)
        - [**CMD 与 ENTRYPOINT：定义容器启动命令**](#cmd-与-entrypoint定义容器启动命令)
        - [**COPY 与 ADD：复制文件**](#copy-与-add复制文件)
        - [**WORKDIR：设置工作目录**](#workdir设置工作目录)
        - [**ENV：设置环境变量**](#env设置环境变量)
        - [**EXPOSE：声明端口**](#expose声明端口)
        - [**VOLUME：定义匿名卷**](#volume定义匿名卷)
        - [**USER：指定运行用户**](#user指定运行用户)
        - [**ARG：定义构建参数**](#arg定义构建参数)
        - [**LABEL：添加元数据**](#label添加元数据)
      - [1.2 Dockerfile 最佳实践](#12-dockerfile-最佳实践)
    - [2. Docker CLI：与 Docker 引擎交互的利器](#2-docker-cli与-docker-引擎交互的利器)
      - [2.1 镜像管理命令](#21-镜像管理命令)
      - [2.2 容器生命周期管理命令](#22-容器生命周期管理命令)
      - [2.3 容器操作命令](#23-容器操作命令)
      - [2.4 系统与其他命令](#24-系统与其他命令)
    - [3. Docker Compose：编排多容器应用的利器](#3-docker-compose编排多容器应用的利器)
      - [3.1 `docker-compose.yml` 核心语法](#31-docker-composeyml-核心语法)
        - [**version：指定 Compose 文件版本**](#version指定-compose-文件版本)
        - [**services：定义服务**](#services定义服务)
        - [**networks：定义网络**](#networks定义网络)
        - [**volumes：定义卷**](#volumes定义卷)
      - [3.2 常用 Docker Compose 命令](#32-常用-docker-compose-命令)

---

### <a name="dockerfile"></a>1. Dockerfile：构建镜像的蓝图

Dockerfile 是一个用来构建镜像的文本文件，其中包含了一系列用户可以调用的命令，用于自动化地创建 Docker 镜像。

#### <a name="dockerfile-core-instructions"></a>1.1 核心指令详解

##### <a name="from"></a>**FROM：指定基础镜像**

`FROM` 指令是 Dockerfile 的第一条指令，用于指定新镜像所基于的基础镜像。

*   **语法：** `FROM <image>[:<tag>] [AS <name>]`
*   **示例：** `FROM ubuntu:22.04`

##### <a name="run"></a>**RUN：执行命令**

`RUN` 指令用于在镜像构建过程中执行命令，并创建一个新的镜像层。

*   **语法：**
    *   `RUN <command>` (shell 格式)
    *   `RUN ["executable", "param1", "param2"]` (exec 格式)
*   **示例：** `RUN apt-get update && apt-get install -y nginx`

##### <a name="cmd-entrypoint"></a>**CMD 与 ENTRYPOINT：定义容器启动命令**

`CMD` 和 `ENTRYPOINT` 都用于指定容器启动时执行的命令，但它们之间存在一些差异。

*   **CMD:**
    *   **语法：**
        *   `CMD ["executable","param1","param2"]` (exec 格式, 推荐)
        *   `CMD command param1 param2` (shell 格式)
        *   `CMD ["param1","param2"]` (作为 ENTRYPOINT 的默认参数)
    *   **作用：** 为启动的容器提供默认的执行命令。如果 `docker run` 时指定了命令，则会覆盖 `CMD` 的命令。
*   **ENTRYPOINT:**
    *   **语法：**
        *   `ENTRYPOINT ["executable", "param1", "param2"]` (exec 格式, 推荐)
        *   `ENTRYPOINT command param1 param2` (shell 格式)
    *   **作用：** 配置容器启动后执行的命令，并且不可被 `docker run` 提供的参数覆盖。 `docker run` 传入的参数会作为 `ENTRYPOINT` 指令的参数。

*   **二者交互：** `CMD` 和 `ENTRYPOINT` 指令可以结合使用，`CMD` 的内容会作为 `ENTRYPOINT` 的默认参数。

##### <a name="copy-add"></a>**COPY 与 ADD：复制文件**

`COPY` 和 `ADD` 指令都用于将文件或目录从构建上下文复制到镜像中。

*   **COPY:**
    *   **语法：** `COPY [--chown=<user>:<group>] <src>... <dest>`
    *   **作用：** 复制本地文件或目录到容器的指定路径。
*   **ADD:**
    *   **语法：** `ADD [--chown=<user>:<group>] <src>... <dest>`
    *   **作用：** 功能与 `COPY` 类似，但 `ADD` 还支持解压本地的压缩文件和从 URL 下载文件。

##### <a name="workdir"></a>**WORKDIR：设置工作目录**

`WORKDIR` 指令用于为 Dockerfile 中在其之后执行的 `RUN`, `CMD`, `ENTRYPOINT`, `COPY` 和 `ADD` 指令设置工作目录。

*   **语法：** `WORKDIR /path/to/workdir`
*   **示例：** `WORKDIR /app`

##### <a name="env"></a>**ENV：设置环境变量**

`ENV` 指令用于设置环境变量，这些变量在构建过程和容器运行时都可用。

*   **语法：**
    *   `ENV <key>=<value> ...`
    *   `ENV <key> <value>`
*   **示例：** `ENV APP_VERSION=1.0`

##### <a name="expose"></a>**EXPOSE：声明端口**

`EXPOSE` 指令用于声明容器在运行时监听的端口。 这仅仅是一个声明，并不会实际发布端口。

*   **语法：** `EXPOSE <port> [<port>/<protocol>...]`
*   **示例：** `EXPOSE 80/tcp`

##### <a name="volume"></a>**VOLUME：定义匿名卷**

`VOLUME` 指令用于创建一个可以绕过联合文件系统的挂载点，用于持久化数据。

*   **语法：** `VOLUME ["/path/to/volume"]`
*   **示例：** `VOLUME ["/data"]`

##### <a name="user"></a>**USER：指定运行用户**

`USER` 指令用于设置运行容器时的用户名或 UID 以及可选的用户组或 GID。

*   **语法：** `USER <user>[:<group>]`
*   **示例：** `USER nginx`

##### <a name="arg"></a>**ARG：定义构建参数**

`ARG` 指令定义了一个变量，用户可以在构建时使用 `--build-arg <varname>=<value>` 标志将其传递给构建器。

*   **语法：** `ARG <name>[=<default value>]`
*   **示例：** `ARG APP_VERSION=1.0`

##### <a name="label"></a>**LABEL：添加元数据**

`LABEL` 指令用于向镜像添加元数据。

*   **语法：** `LABEL <key>=<value> <key>=<value> ...`
*   **示例：** `LABEL maintainer="example@example.com"`

#### <a name="dockerfile-best-practices"></a>1.2 Dockerfile 最佳实践

*   **保持容器最小化：** 避免安装不必要的包，以减少镜像大小和构建时间。
*   **利用构建缓存：** 将不经常变动的内容放在 Dockerfile 的前面，以充分利用 Docker 的构建缓存机制。
*   **使用多阶段构建：** 对于需要编译的语言，使用多阶段构建可以将编译环境和运行环境分离，从而大大减小最终镜像的体积。
*   **减少镜像层数：** 将多个 `RUN` 指令合并为一条，以减少镜像的层数。
*   **使用 `.dockerignore` 文件：** 排除不需要包含在构建上下文中的文件和目录，以加快构建速度。
*   **为指令添加注释：** 提高 Dockerfile 的可读性和可维护性。
*   **单一职责原则：** 每个容器应该只运行一个进程。
*   **尽可能使用官方镜像：** 官方镜像经过了安全扫描和优化，更加可靠。

### <a name="docker-cli"></a>2. Docker CLI：与 Docker 引擎交互的利器

Docker CLI（Command-Line Interface）是与 Docker 守护进程交互的主要工具。 它提供了一系列的命令来管理镜像、容器、网络和卷。

#### <a name="image-management-commands"></a>2.1 镜像管理命令

| 命令 | 描述 |
| --- | --- |
| `docker build` | 从 Dockerfile 构建一个镜像。 |
| `docker images` | 列出本地镜像。 |
| `docker pull` | 从镜像仓库拉取一个镜像。 |
| `docker push` | 将一个镜像推送到镜像仓库。 |
| `docker rmi` | 删除一个或多个本地镜像。 |
| `docker tag` | 为镜像创建一个新的标签。 |
| `docker history` | 查看镜像的构建历史。 |
| `docker inspect` | 显示一个或多个镜像的详细信息。 |

#### <a name="container-lifecycle-management-commands"></a>2.2 容器生命周期管理命令

| 命令 | 描述 |
| --- | --- |
| `docker run` | 创建并启动一个新的容器。 |
| `docker start` | 启动一个或多个已经停止的容器。 |
| `docker stop` | 停止一个或多个正在运行的容器。 |
| `docker restart` | 重启一个或多个容器。 |
| `docker kill` | 强制停止一个或多个正在运行的容器。 |
| `docker rm` | 删除一个或多个已经停止的容器。 |
| `docker pause` | 暂停一个或多个容器中的所有进程。 |
| `docker unpause` | 恢复一个或多个容器中所有被暂停的进程。 |
| `docker create` | 创建一个新的容器，但不启动它。 |

#### <a name="container-operation-commands"></a>2.3 容器操作命令

| 命令 | 描述 |
| --- | --- |
| `docker ps` | 列出容器。 |
| `docker logs` | 获取容器的日志。 |
| `docker exec` | 在正在运行的容器中执行一个命令。 |
| `docker attach` | 附加到正在运行的容器的标准输入、输出和错误流。 |
| `docker cp` | 在容器和本地文件系统之间复制文件/文件夹。 |
| `docker stats` | 实时显示容器的资源使用统计。 |
| `docker top` | 显示一个容器内运行的进程。 |

#### <a name="system-and-other-commands"></a>2.4 系统与其他命令

| 命令 | 描述 |
| --- | --- |
| `docker login` | 登录到 Docker 镜像仓库。 |
| `docker logout` | 从 Docker 镜像仓库登出。 |
| `docker version` | 显示 Docker 的版本信息。 |
| `docker info` | 显示 Docker 的系统范围信息。 |
| `docker system prune` | 删除所有未使用的容器、网络、镜像（悬空和未使用）以及可选的卷。 |

### <a name="docker-compose"></a>3. Docker Compose：编排多容器应用的利器

Docker Compose 是一个用于定义和运行多容器 Docker 应用程序的工具。 通过一个 YAML 文件来配置应用程序的服务，然后使用一个命令，就可以创建并启动所有服务。

#### <a name="docker-compose-yml-syntax"></a>3.1 `docker-compose.yml` 核心语法

`docker-compose.yml` 是 Compose 使用的默认配置文件，它采用 YAML 格式。

##### <a name="version"></a>**version：指定 Compose 文件版本**

文件的顶层键，用于指定 Compose 文件的版本。

*   **示例：** `version: "3.8"`

##### <a name="services"></a>**services：定义服务**

`services` 部分定义了应用程序的各个服务（容器）。

*   **示例：**
    ```yaml
    services:
      webapp:
        build: .
        ports:
          - "8000:8000"
        volumes:
          - .:/code
        depends_on:
          - redis
      redis:
        image: "redis:alpine"
    ```

*   **常用服务配置项：**
    *   `image`: 指定服务使用的镜像。
    *   `build`: 指定用于构建服务镜像的 Dockerfile 路径。
    *   `ports`: 映射端口。
    *   `volumes`: 挂载卷。
    *   `environment`: 设置环境变量。
    *   `depends_on`: 定义服务之间的依赖关系。
    *   `networks`: 将服务连接到网络。

##### <a name="networks"></a>**networks：定义网络**

`networks` 部分用于定义应用程序的网络。

*   **示例：**
    ```yaml
    networks:
      app-network:
        driver: bridge
    ```

##### <a name="volumes"></a>**volumes：定义卷**

`volumes` 部分用于定义应用程序的数据卷。

*   **示例：**
    ```yaml
    volumes:
      db-data:
        driver: local
    ```

#### <a name="docker-compose-commands"></a>3.2 常用 Docker Compose 命令

| 命令 | 描述 |
| --- | --- |
| `docker-compose up` | 创建并启动 Compose 文件中定义的所有服务。 |
| `docker-compose down` | 停止并删除 Compose 文件中定义的所有服务、网络和卷。 |
| `docker-compose start` | 启动已经存在的服务。 |
| `docker-compose stop` | 停止正在运行的服务。 |
| `docker-compose restart` | 重启服务。 |
| `docker-compose build` | 构建或重新构建服务。 |
| `docker-compose ps` | 列出 Compose 项目中的容器。 |
| `docker-compose logs` | 查看服务的日志输出。 |
| `docker-compose exec` | 在正在运行的服务中执行一个命令。 |
| `docker-compose pull` | 拉取服务依赖的镜像。 |
| `docker-compose push` | 推送服务镜像到镜像仓库。 |