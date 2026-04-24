这是一个为你量身定制的 GitHub 风格 README 模板。我为你增加了**项目简介、清晰的目录结构、Sub-Store 脚本参数解析以及基础的使用指引**，让整个项目看起来更加专业且易于维护。

你可以直接复制以下 Markdown 内容并粘贴到你的 `README.md` 文件中。

***

# 📦 Sing-box Personal Config & Rulesets

本项目包含我个人自用的 [sing-box](https://github.com/SagerNet/sing-box) 客户端配置文件及自定义规则集。

配置按不同设备环境（安卓、软路由、原生核心）进行了分类，并结合 [Sub-Store](https://github.com/sub-store-org/Sub-Store) 脚本实现了节点订阅的自动化过滤与分组。

## 📂 目录结构

```text
.
├── config/                  # 核心配置文件目录
│   ├── mobile.json          # 安卓移动端专属配置 (搭配 GUI 客户端使用)
│   ├── momo.json            # OpenWrt 软路由配置 (专为 OpenWrt-momo 优化)
│   └── core.json            # Sing-box 裸核心通用配置 (CLI 命令行使用)
│
└── rules/                   # 自定义路由规则集 (Source 格式)
    ├── direct.json          # 自定义直连规则 (国内域名/IP、私有网络等)
    ├── proxy.json           # 自定义代理规则 (需强制走代理的域名/IP)
    └── reject.json          # 自定义拦截规则 (广告屏蔽、隐私追踪等)
```

## 🛠️ 依赖工具

为了让配置文件发挥完整作用，强烈建议搭配以下工具使用：
* **核心引擎**: [sing-box](https://github.com/SagerNet/sing-box)
* **订阅管理**: [Sub-Store](https://github.com/sub-store-org/Sub-Store) (用于转换和聚合节点订阅)

## 🔗 Sub-Store 节点处理脚本

本配置依赖 Sub-Store 对原始订阅进行处理。通过使用 xream 提供的 template 脚本，配合正则表达式，实现了**自动剔除无效节点**（如官网、流量提示等）并**按国家/地区自动分组**。

### 脚本链接配置

在 Sub-Store 中新建/编辑订阅时，在 `脚本` 栏填入以下完整的链接配置：

```url
https://raw.githubusercontent.com/xream/scripts/main/surge/modules/sub-store-scripts/sing-box/template.js#name=所有订阅&outbound=🕳ℹ️🐸 手动选择|♻️ 自动选择🏷ℹ️^(?!.*(?:官网|剩余|流量|套餐|免费|订阅|到期时间|全球直连|GB|Expire Date|Traffic|ExpireDate)).*🕳ℹ️🇭🇰 香港自动🏷ℹ️^(?!.*(?:us)).*(🇭🇰|HK|hk|香港|港|HongKong)🕳ℹ️🇹🇼 台湾手动🏷ℹ️^(?!.*(?:us)).*(🇹🇼|TW|tw|台湾|湾|Taiwan)🕳ℹ️🇯🇵 日本手动🏷ℹ️^(?!.*(?:us)).*(🇯🇵|JP|jp|日本|日|Japan)🕳ℹ️🇸🇬 狮城手动🏷ℹ️^(?!.*(?:us)).*(新加坡|坡|狮城|SG|Singapore)🕳ℹ️🇺🇲 美国手动🏷ℹ️^(?!.*(?:AUS|RUS)).*(🇺🇸|US|us|美国|美|United States)&type=组合订阅
```

### 脚本参数解析

上述脚本通过 URL 参数实现了以下出站（Outbound）分组逻辑：
* **全节点池** (`🐸 手动选择` | `♻️ 自动选择`): 自动排除了包含“官网、剩余、流量、到期时间、直连”等非节点干扰项。
* **🇭🇰 香港组**: 匹配含 `HK`, `香港`, `HongKong` 等字眼的节点。
* **🇹🇼 台湾组**: 匹配含 `TW`, `台湾`, `Taiwan` 等字眼的节点。
* **🇯🇵 日本组**: 匹配含 `JP`, `日本`, `Japan` 等字眼的节点。
* **🇸🇬 狮城组**: 匹配含 `SG`, `新加坡`, `Singapore` 等字眼的节点。
* **🇺🇲 美国组**: 匹配含 `US`, `美国` 等字眼，并排除了部分容易混淆的简写（如 AUS, RUS）。

## 🚀 使用方法

1.  **准备节点**：在 Sub-Store 中配置好你的订阅链接，并应用上述的 template 脚本。
2.  **下载配置**：根据你的使用环境（安卓、OpenWrt 或原生核心），下载 `config/` 目录下对应的 `.json` 文件。
3.  **合并订阅**：将 Sub-Store 生成的 `outbounds` 节点列表，注入到你下载的配置文件的 `outbounds` 模块中。
4.  **应用规则**：确保 `rules/` 目录下的规则集文件与配置文件处于正确的相对路径，或者在配置中修改为其对应的在线 Raw 链接。

## 📝 注意事项

* 由于不同机场的节点命名规范不统一，若 Sub-Store 脚本未能正确抓取节点，请根据你实际的节点名称调整脚本链接中的正则表达式。
* `rules/` 文件夹中的规则为自定义补充规则，sing-box 基础的分流能力仍依赖于 `geosite` 和 `geoip` 数据库，请确保客户端能够正常更新这些数据库文件。
