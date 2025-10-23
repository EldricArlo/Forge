# Areas_Of_Knowledge\DevOps_And_Infrastructure\Docker\Docker_Cheatsheet.md

## Docker 速查表

### 目录

- [Areas\_Of\_Knowledge\\DevOps\_And\_Infrastructure\\Docker\\Docker\_Cheatsheet.md](#areas_of_knowledgedevops_and_infrastructuredockerdocker_cheatsheetmd)
  - [Docker 速查表](#docker-速查表)
    - [目录](#目录)
    - [一、核心概念](#一核心概念)
    - [二、镜像管理 (Image)](#二镜像管理-image)
      - [2.1 搜索与拉取镜像](#21-搜索与拉取镜像)
      - [2.2 查看与删除镜像](#22-查看与删除镜像)
      - [2.3 构建镜像](#23-构建镜像)
      - [2.4 镜像标签与推送](#24-镜像标签与推送)
      - [2.5 镜像导入与导出](#25-镜像导入与导出)
    - [三、容器管理 (Container)](#三容器管理-container)
      - [3.1 运行容器](#31-运行容器)
      - [3.2 查看容器](#32-查看容器)
      - [3.3 启动、停止与重启容器](#33-启动停止与重启容器)
      - [3.4 进入容器](#34-进入容器)
      - [3.5 删除容器](#35-删除容器)
      - [3.6 查看容器日志与信息](#36-查看容器日志与信息)
    - [四、Dockerfile 指令详解](#四dockerfile-指令详解)
      - [4.1 核心指令](#41-核心指令)
      - [4.2 其他常用指令](#42-其他常用指令)
    - [五、Docker Compose 多容器编排](#五docker-compose-多容器编排)
      - [5.1 核心命令](#51-核心命令)
      - [5.2 `docker-compose.yml` 常用配置](#52-docker-composeyml-常用配置)
    - [六、网络管理 (Network)](#六网络管理-network)
      - [6.1 网络命令](#61-网络命令)
      - [6.2 网络驱动](#62-网络驱动)
    - [七、数据卷管理 (Volume)](#七数据卷管理-volume)
      - [7.1 数据卷命令](#71-数据卷命令)
      - [7.2 挂载数据卷](#72-挂载数据卷)
    - [八、系统与其他命令](#八系统与其他命令)

---

### 一、核心概念

*   **镜像 (Image):** 一个只读的模板，包含了创建 Docker 容器的说明。它是一个轻量级、可执行的独立软件包，包含运行某个应用所需的所有内容：代码、运行时、库、环境变量和配置文件。
*   **容器 (Container):** 镜像的运行实例。容器是相互隔离的，并且具有自己的文件系统、网络和进程空间。
*   **仓库 (Repository):** 集中存放镜像文件的场所。Docker Hub 是官方提供的公共仓库。
*   **Dockerfile:** 用来构建 Docker 镜像的文本文件，包含了一系列用户可以连续执行的命令。
*   **Docker Compose:** 一个用于定义和运行多容器 Docker 应用程序的工具。

### 二、镜像管理 (Image)

#### 2.1 搜索与拉取镜像

*   **从 Docker Hub 搜索镜像:**
    ```bash
    docker search [选项] <镜像名称>
    ```
    *   `--limit int`: 指定搜索结果的最大数量，默认为 25。

*   **从仓库拉取镜像:**
    ```bash
    docker pull <镜像名称>[:<标签>]
    ```
    *   如果不指定标签，默认为 `latest`。

#### 2.2 查看与删除镜像

*   **列出本地所有镜像:**
    ```bash
    docker images
    ```
    或
    ```bash
    docker image ls
    ```

*   **删除一个或多个本地镜像:**
    ```bash
    docker rmi <镜像ID或镜像名称> [<镜像ID或镜像名称>...]
    ```
    *   `-f, --force`: 强制删除镜像，即使有容器正在使用它。

*   **删除所有未被使用的镜像（悬空镜像）:**
    ```bash
    docker image prune
    ```
    *   `-a, --all`: 删除所有未被任何容器使用的镜像。

#### 2.3 构建镜像

*   **从 Dockerfile 构建镜像:**
    ```bash
    docker build -t <镜像名称>[:<标签>] <Dockerfile所在目录>
    ```
    *   `-t, --tag`: 指定镜像的名称和标签。
    *   `-f, --file`: 指定要使用的 Dockerfile 路径。
    *   `--no-cache`: 构建镜像时不使用缓存。

#### 2.4 镜像标签与推送

*   **为本地镜像添加新的标签:**
    ```bash
    docker tag <源镜像ID或名称>:<源标签> <目标镜像名称>:<目标标签>
    ```

*   **将镜像推送到远程仓库:**
    ```bash
    docker push <镜像名称>[:<标签>]
    ```
    *   推送前需要先使用 `docker login` 登录。

#### 2.5 镜像导入与导出

*   **将镜像保存为 tar 归档文件:**
    ```bash
    docker save <镜像名称>:<标签> -o <导出路径/文件名.tar>
    ```

*   **从 tar 归档文件加载镜像:**
    ```bash
    docker load -i <文件路径/文件名.tar>
    ```

### 三、容器管理 (Container)

#### 3.1 运行容器

*   **创建一个新的容器并运行:**
    ```bash
    docker run [选项] <镜像名称>[:<标签>] [要执行的命令]
    ```
    *   `-d, --detach`: 后台运行容器，并返回容器ID。
    *   `-i, --interactive`: 保持 STDIN 打开，即使没有附加。
    *   `-t, --tty`: 为容器分配一个伪输入终端。
    *   `--name <容器名称>`: 为容器指定一个名称。
    *   `-p, --publish <主机端口>:<容器端口>`: 将容器的端口映射到主机的端口。
    *   `-v, --volume <主机路径>:<容器路径>`: 将主机的目录挂载到容器中。
    *   `--rm`: 容器退出时自动删除容器。

#### 3.2 查看容器

*   **列出正在运行的容器:**
    ```bash
    docker ps
    ```
    或
    ```bash
    docker container ls
    ```

*   **列出所有容器（包括已停止的）:**
    ```bash
    docker ps -a
    ```
    或
    ```bash
    docker container ls -a
    ```

#### 3.3 启动、停止与重启容器

*   **启动一个或多个已停止的容器:**
    ```bash
    docker start <容器ID或名称> [<容器ID或名称>...]
    ```

*   **停止一个或多个正在运行的容器:**
    ```bash
    docker stop <容器ID或名称> [<容器ID或名称>...]
    ```

*   **重启一个或多个容器:**
    ```bash
    docker restart <容器ID或名称> [<容器ID或名称>...]
    ```

*   **强制停止一个或多个容器:**
    ```bash
    docker kill <容器ID或名称> [<容器ID或名称>...]
    ```

#### 3.4 进入容器

*   **在正在运行的容器中执行命令:**
    ```bash
    docker exec [选项] <容器ID或名称> <命令>
    ```
    *   `-i -t`: 通常一起使用，以交互模式进入容器的 shell。

*   **附加到正在运行的容器:**
    ```bash
    docker attach <容器ID或名称>
    ```

#### 3.5 删除容器

*   **删除一个或多个已停止的容器:**
    ```bash
    docker rm <容器ID或名称> [<容器ID或名称>...]
    ```
    *   `-f, --force`: 强制删除正在运行的容器。

*   **删除所有已停止的容器:**
    ```bash
    docker container prune
    ```
    或
    ```bash
    docker rm $(docker ps -aq -f status=exited)
    ```

#### 3.6 查看容器日志与信息

*   **获取容器的日志:**
    ```bash
    docker logs [选项] <容器ID或名称>
    ```
    *   `-f, --follow`: 跟踪日志输出。
    *   `--tail <行数>`: 显示结尾的指定行数的日志。

*   **显示一个或多个容器的详细信息:**
    ```bash
    docker inspect <容器ID或名称> [<容器ID或名称>...]
    ```

### 四、Dockerfile 指令详解

#### 4.1 核心指令

*   **`FROM <镜像>`:** 指定基础镜像，必须是 Dockerfile 的第一条指令。
*   **`RUN <命令>`:** 在镜像构建过程中执行命令。
*   **`CMD ["<可执行文件>","<参数1>","<参数2>"]`:** 提供容器启动时默认执行的命令。
*   **`ENTRYPOINT ["<可执行文件>","<参数1>","<参数2>"]`:** 配置容器启动后执行的命令。
*   **`COPY <源路径> <目标路径>`:** 将文件或目录从构建上下文复制到镜像中。
*   **`ADD <源路径> <目标路径>`:** 功能与 `COPY` 类似，但 `ADD` 支持 URL 和解压压缩包。

#### 4.2 其他常用指令

*   **`WORKDIR /路径`:** 设置工作目录。
*   **`ENV <键> <值>`:** 设置环境变量。
*   **`EXPOSE <端口>`:** 声明容器运行时监听的端口。
*   **`VOLUME ["/数据卷路径"]`:** 创建一个可以从本地主机或其他容器挂载的挂载点。
*   **`USER <用户名>`:** 指定运行容器时的用户名或 UID。

### 五、Docker Compose 多容器编排

#### 5.1 核心命令

*   **启动并运行所有服务:**
    ```bash
    docker-compose up
    ```
    *   `-d`: 后台运行。

*   **停止并删除容器、网络、卷和镜像:**
    ```bash
    docker-compose down
    ```

*   **构建或重建服务:**
    ```bash
    docker-compose build
    ```

*   **列出服务:**
    ```bash
    docker-compose ps
    ```

*   **查看服务日志:**
    ```bash
    docker-compose logs [服务名称]
    ```
    *   `-f`: 跟踪日志。

#### 5.2 `docker-compose.yml` 常用配置

*   **`version`:** 指定 Compose 文件格式的版本。
*   **`services`:** 定义不同的应用服务。
*   **`image`:** 指定服务使用的镜像。
*   **`build`:** 指定用于构建镜像的 Dockerfile 路径。
*   **`ports`:** 端口映射。
*   **`volumes`:** 数据卷挂载。
*   **`environment`:** 环境变量。
*   **`networks`:** 配置网络。
*   **`depends_on`:** 定义服务间的依赖关系。

### 六、网络管理 (Network)

#### 6.1 网络命令

*   **列出网络:**
    ```bash
    docker network ls
    ```

*   **创建一个网络:**
    ```bash
    docker network create [选项] <网络名称>
    ```

*   **删除一个或多个网络:**
    ```bash
    docker network rm <网络ID或名称> [<网络ID或名称>...]
    ```

*   **将容器连接到一个网络:**
    ```bash
    docker network connect <网络ID或名称> <容器ID或名称>
    ```

#### 6.2 网络驱动

*   **`bridge`:** 默认驱动。
*   **`host`:** 移除容器和 Docker 主机之间的网络隔离。
*   **`overlay`:** 用于 Swarm 服务。
*   **`none`:** 禁用所有网络。

### 七、数据卷管理 (Volume)

#### 7.1 数据卷命令

*   **列出数据卷:**
    ```bash
    docker volume ls
    ```

*   **创建一个数据卷:**
    ```bash
    docker volume create [选项] <数据卷名称>
    ```

*   **删除一个或多个数据卷:**
    ```bash
    docker volume rm <数据卷名称> [<数据卷名称>...]
    ```

*   **删除所有未使用的数据卷:**
    ```bash
    docker volume prune
    ```

#### 7.2 挂载数据卷

*   **在运行容器时挂载数据卷:**
    ```bash
    docker run -v <数据卷名称>:<容器内路径> <镜像名称>
    ```

*   **挂载主机目录作为数据卷:**
    ```bash
    docker run -v <主机路径>:<容器内路径> <镜像名称>
    ```

### 八、系统与其他命令

*   **显示 Docker 系统信息:**
    ```bash
    docker info
    ```

*   **显示 Docker 版本信息:**
    ```bash
    docker version
    ```

*   **清理未使用的 Docker 资源:**
    ```bash
    docker system prune
    ```
    *   `-a`: 同时清理未使用的镜像。
    *   `--volumes`: 同时清理未使用的数据卷。