## Clash for Android 与 Clash for Windows 使用指南 (Clash Meta & Clash Verge Rev)

**重要提示：** 由于原始的 Clash for Android 和 Clash for Windows 项目的原作者停止维护（并完全的删除了相关仓库，现在网络上能够找到的只有遗留下的已打包完成的安装包，但由于版本的并不高，不建议实际使用）。

本指南将重点介绍其功能更强大且仍在积极更新的后继版本：适用于Android的 **Clash Meta for Android** 和适用于 Windows 的 **Clash Verge Rev**。 这两款应用继承了原版的核心功能，并提供了更广泛的协议支持和更丰富的功能。

还有一个能够适用于Mac book的项目**ClashX**，由于我没有Mac book，故在此不做过多的说明。

### 核心概念：Clash 是什么？

Clash 是一款基于规则的多协议代理工具。其核心优势在于能够根据用户设定的规则，自动判断网络流量是需要通过代理服务器访问（例如访问防火墙禁止的应用），还是直接连接（例如访问防火墙内部允许的应用），从而实现智能分流。

这些相关的内容都可以由一个yaml文件来配置。

---

### 第一部分：Clash Meta for Android 使用教学

Clash Meta for Android (CMFA) 是原版 Clash for Android 的升级版，使用了功能更强大的 Clash Meta 内核，支持包括 V2Ray, Trojan, Shadowsocks(R) 和 Socks 在内的多种代理协议。

#### 基础使用方式

1.  **下载与安装：**
    *   由于已从 Google Play 商店下架，您需要从项目的 GitHub 发布页面或其他可信的第三方网站下载 APK 文件进行安装。
    *   这里建议在Github中下载。
    *   安装时，系统可能会提示您需要授权“安装未知来源的应用”，请予以允许。

2.  **获取订阅链接：**
    *   您需要一个机场服务提供商来获取代理服务器节点。服务商会提供一个 Clash 订阅链接。

3.  **导入订阅链接：**
    *   打开 Clash Meta for Android 应用。
    *   点击主界面的“配置”选项卡。
    *   点击右上角的“+”号，选择“从URL导入”。
    *   在“名称”栏中随意填写一个名称，在“URL”栏中粘贴您的订阅链接。
    *   建议设置一个自动更新时间（例如 720 分钟），以定期同步最新的服务器节点信息。
    *   点击右上角的“保存”按钮。

4.  **启动与连接：**
    *   返回应用主界面，点击刚刚导入的配置文件以激活它。
    *   点击主界面顶部的“已停止”按钮，它会变为“运行中”，表示代理服务已启动。首次启动时，系统会请求创建 VPN 连接的权限，请允许。

5.  **选择节点与代理模式：**
    *   点击底部的“代理”选项卡，您可以看到所有可用的服务器节点。
    *   您可以点击右上角的闪电图标对所有节点进行延迟测试，然后选择一个延迟较低的节点。
    *   在“代理”页面的右上角，您可以选择代理模式：
        *   **规则 (Rule)** (推荐): 根据配置文件中的规则进行智能分流，国内网站直连，国外网站走代理。
        *   **全局 (Global)**: 所有网络流量都通过代理服务器转发。
        *   **直连 (Direct)**: 所有网络流量都不走代理。

#### 进阶使用方式

*   **应用分流：** 在“设置” -> “分应用代理”中，您可以指定哪些应用需要通过代理，哪些应用不通过。
*   **覆写设置 (Override):** 在“设置” -> “覆写”中，您可以对配置文件的某些参数进行自定义修改，而无需更改原始的订阅文件。例如，您可以自定义 DNS 设置或修改某些规则。
*   **查看日志：** 点击底部“日志”选项卡，可以查看实时的网络连接请求和规则匹配情况，方便排查问题。

---

### 第二部分：Clash Verge Rev for Windows 使用教学

Clash Verge Rev 是 Clash Verge 的延续版本，基于 Tauri 框架和 Clash Meta (mihomo) 内核，是一款美观且功能强大的 Windows 客户端。

#### 基础使用方式

1.  **下载与安装：**
    *   从 Clash Verge Rev 的 GitHub 发布页面下载最新的安装包（通常是 `Clash.Verge.Rev_x.x.x_x64-setup.exe`）。
    *   按照安装向导完成安装。

2.  **导入订阅链接：**
    *   打开 Clash Verge Rev。
    *   点击左侧菜单栏的“订阅”。
    *   在顶部的输入框中粘贴您的 Clash 订阅链接，然后点击“导入”。
    *   导入成功后，列表中会显示您的配置文件，单击该文件即可激活使用。

3.  **开启系统代理：**
    *   点击左侧菜单栏的“设置”。
    *   在“系统代理”部分，打开开关以启用。此时，您的浏览器等应用的流量将开始通过 Clash。

4.  **选择节点与代理模式：**
    *   点击左侧菜单栏的“代理”。
    *   在顶部可以选择代理模式：**规则 (Rule)**, **全局 (Global)**, **直连 (Direct)**。通常推荐使用“规则”模式。
    *   在下方的节点列表中，您可以看到不同的策略组（例如：自动选择、香港节点等）。您可以展开策略组，手动选择一个希望使用的服务器节点。

#### 进阶使用方式

