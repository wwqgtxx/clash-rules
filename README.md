## 规则文件地址及使用方式

### 使用方式

要想使用本项目的规则集，只需要在 Clash 配置文件中添加如下 `rule-providers` 和 `rules`。

#### Rule Providers 配置方式

```yaml
rule-providers:
  reject:
    type: http
    format: mrs
    behavior: domain
    url: "https://raw.githubusercontent.com/wwqgtxx/clash-rules/release/reject.mrs"
    path: ./ruleset/reject.mrs
    interval: 86400

  icloud:
    type: http
    format: mrs
    behavior: domain
    url: "https://raw.githubusercontent.com/wwqgtxx/clash-rules/release/icloud.mrs"
    path: ./ruleset/icloud.mrs
    interval: 86400

  apple:
    type: http
    format: mrs
    behavior: domain
    url: "https://raw.githubusercontent.com/wwqgtxx/clash-rules/release/apple.mrs"
    path: ./ruleset/apple.mrs
    interval: 86400

  google:
    type: http
    format: mrs
    behavior: domain
    url: "https://raw.githubusercontent.com/wwqgtxx/clash-rules/release/google.mrs"
    path: ./ruleset/google.mrs
    interval: 86400

  proxy:
    type: http
    format: mrs
    behavior: domain
    url: "https://raw.githubusercontent.com/wwqgtxx/clash-rules/release/proxy.mrs"
    path: ./ruleset/proxy.mrs
    interval: 86400

  direct:
    type: http
    format: mrs
    behavior: domain
    url: "https://raw.githubusercontent.com/wwqgtxx/clash-rules/release/direct.mrs"
    path: ./ruleset/direct.mrs
    interval: 86400

  private:
    type: http
    format: mrs
    behavior: domain
    url: "https://raw.githubusercontent.com/wwqgtxx/clash-rules/release/private.mrs"
    path: ./ruleset/private.mrs
    interval: 86400

  gfw:
    type: http
    format: mrs
    behavior: domain
    url: "https://raw.githubusercontent.com/wwqgtxx/clash-rules/release/gfw.mrs"
    path: ./ruleset/gfw.mrs
    interval: 86400

  tld-not-cn:
    type: http
    format: mrs
    behavior: domain
    url: "https://raw.githubusercontent.com/wwqgtxx/clash-rules/release/tld-not-cn.mrs"
    path: ./ruleset/tld-not-cn.mrs
    interval: 86400

  telegramcidr:
    type: http
    format: mrs
    behavior: ipcidr
    url: "https://raw.githubusercontent.com/wwqgtxx/clash-rules/release/telegramcidr.mrs"
    path: ./ruleset/telegramcidr.mrs
    interval: 86400

  cncidr:
    type: http
    format: mrs
    behavior: ipcidr
    url: "https://raw.githubusercontent.com/wwqgtxx/clash-rules/release/cncidr.mrs"
    path: ./ruleset/cncidr.mrs
    interval: 86400

  lancidr:
    type: http
    format: mrs
    behavior: ipcidr
    url: "https://raw.githubusercontent.com/wwqgtxx/clash-rules/release/lancidr.mrs"
    path: ./ruleset/lancidr.mrs
    interval: 86400

  applications:
    type: http
    format: yaml
    behavior: classical
    url: "https://raw.githubusercontent.com/wwqgtxx/clash-rules/release/applications.yaml"
    path: ./ruleset/applications.yaml
    interval: 86400
```

#### 白名单模式 Rules 配置方式（推荐）

- 白名单模式，意为「**没有命中规则的网络流量，统统使用代理**」，适用于服务器线路网络质量稳定、快速，不缺服务器流量的用户。
- 以下配置中，除了 `DIRECT` 和 `REJECT` 是默认存在于 Clash 中的 policy（路由策略/流量处理策略），其余均为自定义 policy，对应配置文件中 `proxies` 或 `proxy-groups` 中的 `name`。如你直接使用下面的 `rules` 规则，则需要在 `proxies` 或 `proxy-groups` 中手动配置一个 `name` 为 `PROXY` 的 policy。
- 如你希望 Apple、iCloud 和 Google 列表中的域名使用代理，则把 policy 由 `DIRECT` 改为 `PROXY`，以此类推，举一反三。
- 如你不希望进行 DNS 解析，可在 `GEOIP` 规则的最后加上 `,no-resolve`，如 `GEOIP,CN,DIRECT,no-resolve`。

```yaml
rules:
  - RULE-SET,applications,DIRECT
  - DOMAIN,clash.razord.top,DIRECT
  - DOMAIN,yacd.haishan.me,DIRECT
  - RULE-SET,private,DIRECT
  - RULE-SET,reject,REJECT
  - RULE-SET,icloud,DIRECT
  - RULE-SET,apple,DIRECT
  - RULE-SET,google,PROXY
  - RULE-SET,proxy,PROXY
  - RULE-SET,direct,DIRECT
  - RULE-SET,lancidr,DIRECT
  - RULE-SET,cncidr,DIRECT
  - RULE-SET,telegramcidr,PROXY
  - GEOIP,LAN,DIRECT
  - GEOIP,CN,DIRECT
  - MATCH,PROXY
```

#### 黑名单模式 Rules 配置方式

- 黑名单模式，意为「**只有命中规则的网络流量，才使用代理**」，适用于服务器线路网络质量不稳定或不够快，或服务器流量紧缺的用户。通常也是软路由用户、家庭网关用户的常用模式。
- 以下配置中，除了 `DIRECT` 和 `REJECT` 是默认存在于 Clash 中的 policy（路由策略/流量处理策略），其余均为自定义 policy，对应配置文件中 `proxies` 或 `proxy-groups` 中的 `name`。如你直接使用下面的 `rules` 规则，则需要在 `proxies` 或 `proxy-groups` 中手动配置一个 `name` 为 `PROXY` 的 policy。

```yaml
rules:
  - RULE-SET,applications,DIRECT
  - DOMAIN,clash.razord.top,DIRECT
  - DOMAIN,yacd.haishan.me,DIRECT
  - RULE-SET,private,DIRECT
  - RULE-SET,reject,REJECT
  - RULE-SET,tld-not-cn,PROXY
  - RULE-SET,gfw,PROXY
  - RULE-SET,telegramcidr,PROXY
  - MATCH,DIRECT
```

## 致谢

- [@Loyalsoldier/geoip](https://github.com/Loyalsoldier/geoip)
- [@Loyalsoldier/v2ray-rules-dat](https://github.com/Loyalsoldier/v2ray-rules-dat)
- [@gfwlist/gfwlist](https://github.com/gfwlist/gfwlist)
- [@v2fly/domain-list-community](https://github.com/v2fly/domain-list-community)
- [@felixonmars/dnsmasq-china-list](https://github.com/felixonmars/dnsmasq-china-list)
- [@17mon/china_ip_list](https://github.com/17mon/china_ip_list)
