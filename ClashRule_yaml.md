# 自写配置（有问题，待调整）
## 参考链接
clash 配置文件详细解析及示例： https://www.cfmem.com/2021/08/clash.html

规则集： https://github.com/Loyalsoldier/clash-rules

提供一种parser可以自动替换订阅的规则为自定义规则： https://github.com/Loyalsoldier/clash-rules/issues/27

订阅转换： https://sub.v1.mk/

## 配置代码
```
proxy-groups:
  - name: 🚀 节点选择
    type: select
    proxies:
      - ♻️ 自动选择
      - 🔯 故障转移
      - 🔮 负载均衡
      - DIRECT

  - name: ♻️ 自动选择
    type: url-test
    url: http://www.google.com/generate_204
    interval: 300
    tolerance: 50
    use:
      - ProxyProvidersName1
      - ProxyProvidersName2

  - name: 🔯 故障转移
    type: fallback
    url: http://www.google.com/generate_204
    interval: 180
    use:
      - ProxyProvidersName1
      - ProxyProvidersName2

  - name: 🔮 负载均衡
    type: load-balance
    strategy: consistent-hashing
    url: http://www.google.com/generate_204
    interval: 180
    use:
      - ProxyProvidersName1
      - ProxyProvidersName2

  - name: 🎯 全球直连
    type: select
    proxies:
      - DIRECT
      - 🚀 节点选择

  - name: 🛑 全球拦截
    type: select
    proxies:
      - REJECT
      - DIRECT
      - 🚀 节点选择

  - name: 🐟 漏网之鱼
    type: select
    proxies:
      - ♻️ 自动选择
      - 🔯 故障转移
      - 🔮 负载均衡
      - 🎯 全球直连
      - 🛑 全球拦截

proxy-providers:
# 「url」参数填写订阅链接
#
# 订阅链接可以使用 API进行转换
#
# 1.模式选择「进阶模式」 2.填写订阅链接 3.勾选「仅输出节点信息」 4.「生成订阅链接」
  ProxyProvidersName1:
    type: http
    url: "https://v1.mk/..."
    path: ./Proxy/ProxyProvidersName1.yaml
    interval: 3600
    health-check:
      enable: true
      interval: 600
      url: http://www.google.com/generate_204

  ProxyProvidersName2:
    type: http
    url: "https://v1.mk/..."
    path: ./Proxy/ProxyProvidersName2.yaml
    interval: 3600
    health-check:
      enable: true
      interval: 600
      url: http://www.google.com/generate_204

rule-providers:
  ;GFWList 域名列表
  gfw:
    type: http
    behavior: domain
    url: "https://raw.githubusercontent.com/Loyalsoldier/clash-rules/release/gfw.txt"
    path: ./ruleset/gfw.yaml
    interval: 86400

  ;GreatFire 域名列表
  greatfire:
    type: http
    behavior: domain
    url: "https://raw.githubusercontent.com/Loyalsoldier/clash-rules/release/greatfire.txt"
    path: ./ruleset/greatfire.yaml
    interval: 86400

  ;代理域名列表
  proxy:
    type: http
    behavior: domain
    url: "https://raw.githubusercontent.com/Loyalsoldier/clash-rules/release/proxy.txt"
    path: ./ruleset/proxy.yaml
    interval: 86400

  ;直连域名列表 
  direct:
    type: http
    behavior: domain
    url: "https://raw.githubusercontent.com/Loyalsoldier/clash-rules/release/direct.txt"
    path: ./ruleset/direct.yaml
    interval: 86400

  ;Telegram 使用的 IP 地址列表
  telegramcidr:
    type: http
    behavior: ipcidr
    url: "https://raw.githubusercontent.com/Loyalsoldier/clash-rules/release/telegramcidr.txt"
    path: ./ruleset/telegramcidr.yaml
    interval: 86400

  ;中国大陆 IP 地址列表
  cncidr:
    type: http
    behavior: ipcidr
    url: "https://raw.githubusercontent.com/Loyalsoldier/clash-rules/release/cncidr.txt"
    path: ./ruleset/cncidr.yaml
    interval: 86400

  ;局域网 IP 及保留 IP 地址列表
  lancidr:
    type: http
    behavior: ipcidr
    url: "https://raw.githubusercontent.com/Loyalsoldier/clash-rules/release/lancidr.txt"
    path: ./ruleset/lancidr.yaml
    interval: 86400

  ;需要直连的常见软件列表
  applications:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/Loyalsoldier/clash-rules/release/applications.txt"
    path: ./ruleset/applications.yaml
    interval: 86400

rules:
  - RULE-SET,proxy,♻️ 自动选择
  - RULE-SET,gfw,♻️ 自动选择
  - RULE-SET,GreatFire,♻️ 自动选择
  - RULE-SET,telegramcidr,♻️ 自动选择
  - RULE-SET,direct,DIRECT
  - RULE-SET,lancidr,DIRECT
  - RULE-SET,cncidr,DIRECT
  - RULE-SET,applications,DIRECT
  - DOMAIN,clash.razord.top,DIRECT
  - DOMAIN,yacd.haishan.me,DIRECT
  - GEOIP,LAN,DIRECT
  - GEOIP,CN,DIRECT
  - MATCH,♻️ 自动选择

```