*   **TUN 模式：** 对于不支持系统代理的应用程序（如部分游戏或命令行工具），可以在“设置”中开启 TUN 模式。TUN 模式会创建一个虚拟网卡来接管所有网络流量，代理效果更彻底。
*   **配置文件增强 (Merge & Script):**
    *   **Merge (合并):** 允许您在不修改主订阅文件的情况下，额外添加或覆盖某些配置，例如添加自定义规则。
    *   **Script (脚本):** 提供通过编写 JavaScript 脚本来动态修改配置的能力，实现更高级的自定义功能，例如根据节点名称的关键字进行过滤。
*   **系统代理守护：** 在“设置” -> “系统代理”的设置项中，可以开启“代理守护”，该功能会定期检查并恢复系统代理设置，防止被其他软件意外修改。
*   **日志与连接：**
    *   **日志:** 左侧“日志”菜单可以查看 Clash 内核的运行日志。
    *   **连接:** 左侧“连接”菜单可以实时查看当前的网络连接情况，包括流量速度、来源、目的地和匹配的规则。

---

### 第三部分：YAML 配置文件详解

YAML 是 Clash 的灵魂，它定义了代理如何工作。一个完整的 `config.yaml` 文件通常包含以下几个核心部分。

#### 1. 基础配置 (General)

这部分定义了 Clash 的一些基本端口和模式。

```yaml
port: 7890             # HTTP 代理端口
socks-port: 7891       # SOCKS5 代理端口
allow-lan: false         # 是否允许局域网连接
mode: rule             # 代理模式: rule, global, direct
log-level: info        # 日志输出级别: silent, error, warning, info, debug
external-controller: 127.0.0.1:9090 # 外部 UI 控制器地址
```

#### 2. 代理服务器 (Proxies)

这里是您所有代理服务器节点的列表。通常这部分内容由您的机场订阅提供，您也可以手动添加。

**格式示例：**

```yaml
proxies:
  - name: "Shadowsocks-Server"
    type: ss
    server: server.domain.com
    port: 443
    cipher: aes-256-gcm
    password: "your-password"

  - name: "VMess-Server"
    type: vmess
    server: server.another.com
    port: 443
    uuid: "your-uuid"
    alterId: 0
    cipher: auto
    tls: true
    network: ws
    ws-opts:
      path: "/path"
      headers:
        Host: server.another.com
```

#### 3. 代理组 (Proxy Groups)

代理组用于将多个独立的代理节点组合起来，实现负载均衡、自动故障转移或手动选择等策略。

**格式示例：**

```yaml
proxy-groups:
  - name: "PROXY"  # 策略组名称
    type: select      # 策略组类型
    proxies:          # 包含的节点或其他策略组
      - "Auto-Select"
      - "Manual-Select"
      - "Shadowsocks-Server"
      - "VMess-Server"

  - name: "Auto-Select"
    type: url-test    # URL-Test 类型：自动选择延迟最低的节点
    proxies:
      - "Shadowsocks-Server"
      - "VMess-Server"
    url: 'http://www.gstatic.com/generate_204' # 用于测试延迟的 URL
    interval: 300     # 测试间隔（秒）

  - name: "Manual-Select"
    type: select      # Select 类型：手动选择节点
    proxies:
      - "Shadowsocks-Server"
      - "VMess-Server"
      - DIRECT          # DIRECT 表示直连

  - name: "Fallback-Group"
    type: fallback    # Fallback 类型：故障转移，当列表顶部的节点不可用时，自动切换到下一个
    proxies:
      - "VMess-Server"
      - "Shadowsocks-Server"
    url: 'http://www.gstatic.com/generate_204'
    interval: 300
```

#### 4. 规则 (Rules)

规则是 Clash 实现智能分流的核心，Clash 会从上到下逐条匹配规则，一旦匹配成功，就会按照该规则指定的策略组（如 PROXY, DIRECT）来处理流量。

**格式与常见类型：**

*   **DOMAIN-SUFFIX:** 匹配域名后缀。
    *   `DOMAIN-SUFFIX,google.com,PROXY` (匹配所有 `google.com` 及其子域名，走 PROXY 策略组)
*   **DOMAIN-KEYWORD:** 匹配域名关键字。
    *   `DOMAIN-KEYWORD,google,PROXY` (匹配域名中包含 `google` 的流量)
*   **GEOIP:** 匹配国家或地区的 IP。
    *   `GEOIP,CN,DIRECT` (匹配中国的 IP，走直连)
*   **IP-CIDR:** 匹配一个 IP 地址段。
    *   `IP-CIDR,192.168.1.0/24,DIRECT` (匹配局域网地址，走直连)
*   **GEOSITE:** 匹配 V2Ray 的预定义域名列表。
    *   `GEOSITE,google,PROXY` (匹配 Google 相关服务的域名)
*   **MATCH:** 最终规则，当以上所有规则都未匹配时，将采用此规则。
    *   `MATCH,PROXY` (所有未匹配的流量都走 PROXY 策略组)

**规则示例：**

```yaml
rules:
  - DOMAIN-SUFFIX,google.com,PROXY
  - DOMAIN-SUFFIX,youtube.com,PROXY
  - DOMAIN-KEYWORD,bilibili,DIRECT
  - GEOSITE,cn,DIRECT
  - GEOIP,CN,DIRECT
  - MATCH,PROXY
```

通过理解和组合以上这些配置项，您可以打造出完全符合自己使用习惯的强大代理客户端。对于大多数用户而言，直接使用服务商提供的订阅链接已足够满足日常需求。而对于希望深度定制的用户，理解并手动编写 YAML 配置文件将为您打开新世界的大门