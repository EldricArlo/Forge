# Areas_Of_Knowledge\Security_And_Cryptography\Authentication\TOTP_Supported_Services_List.md

### **支持TOTP（兼容验证-器应用）的软件与网站大全 **

TOTP（Time-based One-Time Password，基于时间的一次性密码）是一种广泛应用的多因素认证（MFA）标准，通过“验证器应用”（Authenticator App）生成动态验证码，能够显著提升账户的安全性。几乎所有提示您使用“验证器应用”或“Authenticator App”进行两步验证（2FA）的服务，都支持像Google Authenticator, Microsoft Authenticator, Authy或Bitwarden等标准的TOTP工具。

以下是支持TOTP的服务列表，已补充相关链接和说明：

---

#### **一、 云服务、开发与IT基础设施 (强烈推荐)**

这类服务通常掌管着核心数据和关键业务，开启TOTP至关重要。

*   **云平台提供商:**
    *   **[Amazon Web Services (AWS)](https://aws.amazon.com/)**: 亚马逊提供的全球最全面的云计算服务。
    *   **[Microsoft Azure](https://azure.microsoft.com/)**: 微软提供的企业级云计算平台，支持广泛的服务和解决方案。
    *   **[Google Cloud Platform (GCP)](https://cloud.google.com/)**: 谷歌提供的云计算服务套件，与Google生态系统深度集成。
    *   **[DigitalOcean](https://www.digitalocean.com/)**: 面向开发者的云服务商，以其简洁易用的界面和可预测的定价而闻名。
    *   **[Linode](https://www.linode.com/)**: (已被Akamai收购) 历史悠久的高性能Linux云服务器提供商。

*   **代码托管与DevOps:**
    *   **[GitHub](https://github.com)**: 全球最大的代码托管平台，是开源和私有软件开发的核心枢纽。
    *   **[GitLab](https://gitlab.com)**: 提供从代码管理到CI/CD、监控等功能的一体化DevOps平台。
    *   **[Bitbucket](https://bitbucket.org/)**: Atlassian公司旗下的代码托管服务，与Jira等产品深度集成。
    *   **[Jenkins](https://www.jenkins.io/)**: 开源的持续集成与持续交付(CI/CD)工具，拥有庞大的插件生态系统。
    *   **[JFrog Artifactory](https://jfrog.com/artifactory/)**: 通用的二进制仓库管理器，用于存储和管理软件开发过程中的各种构建产物。
    *   **Chef / Puppet / Ansible**: 自动化配置管理工具的管理界面通常都支持MFA以增强安全性。

*   **网站托管、域名与网络服务:**
    *   **[GoDaddy](https://www.godaddy.com/)**: 全球最大的域名注册商和网站托管服务商之一。
    *   **[Namecheap](https://www.namecheap.com/)**: 以高性价比著称的域名注册和网站托管服务商。
    *   **[Hostinger](https://www.hostinger.com/)**: 提供共享主机、云主机和VPS等多种托管服务的国际化品牌。
    *   **[Cloudflare](https://www.cloudflare.com/)**: 提供CDN、DNS、DDoS防护及网络安全服务的全球网络平台。
    *   **[Akamai](https://www.akamai.com/)** / **[Fastly](https://www.fastly.com/)**: 主流的企业级内容分发网络（CDN）和边缘计算服务商。
    *   **[cPanel / WHM](https://cpanel.net/)** & **[Plesk](https://www.plesk.com/)**: 业界领先的网站托管和服务器管理面板。

*   **服务器与系统管理:**
    *   **Linux服务器 (SSH登录)**: 可通过配置PAM模块（如`google_authenticator_libpam`）为SSH登录强制要求TOTP验证码。
    *   **[VMware vSphere](https://www.vmware.com/products/vsphere.html)**: 业界领先的服务器虚拟化和云基础设施管理平台。
    *   **[Grafana](https://grafana.com/)**: 开源的数据可视化与监控分析平台，常与Prometheus等配合使用。
    *   **[Prometheus](https://prometheus.io/)**: 一款开源的系统监控和警报工具包，特别适用于云原生环境。

*   **数据库服务:**
    *   **[MongoDB Atlas](https://www.mongodb.com/cloud/atlas)**: MongoDB官方提供的全托管云数据库服务。
    *   **[Google Cloud SQL](https://cloud.google.com/sql)**: 谷歌云提供的全托管关系型数据库服务（支持MySQL, PostgreSQL, SQL Server）。

---

#### **二、 金融、支付与加密货币 (强烈推荐)**

直接关系到资金安全，必须开启。

*   **加密货币交易所:**
    *   **[币安 (Binance)](https://www.binance.com/)**: 全球交易量最大的加密货币交易所之一。
    *   **[欧易 (OKX)](https://www.okx.com/)**: 领先的数字资产和加密货币交易平台。
    *   **[Coinbase](https://www.coinbase.com/)**: 总部位于美国的知名加密货币交易所，以合规和易用性著称。
    *   **[Kraken](https://www.kraken.com/)**: 以安全性和可靠性著称的加密货币交易所。
    *   **[Bybit](https://www.bybit.com/)**: 专注于加密货币衍生品交易的平台。
    *   **[Gate.io](https://www.gate.io/)**: 提供数百种加密货币交易的老牌交易所。
    *   **[KuCoin](https://www.kucoin.com/)**: 被誉为“人民的交易所”，提供丰富的山寨币交易对。
    *   **[Bitfinex](https://www.bitfinex.com/)**: 面向专业交易者的老牌加密货币交易平台。
    *   **[Crypto.com](https://crypto.com/)**: 提供交易、支付、借贷等多种服务的综合性加密货币平台。

*   **在线支付与国际汇款:**
    *   **[PayPal](https://www.paypal.com/)**: 全球最广泛使用的在线支付服务之一。
    *   **[Stripe](https://stripe.com/)**: 面向开发者和企业的在线支付处理平台，提供强大的API。
    *   **[Wise](https://wise.com/)** (原 TransferWise): 提供低成本、透明的国际汇款和多币种账户服务。

*   **传统金融与投资:**
    *   **[Charles Schwab](https://www.schwab.com/)**, **[Fidelity](https://www.fidelity.com/)**, **[Vanguard](https://vanguard.com/)**: 美国主流的在线券商和投资管理公司。
    *   **[Interactive Brokers (盈透证券)](https://www.interactivebrokers.com/)**: 备受专业交易者青睐的全球性电子经纪商。
    *   **[eToro](https://www.etoro.com/)**: 领先的社交投资和多资产交易平台。
    *   *说明: 各国网上银行对TOTP的支持情况差异较大，请直接查询您所使用的银行。*
    *   *提醒: 银行系统对IP地址的变动非常敏感，为了您的账户安全，建议使用稳定、可信的网络环境。*

*   **加密货币钱包:**
    *   **[MetaMask](https://metamask.io/)**: 最流行的浏览器扩展形式的以太坊及兼容链钱包。
    *   **[Exodus Wallet](https://www.exodus.com/)**: 设计精美的桌面和移动端多资产加密货币钱包。
    *   **[Trust Wallet](https://trustwallet.com/)**: 币安官方支持的多功能移动端加密货币钱包。

---

#### **三、 社交媒体与通信平台**

保护您的个人隐私和社交关系。

*   **主流社交平台:**
    *   **[Google (谷歌)](https://myaccount.google.com/security)**: 旗下所有服务，如Gmail、YouTube、Google Drive等。
    *   **[Apple ID](https://appleid.apple.com/)**: 苹果生态系统核心账户，用于iCloud、App Store等。
    *   **[Meta (Facebook, Instagram)](https://www.facebook.com/security/2fac/settings)**: 全球最大的社交网络，保护您的个人资料和帖子。
    *   **[X (原 Twitter)](https://x.com)**: 知名社交媒体和实时信息平台。
    *   **[Reddit](https://reddit.com)**: 全球大型社区论坛集合网站。
    *   **[LinkedIn (领英)](https://linkedin.com)**: 全球最大的职业社交平台。
    *   **[Pinterest](https://pinterest.com)** / **[Tumblr](https://tumblr.com)**: 图片分享和轻博客平台。
    *   **[Quora](https://quora.com)**: 高质量的问答社区平台。

*   **即时通讯与社群:**
    *   **[Discord](https://discord.com)**: 深受游戏玩家和各类社群喜爱的语音、视频和文字聊天平台。
    *   **[Telegram](https://telegram.org)**: 注重速度和安全性的加密通讯应用（需在“隐私与安全”中设置两步验证）。
    *   **[Signal](https://signal.org)**: 以端到端加密和隐私保护著称的即时通讯应用。

---

#### **四、 生产力、协作与企业应用**

保护工作文档、项目进度和团队沟通内容。

*   **办公套件与云存储:**
    *   **[Microsoft 365 (Office 365)](https://www.office.com/)**: 微软的办公和云服务，包括Outlook、Word、Excel和OneDrive。
    *   **[Google Workspace (G Suite)](https://workspace.google.com/)**: 包括Gmail, Drive, Docs等，为企业提供协作工具套件。
    *   **[Dropbox](https://www.dropbox.com/)** / **[Box](https://www.box.com/)**: 主流的个人及企业云存储和文件共享服务。
    *   **[Mega.nz](https://mega.nz/)** / **[Sync.com](https://www.sync.com/)**: 注重端到端加密和隐私保护的云存储服务。

*   **团队协作与项目管理:**
    *   **[Slack](https://slack.com)**: 广受欢迎的团队协作和即时沟通工具。
    *   **[Zoom](https://zoom.us/)**: 领先的视频会议和在线协作平台。
    *   **[Asana](https://asana.com/)** / **[Trello](https://trello.com/)** / **[Jira](https://www.atlassian.com/software/jira)** / **[ClickUp](https://clickup.com/)** / **[Monday.com](https://monday.com/)** / **[Basecamp](https://basecamp.com/)** / **[Wrike](https://www.wrike.com/)**: 主流的项目管理与团队协作工具。
    *   **[Confluence](https://www.atlassian.com/software/confluence)** / **[Notion](https://www.notion.so/)**: 强大的团队知识库与多功能一体化笔记工具。
    *   **[Miro](https://miro.com/)** / **[Mural](https://www.mural.co/)**: 用于头脑风暴和团队协作的在线视觉白板工具。
    *   **[Mattermost](https://mattermost.com/)** / **[Rocket.Chat](https://www.rocket.chat/)**: 开源、可自托管的团队聊天工具，是Slack的替代品。

*   **客户关系与支持:**
    *   **[Salesforce](https://www.salesforce.com/)**: 全球领先的客户关系管理(CRM)软件平台。
    *   **[Zendesk](https://www.zendesk.com/)**: 提供客户服务、支持和销售CRM软件的平台。

*   **个人效率工具:**
    *   **[Evernote](https://evernote.com/)**: 经典的笔记和信息组织管理应用。
    *   **[YNAB (You Need A Budget)](https://www.ynab.com/)**: 一款帮助用户进行个人预算和财务管理的工具。

*   **自动化与设计:**
    *   **[Zapier](https://zapier.com/)**: 连接不同Web应用并自动化工作流程的工具。
    *   **[Make](https://www.make.com/)** (原 Integromat): 功能强大的可视化工作流程自动化平台。
    *   **[Adobe Creative Cloud](https://www.adobe.com/creativecloud.html)** / **[Autodesk](https://www.autodesk.com/)** / **[Canva](https://www.canva.com/)**: 领先的创意设计、工程与绘图软件平台。

---

#### **五、 游戏与娱乐平台**

保护您的游戏资产和虚拟物品。

*   **PC游戏平台:**
    *   **[Steam](https://store.steampowered.com/)**: 全球最大的PC游戏数字发行平台，其移动应用内置Steam Guard功能。
    *   **[Epic Games Store](https://www.epicgames.com/store/)**: 热门PC游戏数字商店，常有免费游戏赠送。
    *   **[Battle.net (暴雪战网)](https://www.blizzard.com/battle.net/)**: 暴雪娱乐旗下游戏（如《魔兽世界》、《守望先锋》）的平台。
    *   **[Ubisoft Connect (育碧)](https://ubisoftconnect.com/)**: 育碧旗下游戏（如《刺客信条》、《彩虹六号》）的平台。
    *   **[EA App (艺电)](https://www.ea.com/ea-app)**: 艺电旗下游戏（如《FIFA》、《战地》）的官方PC平台。
    *   **[Riot Games](https://www.riotgames.com/)**: 《英雄联盟》、《无畏契约》等热门游戏的开发商和平台。
    *   **[Rockstar Games (Social Club)](https://socialclub.rockstargames.com/)**: R星游戏（如《GTA 5》、《荒野大镖客2》）的在线服务平台。
    *   **[CD Projekt RED (GOG)](https://www.gog.com/)**: 以提供无DRM保护的游戏而闻名的数字发行平台。

*   **直播与流媒体:**
    *   **[Twitch](https://twitch.tv)**: 全球领先的游戏直播平台，保护主播和观众账户安全至关重要。
    *   **[Netflix](https://www.netflix.com/)** / **[Disney+](https://www.disneyplus.com/)** / **[Hulu](https://www.hulu.com/)** / **[Amazon Prime Video](https://www.primevideo.com/)**: 主流的视频流媒体服务。
    *   **[Spotify](https://open.spotify.com)**: 全球最大的音乐流媒体服务。
    *   **[Patreon](https://patreon.com)** / **[OnlyFans](https://onlyfans.com/)**: 创作者通过订阅模式获得粉丝支持的内容平台。

*   **其他娱乐平台:**
    *   **[Roll20](https://roll20.net/)**: 一款流行的在线桌面角色扮演游戏（TRPG）虚拟桌面平台。
    *   **[FACEIT](https://www.faceit.com/)**: 面向竞技类游戏玩家的电子竞技平台，提供比赛匹配和锦标赛服务。

---

#### **六、 电子商务与零售**

保护您的购物账户、支付信息和订单历史。

*   **综合电商:**
    *   **[Amazon (亚马逊)](https://www.amazon.com/)**: 全球最大的在线零售商。
    *   **[eBay (易趣)](https://www.ebay.com/)**: 全球性的在线拍卖和购物网站。
    *   **[AliExpress](https://www.aliexpress.com/)** / **[Alibaba.com](https://www.alibaba.com/)**: 阿里巴巴集团旗下的国际零售与批发平台。
    *   **[Shopify](https://www.shopify.com/)**: 全球领先的电子商务平台，允许商家创建和运营自己的在线商店。

*   **品牌与零售商:**
    *   **[Walmart](https://www.walmart.com/)** / **[Target](https://www.target.com/)** / **[Best Buy](https://www.bestbuy.com/)** / **[Newegg](https://www.newegg.com/)**: 美国大型综合零售商和电子产品零售商。
    *   **[Nike](https://www.nike.com/)** / **[Adidas](https://www.adidas.com/)** / **[Apple](https://www.apple.com/)** / **[Samsung](https://www.samsung.com/)**: 知名品牌的官方在线商城。

---

#### **七、 其他工具与服务**

*   **密码管理器:**
    *   **[Bitwarden](https://bitwarden.com/)**: 备受推荐的开源密码管理器，其自身账户也支持TOTP保护。
    *   **[1Password](https://1password.com/)**: 功能强大且用户体验优秀的主流密码管理器。
    *   **[LastPass](https://www.lastpass.com/)**: 广泛使用的云端密码管理器。

*   **VPN服务:**
    *   **[NordVPN](https://nordvpn.com/)**, **[ExpressVPN](https://www.expressvpn.com/)**, **[ProtonVPN](https://protonvpn.com/)**, **[Surfshark](https://surfshark.com/)**: 主流的VPN服务商，保护登录账户同样重要。

*   **内容管理系统 (CMS):**
    *   **[WordPress](https://wordpress.org/)**: 可通过安装`Wordfence`, `Google Authenticator`等多种安全插件来启用TOTP。
    *   **[Joomla!](https://www.joomla.org/)** / **[Drupal](https://www.drupal.org/)**: 两款主流的开源内容管理系统，可通过扩展模块支持TOTP。

*   **教育与学术:**
    *   **Blackboard / Canvas / Moodle**: 许多高校和机构使用的在线学习管理系统 (LMS)，具体支持情况取决于学校的配置。
    *   **[ResearchGate](https://www.researchgate.net/)** / **[Academia.edu](https://www.academia.edu/)**: 科研人员进行学术交流与成果分享的社交平台。
    *   **ScienceDirect / IEEE Xplore**: 大型学术出版物和数据库平台，通常通过机构账户登录，其个人账户可能支持。
