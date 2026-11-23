# LunaClashRules

个人维护的 Clash（mihomo）/ Shadowrocket 分流规则。每行一条经典规则（不带策略名），两边通用。

| 文件 | 用途 | 建议策略 |
|---|---|---|

| `rules/proxy.list` | 必须代理 | 默认代理 |
| `rules/direct.list` | 必须直连的例外 | DIRECT |
| `rules/reject.list` | 广告/追踪 | REJECT |
| `rules/warp.list` | Cloudflare WARP | 香港节点 |

国内直连、私网等大列表继续用 MetaCubeX meta-rules-dat，这里只放自己的例外。
私人域名（自有域名、机场域名）不要放进本仓库，留在本地扩展脚本里。

## 引用地址（jsDelivr）

    https://cdn.jsdelivr.net/gh/GeraltWang/LunaClashRules@main/rules/ai.list

jsDelivr 有缓存，改完想立刻生效可以打开
`https://purge.jsdelivr.net/gh/GeraltWang/LunaClashRules@main/rules/ai.list` 刷新。

## mihomo 写法

```yaml
rule-providers:
  luna_ai:
    type: http
    behavior: classical
    format: text
    url: https://cdn.jsdelivr.net/gh/GeraltWang/LunaClashRules@main/rules/ai.list
    path: ./ruleset/luna_ai.list
    interval: 86400
rules:
  - RULE-SET,luna_ai,AI
```

## Shadowrocket 写法

    RULE-SET,https://cdn.jsdelivr.net/gh/GeraltWang/LunaClashRules@main/rules/ai.list,PROXY
