# 自写配置
## 参考链接
clash 配置文件详细解析及示例： https://www.cfmem.com/2021/08/clash.html

规则集： https://github.com/Loyalsoldier/clash-rules

提供一种parser可以自动替换订阅的规则为自定义规则： https://github.com/Loyalsoldier/clash-rules/issues/27

订阅转换： https://sub.v1.mk/

## 配置代码
```
port: 7890
socks-port: 7891
allow-lan: true
mode: Rule
log-level: info
external-controller: :9090
ipv6: true

proxy-providers:
# 「url」参数填写订阅链接
#
# 订阅链接可以使用 API进行转换， https://sub.v1.mk/
#
# 1.填写订阅链接 2.点击显示「高级功能」 3.勾选「仅输出节点信息」 4.点击「更多选项」勾选「启用UDP」  5.「生成订阅链接」
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

proxy-groups:
  - name: 🚀 节点选择
    type: select
    proxies:
      - ♻️ 自动选择  #感觉会很不稳定，已抛弃
      - 🔯 故障转移  #推荐，连接该组第一个节点，不可用时切换到下一个节点
      - 🔰 ProxyProvidersName1  #手动选择
      - 🔰 ProxyProvidersName2
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

  - name: 🔰 ProxyProvidersName1
    type: select
    use:
      - ProxyProvidersName1

  - name: 🔰 ProxyProvidersName2
    type: select
    use:
      - ProxyProvidersName2

  - name: 🎯 全球直连
    type: select
    proxies:
      - DIRECT
      - 🚀 节点选择

  - name: 🛑 全球拦截
    type: select
    proxies:
      - 🚀 节点选择
      - REJECT
      - DIRECT

  - name: 🐟 漏网之鱼
    type: select
    proxies:
      - 🚀 节点选择
      - 🎯 全球直连
      - 🛑 全球拦截

# https://github.com/Loyalsoldier/clash-rules
rule-providers:
  reject:
    # 广告域名列表
    type: http
    behavior: domain
    url: "https://raw.githubusercontent.com/Loyalsoldier/clash-rules/release/reject.txt"
    path: ./ruleset/reject.yaml
    interval: 86400

  icloud:
    # iCloud 域名列表
    type: http
    behavior: domain
    url: "https://raw.githubusercontent.com/Loyalsoldier/clash-rules/release/icloud.txt"
    path: ./ruleset/icloud.yaml
    interval: 86400

  apple:
    # Apple 在中国大陆可直连的域名列表
    type: http
    behavior: domain
    url: "https://raw.githubusercontent.com/Loyalsoldier/clash-rules/release/apple.txt"
    path: ./ruleset/apple.yaml
    interval: 86400

  google:
    # [慎用]Google 在中国大陆可直连的域名列表
    type: http
    behavior: domain
    url: "https://raw.githubusercontent.com/Loyalsoldier/clash-rules/release/google.txt"
    path: ./ruleset/google.yaml
    interval: 86400

  proxy:
    # 代理域名列表
    type: http
    behavior: domain
    url: "https://raw.githubusercontent.com/Loyalsoldier/clash-rules/release/proxy.txt"
    path: ./ruleset/proxy.yaml
    interval: 86400

  direct:
    # 直连域名列表
    type: http
    behavior: domain
    url: "https://raw.githubusercontent.com/Loyalsoldier/clash-rules/release/direct.txt"
    path: ./ruleset/direct.yaml
    interval: 86400

  private:
    # 私有网络专用域名列表
    type: http
    behavior: domain
    url: "https://raw.githubusercontent.com/Loyalsoldier/clash-rules/release/private.txt"
    path: ./ruleset/private.yaml
    interval: 86400

  gfw:
    # GFWList 域名列表
    type: http
    behavior: domain
    url: "https://raw.githubusercontent.com/Loyalsoldier/clash-rules/release/gfw.txt"
    path: ./ruleset/gfw.yaml
    interval: 86400

  greatfire:
    # GreatFire 域名列表
    type: http
    behavior: domain
    url: "https://raw.githubusercontent.com/Loyalsoldier/clash-rules/release/greatfire.txt"
    path: ./ruleset/greatfire.yaml
    interval: 86400

  tld-not-cn:
    # 非中国大陆使用的顶级域名列表
    type: http
    behavior: domain
    url: "https://raw.githubusercontent.com/Loyalsoldier/clash-rules/release/tld-not-cn.txt"
    path: ./ruleset/tld-not-cn.yaml
    interval: 86400

  telegramcidr:
    # Telegram 使用的 IP 地址列表
    type: http
    behavior: ipcidr
    url: "https://raw.githubusercontent.com/Loyalsoldier/clash-rules/release/telegramcidr.txt"
    path: ./ruleset/telegramcidr.yaml
    interval: 86400

  cncidr:
    # 中国大陆 IP 地址列表
    type: http
    behavior: ipcidr
    url: "https://raw.githubusercontent.com/Loyalsoldier/clash-rules/release/cncidr.txt"
    path: ./ruleset/cncidr.yaml
    interval: 86400

  lancidr:
    # 局域网 IP 及保留 IP 地址列表
    type: http
    behavior: ipcidr
    url: "https://raw.githubusercontent.com/Loyalsoldier/clash-rules/release/lancidr.txt"
    path: ./ruleset/lancidr.yaml
    interval: 86400

  applications:
    # 需要直连的常见软件列表
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/Loyalsoldier/clash-rules/release/applications.txt"
    path: ./ruleset/applications.yaml
    interval: 86400

rules:  # 白名单模式，意为「没有命中规则的网络流量，统统使用代理」
#  - RULE-SET,applications,DIRECT
  - DOMAIN,clash.razord.top,DIRECT
  - DOMAIN,yacd.haishan.me,DIRECT
  - RULE-SET,private,DIRECT
  - RULE-SET,reject,REJECT
  - RULE-SET,icloud,DIRECT
  - RULE-SET,apple,DIRECT
  - RULE-SET,google,DIRECT
  - RULE-SET,proxy,PROXY
  - RULE-SET,direct,DIRECT
  - RULE-SET,lancidr,DIRECT
  - RULE-SET,cncidr,DIRECT
  - RULE-SET,telegramcidr,PROXY
  - GEOIP,LAN,DIRECT
  - GEOIP,CN,DIRECT
  - MATCH,PROXY

rules:  # 黑名单模式，意为「只有命中规则的网络流量，才使用代理」
#  - RULE-SET,applications,DIRECT
  - DOMAIN,clash.razord.top,DIRECT
  - DOMAIN,yacd.haishan.me,DIRECT
  - RULE-SET,private,DIRECT
  - RULE-SET,reject,REJECT
  - RULE-SET,tld-not-cn,PROXY
  - RULE-SET,gfw,PROXY
  - RULE-SET,greatfire,PROXY
  - RULE-SET,telegramcidr,PROXY
  - MATCH,DIRECT

```
