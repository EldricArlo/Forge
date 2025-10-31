# FFmpeg 命令行工具深度使用指南

$\text{FFmpeg}$ 是一个功能极其强大的开源跨平台音视频处理框架，它能处理几乎所有已知的多媒体格式。其核心是一个命令行工具，用于进行转码、录制、流传输等操作。

## 完整目录

- [FFmpeg 命令行工具深度使用指南](#ffmpeg-命令行工具深度使用指南)
  - [完整目录](#完整目录)
  - [一、基础概念与架构](#一基础概念与架构)
    - [1.1. 什么是 FFmpeg？](#11-什么是-ffmpeg)
    - [1.2. 核心组件](#12-核心组件)
    - [1.3. FFmpeg 命令基本结构](#13-ffmpeg-命令基本结构)
  - [二、基础操作与文件转换](#二基础操作与文件转换)
    - [2.1. 格式转换（转码）](#21-格式转换转码)
    - [2.2. 无损复制（快速封装）](#22-无损复制快速封装)
    - [2.3. 提取音轨或视频轨](#23-提取音轨或视频轨)
    - [2.4. 批量操作（使用 Shell/Batch）](#24-批量操作使用-shellbatch)
  - [三、视频处理与编码控制](#三视频处理与编码控制)
    - [3.1. 改变编解码器](#31-改变编解码器)
    - [3.2. 调整分辨率（-s / -vf scale）](#32-调整分辨率-s---vf-scale)
    - [3.3. 控制比特率与质量](#33-控制比特率与质量)
    - [3.4. 改变帧率（FPS）](#34-改变帧率fps)
  - [四、时间轴操作与剪辑](#四时间轴操作与剪辑)
    - [4.1. 剪辑：指定开始和结束时间](#41-剪辑指定开始和结束时间)
    - [4.2. 视频片段合并（Concat 分离器）](#42-视频片段合并concat-分离器)
    - [4.3. 循环与重复](#43-循环与重复)
  - [五、音频处理与调整](#五音频处理与调整)
    - [5.1. 改变音频编解码器](#51-改变音频编解码器)
    - [5.2. 调整音量](#52-调整音量)
    - [5.3. 混音与声道映射](#53-混音与声道映射)
  - [六、滤镜系统（Filtergraphs）](#六滤镜系统filtergraphs)
    - [6.1. 滤镜概念与 `-vf` / `-af`](#61-滤镜概念与--vf---af)
    - [6.2. 常用视频滤镜](#62-常用视频滤镜)
    - [6.3. 常用音频滤镜](#63-常用音频滤镜)
    - [6.4. 复杂滤镜链（Filterchains）](#64-复杂滤镜链filterchains)
  - [七、高级应用与流处理](#七高级应用与流处理)
    - [7.1. 实时屏幕录制](#71-实时屏幕录制)
    - [7.2. HLS / DASH 流媒体分段](#72-hls--dash-流媒体分段)
    - [7.3. 视频水印与字幕](#73-视频水印与字幕)

---

## 一、基础概念与架构

### 1.1. 什么是 FFmpeg？

FFmpeg 是一套完整的跨平台解决方案，用于记录、转换和流化数字音视频。它由一系列程序和库组成，核心是 `ffmpeg` 命令行工具。它支持几乎所有主要的音视频格式、编解码器和协议。

### 1.2. 核心组件

| 组件 | 描述 |
| :--- | :--- |
| **`ffmpeg`** | 核心命令行工具，用于音视频转码和处理。 |
| **`ffprobe`** | 用于分析媒体流和文件格式的命令行工具，能输出详细的媒体元数据。 |
| **`ffplay`** | 简单的媒体播放器，主要用于测试滤镜和编解码器。 |
| **libavcodec** | 包含了所有 $\text{FFmpeg}$ 的编解码器。 |
| **libavformat** | 包含了所有 $\text{FFmpeg}$ 的复用器和解复用器（处理文件格式）。 |

### 1.3. FFmpeg 命令基本结构

$\text{FFmpeg}$ 命令的结构严格遵循 **输入选项** $\rightarrow$ **输入文件** $\rightarrow$ **输出选项** $\rightarrow$ **输出文件** 的顺序。

**语法：**
```bash
ffmpeg [全局选项] {[输入文件选项] -i [输入文件]} ... {[输出文件选项] [输出文件]} ...
```

**关键选项：**
| 选项 | 描述 |
| :--- | :--- |
| `-i` | **I**nput file，指定输入文件。 |
| `-c` | **C**odec，指定编解码器。 |
| `-c:v` | 视频编解码器（如 `libx264`）。 |
| `-c:a` | 音频编解码器（如 `aac`）。 |
| `-map` | 指定从输入流到输出流的映射关系。 |
| `-y` | 覆盖输出文件而不询问。 |
| `-ss` | **S**tart **S**econds，指定输入或输出的起始时间。 |

---

## 二、基础操作与文件转换

### 2.1. 格式转换（转码）

将一个文件从一种格式转换为另一种格式，通常会涉及编解码器和容器格式的更改。

**示例：**
```bash
# 将 MP4 文件转换为 WebM 格式，使用 VP9 视频编码和 Opus 音频编码
ffmpeg -i input.mp4 -c:v libvpx-vp9 -c:a libopus output.webm

# 将所有流转换为 H.264 (libx264) 和 AAC 编码，输出 MP4 容器
ffmpeg -i input.mov -c:v libx264 -preset medium -crf 23 -c:a aac -b:a 128k output.mp4
```

### 2.2. 无损复制（快速封装）

如果只需要更改**容器格式**（如从 `.mkv` 到 `.mp4`）而不更改编解码器，使用 `-c copy` 可以避免耗时的重新编码过程。

**命令：**
```bash
# 无损复制视频和音频流，仅更改容器格式
ffmpeg -i input.mkv -c copy output.mp4
```

### 2.3. 提取音轨或视频轨

通过 `-vn`（无视频）或 `-an`（无音频）选项，可以轻松提取视频或音频流。

**示例：**
```bash
# 仅提取音频流并保存为 MP3 格式
ffmpeg -i input.mp4 -vn -c:a libmp3lame -q:a 2 output.mp3

# 仅提取视频流（无音频），并无损复制到 MOV 容器
ffmpeg -i input.mp4 -an -c copy output.mov
```

### 2.4. 批量操作（使用 Shell/Batch）

在 Windows 的 Batch 或 Linux/macOS 的 Bash 中，可以结合 $\text{FFmpeg}$ 实现批量文件转换。

**Bash 示例（Linux/macOS）：**
```bash
# 批量将当前目录下所有 .mov 文件转换为 .mp4
for f in *.mov; do ffmpeg -i "$f" -c:v libx264 -crf 23 -c:a aac "${f%.mov}.mp4"; done
```

---

## 三、视频处理与编码控制

### 3.1. 改变编解码器

使用 `-c:v` 指定新的视频编解码器。

| 编码器 | 描述 |
| :--- | :--- |
| `libx264` | H.264/AVC 编码器，最常用。 |
| `libx265` | H.265/HEVC 编码器，高压缩率。 |
| `libvpx-vp9` | Google 的 VP9 编码器。 |
| `h264_nvenc` | NVIDIA 硬件加速 H.264 编码器。 |

### 3.2. 调整分辨率（-s / -vf scale）

*   **`-s`**：简单地指定输出分辨率（如 `1280x720`）。
*   **`-vf scale`**：更灵活，可以使用表达式进行缩放。

**示例：**
```bash
# 简单缩放为 1280x720
ffmpeg -i input.mp4 -s 1280x720 output.mp4

# 使用 -2 保持宽高比：宽度为 720，高度自动计算为 2 的倍数
ffmpeg -i input.mp4 -vf scale=720:-2 output.mp4
```

### 3.3. 控制比特率与质量

| 选项 | 描述 |
| :--- | :--- |
| `-b:v RATE` | 指定**目标视频比特率**（如 `5M`）。用于 ABR（平均比特率）编码。 |
| `-crf VALUE` | **恒定质量因子**（H.264/H.265 常用）。值越低，质量越高，文件越大。推荐值 18-28。 |
| `-preset OPTION` | 预设编码速度和压缩效率。如 `fast`、`medium`、`slow`。`medium` 为默认。 |

**示例（恒定质量编码）：**
```bash
# 使用 CRF 20（高质量）进行 H.264 编码
ffmpeg -i input.mp4 -c:v libx264 -crf 20 output.mp4
```

### 3.4. 改变帧率（FPS）

*   **输入帧率：** `-r` 或 `-framerate`（在 `-i` 之前）用于指定输入帧率，常用于图像序列或设备。
*   **输出帧率：** `-r`（在 `-i` 之后）用于指定输出视频的帧率。

**示例：**
```bash
# 将视频帧率强制转换为 30 FPS
ffmpeg -i input.mp4 -r 30 output.mp4
```

---

## 四、时间轴操作与剪辑

### 4.1. 剪辑：指定开始和结束时间

$\text{FFmpeg}$ 提供了两种主要的剪辑模式，它们在性能和精度上有差异。时间格式可以是 `HH:MM:SS.ms` 或秒数。

| 选项 | 位置 | 描述 | 效率/精度 |
| :--- | :--- | :--- | :--- |
| `-ss` | 在 `-i` **之前** | **Seek to Keyframe。** 快速跳转到最接近的时间点，效率高，但时间点可能不精确。 | **快，不精确** |
| `-ss` | 在 `-i` **之后** | **Decode to Exact Time。** 需从文件头解码到指定时间点，效率低，但时间点精确。 | **慢，精确** |
| `-to` | | 指定输出的**结束时间点**。 | |
| `-t DURATION` | | 指定输出的**持续时间**。 | |

**示例（快速模式，剪辑 10 秒到 20 秒）：**
```bash
ffmpeg -ss 00:00:10 -i input.mp4 -to 00:00:20 -c copy output.mp4
```

### 4.2. 视频片段合并（Concat 分离器）

合并多个具有**相同编解码器和参数**的视频文件，最简单的方法是使用 Concat 分离器。

1.  创建一个文本文件 `list.txt`：
    ```txt
    file 'file1.mp4'
    file 'file2.mp4'
    file 'file3.mp4'
    ```
2.  执行合并（使用 `-c copy` 无损复制）：
    ```bash
    ffmpeg -f concat -i list.txt -c copy output.mp4
    ```

### 4.3. 循环与重复

使用 `-stream_loop` 选项可以在输出中重复输入流指定的次数。`-1` 表示无限循环。

**示例：**
```bash
# 将输入视频重复播放 3 次（总共 4 次）
ffmpeg -i input.mp4 -stream_loop 3 output.mp4
```

---

## 五、音频处理与调整

### 5.1. 改变音频编解码器

使用 `-c:a` 指定音频编解码器。

**示例：**
```bash
# 将音频编码为高质量的 AAC (Advanced Audio Coding)
ffmpeg -i input.mp4 -c:v copy -c:a aac -b:a 192k output.mp4

# 将音频编码为 MP3，指定质量参数 (q:a 0 是最高质量)
ffmpeg -i input.mp4 -c:v copy -c:a libmp3lame -q:a 2 output.mp4
```

### 5.2. 调整音量

使用音频滤镜 `volume`。

**示例：**
```bash
# 音量增加 10dB (音量提高约 3 倍)
ffmpeg -i input.mp4 -c:v copy -af "volume=10dB" output.mp4

# 音量减少一半 (0.5 倍)
ffmpeg -i input.mp4 -c:v copy -af "volume=0.5" output.mp4
```

### 5.3. 混音与声道映射

使用音频滤镜 `pan` 或 `amerge`。

**示例（将立体声降混为单声道）：**
```bash
ffmpeg -i stereo.wav -ac 1 mono.wav
# 或使用滤镜
ffmpeg -i stereo.wav -filter_complex "apad,pan=mono|c0<c0+c1" mono.wav
```

---

## 六、滤镜系统（Filtergraphs）

$\text{FFmpeg}$ 的强大之处在于其滤镜系统，允许对音视频进行复杂的处理。

### 6.1. 滤镜概念与 `-vf` / `-af`

*   **`-vf`** (`-filter:v`)：应用于**视频**流的滤镜图。
*   **`-af`** (`-filter:a`)：应用于**音频**流的滤镜图。
*   **`-filter_complex`**：用于处理多个输入、或需要多个流之间交互的**复杂**滤镜图（如画中画、混音）。

### 6.2. 常用视频滤镜

| 滤镜 | 描述 | 示例 |
| :--- | :--- | :--- |
| `scale` | 改变分辨率。 | `scale=1920:1080` |
| `crop` | 裁剪视频区域。 | `crop=w:h:x:y` (如 `crop=640:480:100:50`) |
| `transpose` | 旋转和翻转。 | `transpose=1` (顺时针 90 度) |
| `drawtext` | 绘制文本水印。 | 见下一章节。 |

### 6.3. 常用音频滤镜

| 滤镜 | 描述 | 示例 |
| :--- | :--- | :--- |
| `volume` | 调整音量。 | `volume=2.0` |
| `silenceremove` | 移除静音部分。 | `silenceremove=start_threshold=0.01` |
| `earwax` | 优化立体声效果。 | `earwax` |

### 6.4. 复杂滤镜链（Filterchains）

滤镜可以串联起来形成链条，用逗号 `,` 分隔。

**示例（缩放并裁剪）：**
```bash
# 先将视频缩放到 1920 宽，然后从左上角 (0,0) 裁剪 100x100 的区域
ffmpeg -i input.mp4 -vf "scale=1920:-2,crop=100:100:0:0" output.mp4
```

---

## 七、高级应用与流处理

### 7.1. 实时屏幕录制

$\text{FFmpeg}$ 可以使用各种输入设备和屏幕捕获协议进行录制。

**示例（macOS/Linux - X11 Grabber）：**
```bash
# 录制屏幕
ffmpeg -f x11grab -s 1920x1080 -i :0.0 -c:v libx264 -preset ultrafast output.mkv
```

### 7.2. HLS / DASH 流媒体分段

$\text{FFmpeg}$ 可以将媒体文件分割成小块（Segment）并生成流媒体清单（Manifest）文件。

**示例（生成 HLS 流）：**
```bash
ffmpeg -i input.mp4 -c:v libx264 -c:a aac -hls_time 10 -hls_playlist_type vod output.m3u8
```

### 7.3. 视频水印与字幕

使用 `drawtext` 滤镜添加文字水印，或使用 `subtitles` / `ass` 滤镜添加字幕。

**示例（添加居中水印）：**
```bash
ffmpeg -i input.mp4 -vf "drawtext=fontfile=/System/Library/Fonts/Supplemental/Arial.ttf:text='Watermark':x=(w-text_w)/2:y=(h-text_h)/2:fontsize=30:fontcolor=white:shadowcolor=black:shadowx=2:shadowy=2" output.mp4
```