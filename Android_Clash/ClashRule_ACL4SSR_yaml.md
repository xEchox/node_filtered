# 自写配置
## 参考链接
clash 配置文件详细解析及示例： https://www.cfmem.com/2021/08/clash.html

规则集： https://github.com/ACL4SSR/ACL4SSR/tree/master/Clash
规则配置： https://github.com/ACL4SSR/ACL4SSR/issues/729

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

# From: https://github.com/ACL4SSR/ACL4SSR/issues/729
rule-providers:
  AbemaTV:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/AbemaTV.yaml"
    path: ./ruleset/AbemaTV.yaml
    interval: 86400

  Adobe:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/Adobe.yaml"
    path: ./ruleset/Adobe.yaml
    interval: 86400

  Amazon:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/Amazon.yaml"
    path: ./ruleset/Amazon.yaml
    interval: 86400

  AmazonIp:
    type: http
    behavior: ipcidr
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/AmazonIp.yaml"
    path: ./ruleset/AmazonIp.yaml
    interval: 86400

  Apple:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/Apple.yaml"
    path: ./ruleset/Apple.yaml
    interval: 86400

#  Bahamut:
#    type: http
#    behavior: classical
#    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/Bahamut.yaml"
#    path: ./ruleset/Bahamut.yaml
#    interval: 86400

  Bilibili:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/Bilibili.yaml"
    path: ./ruleset/Bilibili.yaml
    interval: 86400

  BilibiliHMT:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/BilibiliHMT.yaml"
    path: ./ruleset/BilibiliHMT.yaml
    interval: 86400

  Blizzard:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/Blizzard.yaml"
    path: ./ruleset/Blizzard.yaml
    interval: 86400

  Developer:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/Developer.yaml"
    path: ./ruleset/Developer.yaml
    interval: 86400

  DisneyPlus:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/DisneyPlus.yaml"
    path: ./ruleset/DisneyPlus.yaml
    interval: 86400

  EHGallery:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/EHGallery.yaml"
    path: ./ruleset/EHGallery.yaml
    interval: 86400

  Epic:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/Epic.yaml"
    path: ./ruleset/Epic.yaml
    interval: 86400

  Google:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/Google.yaml"
    path: ./ruleset/Google.yaml
    interval: 86400

#  GoogleCN:
#    type: http
#    behavior: classical
#    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/GoogleCN.yaml"
#    path: ./ruleset/GoogleCN.yaml
#    interval: 86400

  GoogleFCM:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/GoogleFCM.yaml"
    path: ./ruleset/GoogleFCM.yaml
    interval: 86400

  HBO:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/HBO.yaml"
    path: ./ruleset/HBO.yaml
    interval: 86400

  Microsoft:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/Microsoft.yaml"
    path: ./ruleset/Microsoft.yaml
    interval: 86400

  NetEaseMusic:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/NetEaseMusic.yaml"
    path: ./ruleset/NetEaseMusic.yaml
    interval: 86400

  Netflix:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/Netflix.yaml"
    path: ./ruleset/Netflix.yaml
    interval: 86400

  OneDrive:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/OneDrive.yaml"
    path: ./ruleset/OneDrive.yaml
    interval: 86400

  Porn:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/Porn.yaml"
    path: ./ruleset/Porn.yaml
    interval: 86400

  Scholar:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/Scholar.yaml"
    path: ./ruleset/Scholar.yaml
    interval: 86400

  Sony:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/Sony.yaml"
    path: ./ruleset/Sony.yaml
    interval: 86400

  Spotify:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/Spotify.yaml"
    path: ./ruleset/Spotify.yaml
    interval: 86400

  Steam:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/Steam.yaml"
    path: ./ruleset/Steam.yaml
    interval: 86400

  SteamCN:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/SteamCN.yaml"
    path: ./ruleset/SteamCN.yaml
    interval: 86400

  Telegram:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/Telegram.yaml"
    path: ./ruleset/Telegram.yaml
    interval: 86400

  Xbox:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/Xbox.yaml"
    path: ./ruleset/Xbox.yaml
    interval: 86400

  YouTube:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Ruleset/YouTube.yaml"
    path: ./ruleset/YouTube.yaml
    interval: 86400

  BanAD:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/BanAD.yaml"
    path: ./ruleset/BanAD.yaml
    interval: 86400

  BanEasyList:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/BanEasyList.yaml"
    path: ./ruleset/BanEasyList.yaml
    interval: 86400

  BanEasyListChina:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/BanEasyListChina.yaml"
    path: ./ruleset/BanEasyListChina.yaml
    interval: 86400

  BanEasyPrivacy:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/BanEasyPrivacy.yaml"
    path: ./ruleset/BanEasyPrivacy.yaml
    interval: 86400

  BanProgramAD:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/BanProgramAD.yaml"
    path: ./ruleset/BanProgramAD.yaml
    interval: 86400

  ChinaCompanyIp:
    type: http
    behavior: ipcidr
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/ChinaCompanyIp.yaml"
    path: ./ruleset/ChinaCompanyIp.yaml
    interval: 86400

  ChinaDomain:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/ChinaDomain.yaml"
    path: ./ruleset/ChinaDomain.yaml
    interval: 86400

  ChinaIp:
    type: http
    behavior: ipcidr
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/ChinaIp.yaml"
    path: ./ruleset/ChinaIp.yaml
    interval: 86400

  ChinaMedia:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/ChinaMedia.yaml"
    path: ./ruleset/ChinaMedia.yaml
    interval: 86400

  Download:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/Download.yaml"
    path: ./ruleset/Download.yaml
    interval: 86400

  LocalAreaNetwork:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/LocalAreaNetwork.yaml"
    path: ./ruleset/LocalAreaNetwork.yaml
    interval: 86400

  ProxyGFWlist:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/ProxyGFWlist.yaml"
    path: ./ruleset/ProxyGFWlist.yaml
    interval: 86400

  ProxyLite:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/ProxyLite.yaml"
    path: ./ruleset/ProxyLite.yaml
    interval: 86400

  ProxyMedia:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/ProxyMedia.yaml"
    path: ./ruleset/ProxyMedia.yaml
    interval: 86400

  UnBan:
    type: http
    behavior: classical
    url: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/Providers/UnBan.yaml"
    path: ./ruleset/UnBan.yaml
    interval: 86400

