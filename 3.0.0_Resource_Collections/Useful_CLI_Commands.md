## CLI 命令终极精要与实践手册

---

## 📄 目录 (Table of Contents)

- [CLI 命令终极精要与实践手册](#cli-命令终极精要与实践手册)
- [📄 目录 (Table of Contents)](#-目录-table-of-contents)
- [1. 文件与目录管理 (File \& Directory Management)](#1-文件与目录管理-file--directory-management)
  - [1.1. 导航与查看](#11-导航与查看)
  - [1.2. 创建、移动与删除](#12-创建移动与删除)
  - [1.3. 查找与定位](#13-查找与定位)
- [2. 文本内容处理 (Text Content Processing)](#2-文本内容处理-text-content-processing)
  - [2.1. 查看与读取](#21-查看与读取)
  - [2.2. 筛选与搜索 (grep)](#22-筛选与搜索-grep)
  - [2.3. 数据流处理 (sed \& awk)](#23-数据流处理-sed--awk)
- [3. 系统信息与资源监控 (System Info \& Monitoring)](#3-系统信息与资源监控-system-info--monitoring)
  - [3.1. 进程管理](#31-进程管理)
  - [3.2. 磁盘与内存](#32-磁盘与内存)
  - [3.3. 系统环境](#33-系统环境)
- [4. 权限与用户管理 (Permissions \& User Management)](#4-权限与用户管理-permissions--user-management)
  - [4.1. 权限操作](#41-权限操作)
  - [4.2. 用户操作](#42-用户操作)
- [5. 网络与远程连接 (Networking \& Remote Access)](#5-网络与远程连接-networking--remote-access)
  - [5.1. 网络诊断](#51-网络诊断)
  - [5.2. 数据传输](#52-数据传输)
  - [5.3. 远程会话](#53-远程会话)
- [6. 压缩与归档 (Compression \& Archiving)](#6-压缩与归档-compression--archiving)
  - [6.1. tar](#61-tar)
  - [6.2. zip/unzip](#62-zipunzip)

---

## 1. 文件与目录管理 (File & Directory Management)

### 1.1. 导航与查看

| 命令 | 描述 | 常用选项 (Flags) | 详细用法示例 |
| :--- | :--- | :--- | :--- |
| **`pwd`** | 打印当前工作目录的绝对路径。 | `-P` (显示物理路径，而非符号链接) | `pwd` |
| **`cd`** | 更改目录。 | (无) | `cd /var/log` (进入指定目录); `cd ..` (返回上一级); `cd ~` (返回用户主目录) |
| **`ls`** | 列出目录内容。 | `-l` (长格式显示，包含权限、大小、日期等) | `ls -l` |
| | | `-a` (显示所有文件，包括隐藏文件) | `ls -la` |
| | | `-h` (以人类可读格式显示文件大小，如 1K, 234M) | `ls -lh` |
| **`tree`** | (需安装) 以树状结构递归列出目录内容。 | `-L N` (限制显示层级 N) | `tree -L 2 /home/user` |

### 1.2. 创建、移动与删除

| 命令 | 描述 | 常用选项 (Flags) | 详细用法示例 |
| :--- | :--- | :--- | :--- |
| **`mkdir`** | 创建目录 (Make Directory)。 | `-p` (递归创建目录，即使父目录不存在) | `mkdir -p project/src/data` |
| **`touch`** | 创建空文件，或更新现有文件的访问/修改时间。 | (无) | `touch new_script.sh` |
| **`cp`** | 复制文件或目录。 | `-r` (递归复制，用于目录) | `cp -r /source/dir /dest/dir` |
| | | `-i` (覆盖前进行提示) | `cp -i file1 file_backup` |
| **`mv`** | 移动或重命名文件/目录。 | (无) | `mv old_name.txt new_name.txt` (重命名); `mv file.log /tmp/logs/` (移动) |
| **`rm`** | 删除文件或目录。 | `-f` (强制删除，无提示) | `rm -f file_to_delete.txt` |
| | | `-r` (递归删除，用于目录) | `rm -rf sensitive_dir` **(谨慎使用！)** |
| **`ln`** | 创建链接 (Link)。 | `-s` (创建符号链接/软链接) | `ln -s /path/to/original /path/to/link` |

### 1.3. 查找与定位

| 命令 | 描述 | 常用选项 (Flags) | 详细用法示例 |
| :--- | :--- | :--- | :--- |
| **`find`** | 在指定目录及其子目录中查找文件。 | `-name` (按名称查找，支持通配符) | `find /var/log -name "*.log"` |
| | | `-type` (按类型查找，如 `f` 文件, `d` 目录) | `find . -type f -size +100M` (查找大于 100M 的文件) |
| | | `-exec` (对找到的文件执行命令) | `find . -name "*.bak" -exec rm {} \;` (删除所有 .bak 文件) |
| **`locate`** | 通过数据库快速查找文件 (速度快，但可能不实时)。 | (无) | `locate bashrc` |

---

## 2. 文本内容处理 (Text Content Processing)

### 2.1. 查看与读取

| 命令 | 描述 | 常用选项 (Flags) | 详细用法示例 |
| :--- | :--- | :--- | :--- |
| **`cat`** | 连接文件内容并打印到标准输出 (适合小文件)。 | `-n` (显示行号) | `cat -n script.sh` |
| **`less`** | 分页查看文件内容 (推荐用于大文件)。 | (交互式) | `less large_data.log` (按 `q` 退出，按 `/` 搜索) |
| **`head`** | 查看文件头部内容 (默认前 10 行)。 | `-n N` (指定行数 N) | `head -n 5 access.log` (查看前 5 行) |
| **`tail`** | 查看文件尾部内容 (默认后 10 行)。 | `-f` (实时跟踪文件末尾追加的内容) | `tail -f /var/log/syslog` (用于实时监控日志) |

### 2.2. 筛选与搜索 (grep)

| 命令 | 描述 | 常用选项 (Flags) | 详细用法示例 |
| :--- | :--- | :--- | :--- |
| **`grep`** | 根据正则表达式搜索文件内容，并打印匹配的行。 | `-i` (忽略大小写) | `grep -i "error" server.log` |
| | | `-v` (反向匹配，排除包含模式的行) | `grep -v "INFO" debug.log` |
| | | `-r` (递归搜索子目录中的文件) | `grep -r "database_conn" .` |
| | | `-E` 或 `egrep` (使用扩展正则表达式) | `grep -E '^(Error|Warning)' app.log` |

### 2.3. 数据流处理 (sed & awk)

| 命令 | 描述 | 常用选项 (Flags) | 详细用法示例 |
| :--- | :--- | :--- | :--- |
| **`sed`** | 流编辑器，主要用于对文本进行非交互式转换和替换。 | `-i` (直接修改文件内容) | `sed 's/old_text/new_text/g' file.txt` (全局替换) |
| | | (无) | `ls -l | sed '1d'` (删除 `ls` 输出的第一行标题) |
| **`awk`** | 强大的文本分析工具，按字段处理数据。 | (无) | `awk '{print $1, $3}' access.log` (打印日志的第一和第三个字段) |
| | | (无) | `ps aux | awk '$8 ~ /D/ {print $2}'` (查找处于不可中断睡眠状态的进程 ID) |

---

## 3. 系统信息与资源监控 (System Info & Monitoring)

### 3.1. 进程管理

| 命令 | 描述 | 常用选项 (Flags) | 详细用法示例 |
| :--- | :--- | :--- | :--- |
| **`ps`** | 报告当前进程的快照。 | `aux` (显示所有用户的进程，包含详细信息) | `ps aux | grep nginx` (查找 Nginx 进程) |
| **`top`** | 实时显示系统进程、CPU、内存等资源使用情况。 | (交互式) | `top` (按 `Shift+P` 按 CPU 排序，按 `Shift+M` 按内存排序) |
| **`htop`** | (需安装) `top` 的增强版本，交互性更强，界面更友好。 | (交互式) | `htop` |
| **`kill`** | 向进程发送信号 (默认是终止信号)。 | `-9` (强制终止进程, SIGKILL) | `kill -9 12345` (终止 PID 为 12345 的进程) |
| **`killall`** | 按名称终止所有相关进程。 | (无) | `killall firefox` |
| **`lsof`** | 列出打开文件 (List Open Files)，用于诊断文件和网络连接。 | `-i :PORT` (显示使用特定端口的进程) | `lsof -i :8080` |

### 3.2. 磁盘与内存

| 命令 | 描述 | 常用选项 (Flags) | 详细用法示例 |
| :--- | :--- | :--- | :--- |
| **`df`** | 报告文件系统的磁盘空间使用情况。 | `-h` (人类可读格式) | `df -h` |
| **`du`** | 估算文件或目录的磁盘空间使用量。 | `-sh` (显示总和 (s) 并以人类可读格式 (h)) | `du -sh /var/log` |
| **`free`** | 显示系统内存使用情况。 | `-h` (人类可读格式) | `free -h` |

### 3.3. 系统环境

| 命令 | 描述 | 常用选项 (Flags) | 详细用法示例 |
| :--- | :--- | :--- | :--- |
| **`uname`** | 打印当前系统信息。 | `-a` (打印所有信息，如内核版本、操作系统) | `uname -a` |
| **`history`** | 显示已执行的命令历史记录。 | (无) | `history | grep ssh` (在历史记录中搜索 ssh 命令) |
| **`alias`** | 创建或显示命令别名。 | (无) | `alias ll='ls -lhA'` |

---

## 4. 权限与用户管理 (Permissions & User Management)

### 4.1. 权限操作

| 命令 | 描述 | 常用选项 (Flags) | 详细用法示例 |
| :--- | :--- | :--- | :--- |
| **`chmod`** | 更改文件或目录的权限。 | `-R` (递归更改子目录和文件权限) | `chmod 755 script.sh` (rwxr-xr-x) |
| | | (符号模式) | `chmod u+x script.sh` (给用户添加执行权限) |
| **`chown`** | 更改文件或目录的所有者。 | `-R` (递归更改) | `chown -R user:group /path/to/dir` |
| **`sudo`** | 以超级用户 (或指定用户) 的身份执行命令。 | (无) | `sudo apt update` |

### 4.2. 用户操作

| 命令 | 描述 | 常用选项 (Flags) | 详细用法示例 |
| :--- | :--- | :--- | :--- |
| **`whoami`** | 显示当前登录用户的用户名。 | (无) | `whoami` |
| **`id`** | 显示用户和组 ID。 | (无) | `id newuser` |
| **`passwd`** | 更改用户密码。 | (无) | `passwd` (更改当前用户密码); `sudo passwd root` (更改 root 密码) |

---

## 5. 网络与远程连接 (Networking & Remote Access)

### 5.1. 网络诊断

| 命令 | 描述 | 常用选项 (Flags) | 详细用法示例 |
| :--- | :--- | :--- | :--- |
| **`ping`** | 发送 ICMP ECHO_REQUEST 消息到网络主机。 | `-c N` (发送 N 个包后停止) | `ping -c 4 google.com` |
| **`ip a`** | 显示网络接口信息 (推荐替代 `ifconfig`)。 | (无) | `ip a` |
| **`netstat`** | 显示网络连接、路由表、接口统计等。 | `-tuln` (显示 TCP/UDP 监听端口，不解析服务名) | `netstat -tuln` |
| **`ss`** | (Socket Statistics) 替代 `netstat`，速度更快。 | `-tuln` (与 netstat 类似) | `ss -tuln` |
| **`traceroute`** | 跟踪数据包从本地到目标主机的路径。 | (无) | `traceroute baidu.com` |

### 5.2. 数据传输

| 命令 | 描述 | 常用选项 (Flags) | 详细用法示例 |
| :--- | :--- | :--- | :--- |
| **`curl`** | 从服务器传输数据，支持多种协议 (HTTP, FTP 等)。 | `-O` (下载文件并保持原始名称) | `curl -O http://example.com/file.zip` |
| | | `-I` (只显示 HTTP 头部信息) | `curl -I https://api.github.com` |
| **`wget`** | 非交互式下载网络文件。 | `-r` (递归下载整个网站) | `wget --no-check-certificate https://example.com/data.tar.gz` |

### 5.3. 远程会话

| 命令 | 描述 | 常用选项 (Flags) | 详细用法示例 |
| :--- | :--- | :--- | :--- |
| **`ssh`** | 安全外壳协议，用于远程登录和安全通信。 | `-p PORT` (指定端口) | `ssh user@remote_server -p 2222` |
| **`scp`** | 安全复制文件 (基于 SSH)。 | `-r` (递归复制目录) | `scp -r local_dir user@remote:/path/` |
| **`rsync`** | 远程文件同步，高效且支持断点续传。 | `-avz` (归档模式，详细输出，压缩) | `rsync -avz /local/path/ user@remote:/remote/path/` |

---

## 6. 压缩与归档 (Compression & Archiving)

### 6.1. tar

| 命令 | 描述 | 常用选项 (Flags) | 详细用法示例 |
| :--- | :--- | :--- | :--- |
| **`tar`** | 打包和解包文件 (Linux 下最常用)。 | `-c` (创建归档文件) | `tar -cvf archive.tar /path/to/files` (打包) |
| | | `-x` (解开归档文件) | `tar -xvf archive.tar` (解包) |
| | | `-z` (通过 Gzip 压缩/解压) | `tar -czvf archive.tar.gz /data` (打包并压缩) |
| | | `-j` (通过 Bzip2 压缩/解压) | `tar -xjvf archive.tar.bz2` |
| | | `-v` (显示详细过程) | (通常与 c/x 连用) |
| | | `-f` (指定归档文件名) | (必须的，且通常放在最后) |

**最常用组合：**

*   **压缩：** `tar -czvf archive_name.tar.gz /source/directory`
*   **解压：** `tar -xzvf archive_name.tar.gz`

### 6.2. zip/unzip

| 命令 | 描述 | 常用选项 (Flags) | 详细用法示例 |
| :--- | :--- | :--- | :--- |
| **`zip`** | 创建 `.zip` 格式的压缩文件。 | `-r` (递归压缩目录) | `zip -r archive.zip directory_name` |
| **`unzip`** | 解压 `.zip` 格式的文件。 | `-d` (指定解压目录) | `unzip archive.zip -d /tmp/extract_here` |
