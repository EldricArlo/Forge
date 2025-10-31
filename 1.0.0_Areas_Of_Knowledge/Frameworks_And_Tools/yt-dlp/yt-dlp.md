# yt-dlp 命令行工具深度使用指南

`yt-dlp` 是一个功能丰富的命令行音视频下载器，它是著名项目 `youtube-dl` 的一个分支，拥有更活跃的维护、更快的下载速度，并支持更多网站（包括但不限于 YouTube、Bilibili、Twitter、Twitch 等）。

## 完整目录

- [yt-dlp 命令行工具深度使用指南](#yt-dlp-命令行工具深度使用指南)
  - [完整目录](#完整目录)
  - [一、基础入门](#一基础入门)
    - [1.1. 什么是 yt-dlp？](#11-什么是-yt-dlp)
    - [1.2. 核心依赖：FFmpeg](#12-核心依赖ffmpeg)
    - [1.3. 安装与更新](#13-安装与更新)
  - [二、基础下载命令](#二基础下载命令)
    - [2.1. 下载单个视频](#21-下载单个视频)
    - [2.2. 下载播放列表](#22-下载播放列表)
    - [2.3. 批量下载（URL 文件）](#23-批量下载url-文件)
    - [2.4. 程序更新](#24-程序更新)
  - [三、输出与文件系统选项](#三输出与文件系统选项)
    - [3.1. 自定义输出文件名（-o）](#31-自定义输出文件名-o)
    - [3.2. 输出模板变量](#32-输出模板变量)
    - [3.3. 下载限制与跳过](#33-下载限制与跳过)
  - [四、格式选择与质量控制](#四格式选择与质量控制)
    - [4.1. 查看可用格式（-F）](#41-查看可用格式-f)
    - [4.2. 格式选择基础（-f）](#42-格式选择基础-f)
    - [4.3. 格式选择进阶：排序（-S）](#43-格式选择进阶排序-s)
    - [4.4. 仅下载音频](#44-仅下载音频)
  - [五、后处理与元数据](#五后处理与元数据)
    - [5.1. 后处理简介（--ppa）](#51-后处理简介--ppa)
    - [5.2. 嵌入元数据（--embed-metadata）](#52-嵌入元数据--embed-metadata)
    - [5.3. 字幕下载与嵌入](#53-字幕下载与嵌入)
    - [5.4. 缩略图](#54-缩略图)
  - [六、高级功能与配置](#六高级功能与配置)
    - [6.1. 配置文件（--config-location）](#61-配置文件--config-location)
    - [6.2. 绕过限制（Cookies/代理）](#62-绕过限制cookies代理)
    - [6.3. 下载直播](#63-下载直播)

---

## 一、基础入门

### 1.1. 什么是 yt-dlp？

$\text{yt-dlp}$ 是一个基于 Python 的命令行工具，专为下载视频网站上的音视频内容而设计。它继承了 $\text{youtube-dl}$ 的强大功能，并增加了如**更智能的格式选择**、**更多的后处理选项**、**更好的并发下载支持**等特性。

### 1.2. 核心依赖：FFmpeg

为了实现视频和音频流的**合并**（如 YouTube 提供的 DASH 流，视频和音频通常是分离的）、**格式转换**以及**元数据嵌入**等复杂的后处理操作，$\text{yt-dlp}$ 强烈依赖 **FFmpeg**（及其配套的 FFprobe）。请确保您的系统已安装 FFmpeg 并将其加入了系统路径（Path）。

### 1.3. 安装与更新

$\text{yt-dlp}$ 支持 Windows、macOS 和 Linux。推荐的安装方式有：

| 平台 | 安装方式（推荐） | 命令示例 |
| :--- | :--- | :--- |
| **通用** | Python 的 pip | `pip install -U yt-dlp` |
| **Windows** | 下载独立可执行文件 | [yt-dlp.exe](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp.exe) |
| **Linux/macOS** | 包管理器（如 Homebrew, apt）或 pip | `brew install yt-dlp` (macOS) |

---

## 二、基础下载命令

### 2.1. 下载单个视频

这是最基本的用法，只需提供视频的 URL。

**语法：**
```bash
yt-dlp [选项] "视频URL"
```

**示例：**
```bash
# 下载指定视频，使用默认设置（通常是最佳质量的视频和音频合并）
yt-dlp "https://www.youtube.com/watch?v=xxxxxxxxxxx"
```

### 2.2. 下载播放列表

当 URL 指向一个播放列表时，$\text{yt-dlp}$ 会自动识别并尝试下载列表中的所有视频。

**示例：**
```bash
# 下载整个播放列表
yt-dlp "https://www.youtube.com/playlist?list=PLxxxxxxxxxxxx"

# 仅下载播放列表中的第 5 到 10 个视频
yt-dlp --playlist-items 5-10 "播放列表URL"
```

### 2.3. 批量下载（URL 文件）

对于大量视频下载，将 URL 列表保存到文本文件（每行一个 URL）然后使用 `-a` 选项。

**语法：**
```bash
yt-dlp -a [URL文件路径] [选项]
```

**示例：**
```bash
# 从名为 urls.txt 的文件中读取所有 URL 并下载
yt-dlp -a urls.txt
```

### 2.4. 程序更新

如果您是通过 pip 或下载的二进制文件安装的 $\text{yt-dlp}$，可以使用内置命令进行更新。

**命令：**
```bash
yt-dlp -U
# 或
yt-dlp --update
```

---

## 三、输出与文件系统选项

### 3.1. 自定义输出文件名（-o）

`yt-dlp` 强大的地方在于其输出模板，允许您根据视频的元数据动态生成文件名和目录结构。

**语法：**
```bash
yt-dlp -o "模板" "URL"
```

**示例：**
```bash
# 格式：[上传者] 标题 [ID].扩展名
yt-dlp -o "[%(uploader)s] %(title)s [%(id)s].%(ext)s" "URL"

# 格式：存储到以“上传者”命名的子文件夹下，文件名为“上传日期_标题.扩展名”
yt-dlp -o "%(uploader)s/%(upload_date)s_%(title)s.%(ext)s" "URL"
```

### 3.2. 输出模板变量

以下是常用且核心的模板变量：

| 变量 | 描述 |
| :--- | :--- |
| `%(title)s` | 视频标题 |
| `%(id)s` | 视频的唯一 ID |
| `%(ext)s` | 文件扩展名 |
| `%(uploader)s` | 视频上传者名称 |
| `%(upload_date)s` | 视频上传日期（格式：YYYYMMDD） |
| `%(playlist)s` | 播放列表的名称（如果适用） |
| `%(height)s` | 视频的分辨率高度（如 1080） |
| `%(resolution)s` | 视频的完整分辨率字符串（如 1920x1080） |

### 3.3. 下载限制与跳过

| 选项 | 描述 |
| :--- | :--- |
| `-i`, `--ignore-errors` | 忽略下载和后处理中的错误，继续下载播放列表中的下一个视频。 |
| `--abort-on-error` | 如果发生任何错误，立即终止整个下载过程（播放列表模式）。 |
| `--download-archive FILE` | 使用一个归档文件记录已下载视频的 ID，避免重复下载。 |
| `--max-filesize SIZE` | 限制最大文件大小，如 `50m` (50MB) 或 `4g` (4GB)。 |

---

## 四、格式选择与质量控制

### 4.1. 查看可用格式（-F）

在下载之前，使用 `-F` 或 `--list-formats` 选项可以查看所有可用的视频和音频流及其对应的格式代码、分辨率、大小等信息。

**命令：**
```bash
yt-dlp -F "URL"
```

### 4.2. 格式选择基础（-f）

通过 `-f` 或 `--format` 选项，您可以指定要下载的视频和音频格式。

| 格式选择器 | 描述 |
| :--- | :--- |
| `-f best` | 默认选项，选择最佳质量的视频+音频（如果视频和音频是合并的）。 |
| `-f bestvideo+bestaudio` | **最常用**。选择最佳视频流和最佳音频流，然后使用 FFmpeg **合并**。 |
| `-f 137+140` | 精确选择：格式代码为 137（1080p 视频流）和 140（M4A 音频流）并合并。 |
| `-f "bv*[ext=mp4]+ba[ext=m4a]/b"` | 更灵活的选择：选择扩展名为 mp4 的最佳视频流和 m4a 的最佳音频流，如果找不到，则选择最佳合并流。 |

### 4.3. 格式选择进阶：排序（-S）

`-S` 或 `--format-sort` 选项允许您定义复杂的格式排序规则，以精细控制下载质量。

**语法：** `[KEY][DIRECTION],[KEY][DIRECTION],...`

*   **KEY:** `res` (分辨率), `fps` (帧率), `br` (总比特率), `vbr` (视频比特率), `abr` (音频比特率), `ext` (扩展名), `codec` (编解码器) 等。
*   **DIRECTION:** 默认是降序（最佳），`+` 表示升序（最差）。

**示例：**
```bash
# 优先选择分辨率最高的格式，如果分辨率相同，则选择帧率更高的格式。
yt-dlp -S "res,fps" "URL"

# 选择分辨率不超过 720p 的最佳视频流，并且要求编解码器是 h264
yt-dlp -f "bv*[height<=720][vcodec~='h264']+ba" "URL"
```

### 4.4. 仅下载音频

要下载纯音频，请使用 `-x` 选项（Extract Audio），并配合 `--audio-format` 指定目标格式（需要 FFmpeg）。

**示例：**
```bash
# 提取最佳质量的音频，并将其转换为 MP3 格式
yt-dlp -x --audio-format mp3 "URL"

# 提取最佳质量的音频，并将其转换为 Opus 格式
yt-dlp -x --audio-format opus "URL"
```

---

## 五、后处理与元数据

后处理是下载完成后对文件的进一步操作，通常由 FFmpeg 执行。

### 5.1. 后处理简介（--ppa）

Post-Processor Arguments (`--ppa`) 允许您向 FFmpeg 传递额外的命令行参数。

**示例：**
```bash
# 将下载的视频剪辑为从第 10 秒到第 30 秒
yt-dlp "URL" --ppa "SponsorBlock:skip_segments=all" --postprocessor-args "-ss 00:00:10 -to 00:00:30"
```

### 5.2. 嵌入元数据（--embed-metadata）

用于将视频的标题、描述、上传者等信息嵌入到最终的媒体文件中。

**命令：**
```bash
yt-dlp --embed-metadata "URL"
```

### 5.3. 字幕下载与嵌入

| 选项 | 描述 |
| :--- | :--- |
| `--list-subs` | 列出所有可用的字幕语言。 |
| `--write-subs` | 下载所有可用的字幕文件。 |
| `--sub-langs zh-Hans,en` | 仅下载简体中文和英文的字幕。 |
| `--embed-subs` | 将字幕嵌入到视频文件中（需要 FFmpeg）。 |

**示例：**
```bash
# 下载并嵌入简体中文和英文字幕到视频中
yt-dlp --write-subs --sub-langs zh-Hans,en --embed-subs "URL"
```

### 5.4. 缩略图

| 选项 | 描述 |
| :--- | :--- |
| `--write-thumbnail` | 下载最佳质量的视频缩略图。 |
| `--embed-thumbnail` | 将缩略图嵌入到视频文件中（主要用于音频文件作为封面）。 |

---

## 六、高级功能与配置

### 6.1. 配置文件（--config-location）

对于常用的复杂命令，建议创建配置文件（通常是 `yt-dlp.conf`），将常用的选项写入其中，然后 $\text{yt-dlp}$ 在运行时会自动加载。这样可以避免在每次下载时输入冗长命令。

**示例：**
1.  创建一个文件，例如 `yt-dlp.conf`。
2.  在文件中写入常用选项：
    ```conf
    # yt-dlp.conf 示例
    -i
    -o ~/Videos/Downloaded/%(uploader)s/%(title)s.%(ext)s
    -f bestvideo[ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]/best
    --embed-metadata
    --embed-subs
    ```
3.  运行时，$\text{yt-dlp}$ 会自动应用这些配置（除非使用 `--ignore-config`）。

### 6.2. 绕过限制（Cookies/代理）

*   **登录/会员视频（Cookies）：**
    *   `--cookies FILE`：使用浏览器导出的 **Netscape 格式** 的 Cookies 文件，以下载需要登录或会员权限的视频。
*   **地理限制（代理）：**
    *   `--proxy URL`：使用指定的 HTTP/HTTPS/SOCKS 代理服务器。
    *   **示例：** `yt-dlp --proxy "socks5://127.0.0.1:1080" "URL"`

### 6.3. 下载直播

$\text{yt-dlp}$ 可以处理进行中的直播和预定的直播。

**选项：**
| 选项 | 描述 |
| :--- | :--- |
| `--live-from-start` | 从直播的起点（而不是当前时间点）开始下载。 |
| `--wait-for-video TIME` | 等待预定的直播开始，例如 `300` (等待 5 分钟)。

**示例：**
```bash
# 等待直播开始，并从头开始下载
yt-dlp --wait-for-video 300 --live-from-start "直播URL"
```