rules:
  # 广告
  - RULE-SET,BanAD,🛑 全球拦截
  - RULE-SET,BanEasyList,🛑 全球拦截
  - RULE-SET,BanEasyListChina,🛑 全球拦截
  - RULE-SET,BanEasyPrivacy,🛑 全球拦截
  - RULE-SET,BanProgramAD,🛑 全球拦截
  # 国内
  - RULE-SET,ChinaCompanyIp,🎯 全球直连
  - RULE-SET,ChinaDomain,🎯 全球直连
  - RULE-SET,ChinaIp,🎯 全球直连
  # 本地
  - RULE-SET,Download,🎯 全球直连
  - RULE-SET,LocalAreaNetwork,🎯 全球直连
  # GFW
  - RULE-SET,ProxyGFWlist,🚀 节点选择
  - RULE-SET,ProxyLite,🚀 节点选择
  # 白名单
  - RULE-SET,UnBan,🎯 全球直连
  # 媒体
  - RULE-SET,ChinaMedia,🎯 全球直连
  - RULE-SET,ProxyMedia,🚀 节点选择
  - RULE-SET,AbemaTV,🚀 节点选择
  - RULE-SET,Bilibili,🎯 全球直连
  - RULE-SET,BilibiliHMT,🚀 节点选择
  - RULE-SET,DisneyPlus,🚀 节点选择
  - RULE-SET,HBO,🚀 节点选择
  - RULE-SET,NetEaseMusic,🎯 全球直连
  - RULE-SET,Netflix,🚀 节点选择
  - RULE-SET,Sony,🚀 节点选择
  - RULE-SET,Spotify,🚀 节点选择
  - RULE-SET,YouTube,🚀 节点选择
  # 购物
  - RULE-SET,Amazon,🚀 节点选择
  - RULE-SET,AmazonIp,🚀 节点选择
  - RULE-SET,Apple,🚀 节点选择
  # 游戏
#  - RULE-SET,Bahamut,🚀 节点选择
  - RULE-SET,Blizzard,🎯 全球直连
  - RULE-SET,Epic,🚀 节点选择
  - RULE-SET,Steam,🚀 节点选择
  - RULE-SET,SteamCN,🎯 全球直连
  - RULE-SET,Xbox,🚀 节点选择
  # 开发
  - RULE-SET,Developer,🚀 节点选择
  # 18
  - RULE-SET,EHGallery,🚀 节点选择
  - RULE-SET,Porn,🚀 节点选择
  # 学术
  - RULE-SET,Scholar,🚀 节点选择
  # 其他
  - RULE-SET,Adobe,🚀 节点选择
  - RULE-SET,Google,🚀 节点选择
#  - RULE-SET,GoogleCN,🎯 全球直连
  - RULE-SET,GoogleFCM,🚀 节点选择
  - RULE-SET,Microsoft,🚀 节点选择
  - RULE-SET,OneDrive,🚀 节点选择
  - RULE-SET,Telegram,🚀 节点选择
  # 必须
  - MATCH,🚀 节点选择

```
