# LunaClashRules

个人维护的 Clash（mihomo）/ Shadowrocket 分流规则。每行一条经典规则（不带策略名），两边通用。

## 规则列表（按建议顺序排列）

顺序有讲究：前面的列表会先命中，与后面列表重叠的域名以前面为准。

| 顺序 | 文件 | 用途 | 建议策略 |
|---|---|---|---|
| 1 | `rules/reject.list` | 广告/追踪 | REJECT |
| 2 | `rules/direct.list` | 必须直连的例外 | DIRECT |
| 3 | `rules/warp.list` | Cloudflare WARP | 香港节点 |
| 4 | `rules/update.list` | Apple / Windows / Office 系统更新与下载 | DIRECT |
| 5 | `rules/openai.list` | OpenAI / ChatGPT / Sora | OpenAI |
| 6 | `rules/claude-risk.list` | Claude 风控相关第三方（须在 payment 前） | Claude |
| 7 | `rules/claude.list` | Anthropic / Claude | Claude |
| 8 | `rules/xai-cursor.list` | Grok / xAI / Cursor（须在 x 前） | Grok |
| 9 | `rules/x.list` | X/Twitter、SpaceX、Starlink、Tesla | X |
| 10 | `rules/youtube.list` | YouTube（须在 google 前） | YouTube |
| 11 | `rules/google.list` | Google / Gemini | Google |
| 12 | `rules/github.list` | GitHub + Copilot（须在 microsoft 前） | GitHub |
| 13 | `rules/apple.list` | Apple / iCloud | Apple（默认直连） |
| 14 | `rules/microsoft.list` | Microsoft / Office / Xbox | Microsoft（默认直连） |
| 15 | `rules/crypto.list` | 交易所、钱包、资讯 | Crypto |
| 16 | `rules/netflix.list` | Netflix | Netflix |
| 17 | `rules/disney.list` | Disney+ | Disney+ |
| 18 | `rules/hbo.list` | HBO Max | HBO |
| 19 | `rules/primevideo.list` | Prime Video | Prime Video |
| 20 | `rules/payment.list` | PayPal / Stripe / Wise 等 | Payment |
| 21 | `rules/telegram.list` | Telegram | Telegram |
| 22 | `rules/discord.list` | Discord | Discord |
| 23 | `rules/proxy.list` | 其他必须代理（HF、Kaggle、WhatsApp） | 默认代理 |

之后可接 MetaCubeX geosite 兜底（google、youtube、netflix、disney、hbo、primevideo、category-cryptocurrency、apple、microsoft），用本地 `geosite.dat`，不额外挂远程规则集。

私人域名（自有域名、机场域名）不要放进本仓库，留在本地扩展脚本里。

## 引用地址

大陆直连实测：`cdn.jsdelivr.net` 与 `raw.githubusercontent.com` 不通，`fastly.jsdelivr.net` 可用，因此统一用 fastly：

    https://fastly.jsdelivr.net/gh/GeraltWang/LunaClashRules@main/rules/openai.list

备用：把域名换成 `testingcf.jsdelivr.net`，路径不变。

jsDelivr 有缓存，改完想立刻生效可以打开
`https://purge.jsdelivr.net/gh/GeraltWang/LunaClashRules@main/rules/openai.list` 刷新。

## mihomo 写法

`proxy: DIRECT` 让规则不依赖节点也能下载；`path` 本地缓存，更新失败时继续用旧文件。

```yaml
rule-providers:
  luna_openai:
    type: http
    behavior: classical
    format: text
    url: https://fastly.jsdelivr.net/gh/GeraltWang/LunaClashRules@main/rules/openai.list
    path: ./ruleset/luna_openai.list
    interval: 86400
    proxy: DIRECT
rules:
  - RULE-SET,luna_openai,OpenAI
```

## Shadowrocket 写法

    RULE-SET,https://fastly.jsdelivr.net/gh/GeraltWang/LunaClashRules@main/rules/openai.list,PROXY
