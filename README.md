# 特别说明：所有 sing-box 相关教程都是基于 sing-box PuerNya 版内核，请尽量使用[最新版](https://github.com/DustinWin/clash_singbox-tools/tree/main/sing-box-puernya)
**更新日志（2024-07-13）：** 
1. 修改 Clash 教程，适配新版 Clash Verge，精简流程
2. 修改 Clash 教程，优化 `dns` 配置，全部使用纯域名 DNS 服务器
3. 修改 Clash 教程，优化 `dns.servers` 配置，启用 HTTP/3 特性或 QUIC 协议
4. 修改 Clash 分享篇，添加 `hosts` 配置项，将 `miwifi.com` 指向 `192.168.31.1`
5. 修改 sing-box 分享篇，添加 `dns.hosts` 配置项
6. 修改 sing-box 基础篇，添加 `dns.lazy_cache` 和 `route.concurrent_dial` 配置项
7. 修改 sing-box 教程，优化 `dns.servers` 配置，启用 HTTP/3 特性，
8. 优化 `dns.servers` 配置，启用 HTTP/3 特性
9. 修改 sing-box 分享篇，修改 `dns.final` 为 `dns_direct`
10. 修改 sing-box 分享篇，修改 `dns.rules` 为全 fakeip 模式
11. 修改 sing-box 基础篇，删除添加 dns.json 配置文件的步骤
12. 修改 sing-box 进阶篇，优化 `dns` 分流，引入 `dns.fakeip.exclude_rule`，提高兼容
13. 《ShellCrash 和 AdGuardHome 快速安装教程》新增“安装 Clash dashboard 面板”的步骤
14. 其它步骤和文案优化

**更新日志（2024-07-05）：** 
1. 优化了 `dns` 设置，更加合理（**强烈推荐使用最新方案，sing-box 防止 dns 泄露已更新**）
2. 部分教程增加使用在线 Dashboard 面板的步骤
3. 优化《全网最详细的解锁 SSH ShellCrash 搭配 AdGuardHome 安装和配置教程》里的 AdGuardHome DNS 设置
4. 修改 sing-box 分享篇中的《分享自己使用 ShellCrash（fakeip 模式）搭配 AdGuardHome 的一套配置》，由 `geodata` 方案改为 `rule_set` 方案
5. 修改 sing-box 分享篇中的《分享自己使用 sing-box for Android 搭配 rule_set 方案的一套配置》，删除 `outbound_providers.path` 和 `rule_set.path` 中的文件夹路径，提高兼容性
6. 修改 sing-box 分享篇，再次优化 `dns` 配置，**防止 DNS 泄露（可进入 https://ipleak.net 测试），同时也可保证兼容性**
7. 修改 sing-box 分享篇，修改 `dns.rules` 中的 `rewrite_ttl` 为 `0`，添加 `route.concurrent_dial` 配置项
8. 修改 sing-box 分享篇，`dns` 添加 `lazy_cache` 配置项
9. 修改 sing-box 分享篇，`dns.fallback_rules` 修改为混合模式
10. 修改 sing-box 分享篇，调整需要走 realip 的域名到 `dns.fakeip.exclude_rule` 中
11. 修改 sing-box 基础篇，优化“黑名单模式”，可以完美运行了
12. 修改 sing-box，`dns` 配置再次进行了优化，推荐使用最新方案（须更新到 sing-box PuerNya 版内核最新版本）
13. 修改 sing-box 分享篇，`dns.rules` 迁移 `stop_fallthrough` 到 `allow_fallthrough`
14. 其它文案优化
---
**ShellCrash（fake-ip 模式）搭配 AdGuardHome 的完美方案（Clash），现已[出炉](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%88%86%E4%BA%AB%E7%AF%87/%E5%88%86%E4%BA%AB%E8%87%AA%E5%B7%B1%E4%BD%BF%E7%94%A8%20ShellCrash%EF%BC%88fake-ip%20%E6%A8%A1%E5%BC%8F%EF%BC%89%E6%90%AD%E9%85%8D%20AdGuardHome%20%E7%9A%84%E4%B8%80%E5%A5%97%E9%85%8D%E7%BD%AE.md)，强烈推荐！**  
**ShellCrash（fakeip 模式）搭配 AdGuardHome 的完美方案（sing-box），现已[出炉](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%88%86%E4%BA%AB%E7%AF%87/%E5%88%86%E4%BA%AB%E8%87%AA%E5%B7%B1%E4%BD%BF%E7%94%A8%20ShellCrash%EF%BC%88fakeip%20%E6%A8%A1%E5%BC%8F%EF%BC%89%E6%90%AD%E9%85%8D%20AdGuardHome%20%E7%9A%84%E4%B8%80%E5%A5%97%E9%85%8D%E7%BD%AE.md)，强烈推荐！**

# 置顶教程：
## [全网最详细的解锁 SSH ShellCrash 搭配 AdGuardHome 安装和配置教程-Clash 方案](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%85%A8%E7%BD%91%E6%9C%80%E8%AF%A6%E7%BB%86%E7%9A%84%E8%A7%A3%E9%94%81%20SSH%20ShellCrash%20%E6%90%AD%E9%85%8D%20AdGuardHome%20%E5%AE%89%E8%A3%85%E5%92%8C%E9%85%8D%E7%BD%AE%E6%95%99%E7%A8%8B-Clash%20%E6%96%B9%E6%A1%88.md)
## [全网最详细的解锁 SSH ShellCrash 搭配 AdGuardHome 安装和配置教程-sing-box 方案](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%85%A8%E7%BD%91%E6%9C%80%E8%AF%A6%E7%BB%86%E7%9A%84%E8%A7%A3%E9%94%81%20SSH%20ShellCrash%20%E6%90%AD%E9%85%8D%20AdGuardHome%20%E5%AE%89%E8%A3%85%E5%92%8C%E9%85%8D%E7%BD%AE%E6%95%99%E7%A8%8B-sing-box%20%E6%96%B9%E6%A1%88.md)
## [ShellCrash 和 AdGuardHome 快速安装教程](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/ShellCrash%20%E5%92%8C%20AdGuardHome%20%E5%BF%AB%E9%80%9F%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B.md)
---
# 教程合集:
注：
- 1. geodata 方案更适用于路由器等设备（连接多台设备而无法判断设备进程，仅推荐路由器搭载 mihomo 内核时使用），配置文件编写简单，对小白用户友好
- 2. ruleset 方案适用于对分流规则要求比较严格的用户，配置文件编写复杂，可按需配置且配置灵活
- 3. [Clash](https://github.com/Dreamacro/clash) 搭配 geodata 方案采用 `GEOSITE` 和 `GEOIP` 规则搭配 geosite.dat 和 geoip.dat（或 Country.mmdb） 路由规则文件
- 4. Clash 搭配 rule-set 方案采用 `RULE-SET` 规则搭配 `rule-providers` 配置项
- 5. [sing-box](https://github.com/SagerNet/sing-box) 搭配 geodata 方案采用 `GEOSITE` 和 `GEOIP` 规则搭配 geosite.db 和 geoip.db 路由规则文件
- 6. sing-box 搭配 rule_set 方案采用 `rule_set` 规则搭配 `rule_set` 配置项

<table>
  <tr>
    <td rowspan="18">Clash 教程合集</td>
    <td rowspan="9">geodata 方案</td>
    <td rowspan="3">基础篇</td>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E7%AD%96%E7%95%A5%E7%BB%84%E5%92%8C%E8%A7%84%E5%88%99%E7%9A%84%20Clash%20%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9B%B4%E9%93%BE-geodata%20%E6%96%B9%E6%A1%88.md">生成带有自定义策略组和规则的 Clash 配置文件直链-geodata 方案</a></td>
  </tr>
  <tr>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%9F%BA%E7%A1%80%E7%AF%87/ShellCrash%20%E9%85%8D%E7%BD%AE-geodata%20%E6%96%B9%E6%A1%88.md">ShellCrash 配置-geodata 方案</a></td>
  </tr>
  <tr>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%9F%BA%E7%A1%80%E7%AF%87/Clash%20Verge%20%E9%85%8D%E7%BD%AE-geodata%20%E6%96%B9%E6%A1%88.md">Clash Verge 配置-geodata 方案</a></td>
  </tr>
  <tr>
    <td rowspan="4">进阶篇</td>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E8%BF%9B%E9%98%B6%E7%AF%87/ShellCrash%20%E6%9C%AC%E5%9C%B0%E9%85%8D%E7%BD%AE%E8%87%AA%E5%AE%9A%E4%B9%89%E7%AD%96%E7%95%A5%E7%BB%84%E5%92%8C%E8%A7%84%E5%88%99-geodata%20%E6%96%B9%E6%A1%88.md">ShellCrash 本地配置自定义策略组和规则-geodata 方案</a></td>
  </tr>
  <tr>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E8%BF%9B%E9%98%B6%E7%AF%87/ShellCrash%20%E4%BD%BF%E7%94%A8%20mihomo%20%E5%86%85%E6%A0%B8%E8%BF%9B%E8%A1%8C%20DNS%20%E5%88%86%E6%B5%81%E6%95%99%E7%A8%8B-geodata%20%E6%96%B9%E6%A1%88.md">ShellCrash 使用 mihomo 内核进行 DNS 分流教程-geodata 方案</a></td>
  </tr>
  <tr>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E8%BF%9B%E9%98%B6%E7%AF%87/Clash%20Verge%20%E4%BD%BF%E7%94%A8%20mihomo%20%E5%86%85%E6%A0%B8%E8%BF%9B%E8%A1%8C%20DNS%20%E5%88%86%E6%B5%81%E6%95%99%E7%A8%8B-geodata%20%E6%96%B9%E6%A1%88.md">Clash Verge 使用 mihomo 内核进行 DNS 分流教程-geodata 方案</a></td>
  <tr>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E8%BF%9B%E9%98%B6%E7%AF%87/mihomo%20%E5%86%85%E6%A0%B8%E9%85%8D%E7%BD%AE%20DNS%20%E4%B8%8D%E6%B3%84%E9%9C%B2%E6%95%99%E7%A8%8B-geodata%20%E6%96%B9%E6%A1%88.md">mihomo 内核配置 DNS 不泄露教程-geodata 方案</a></td>
  </tr>
  <tr>
    <td rowspan="2">分享篇</td>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%88%86%E4%BA%AB%E7%AF%87/%E5%88%86%E4%BA%AB%E8%87%AA%E5%B7%B1%E4%BD%BF%E7%94%A8%20ShellCrash%20%E6%90%AD%E9%85%8D%20geodata%20%E6%96%B9%E6%A1%88%E7%9A%84%E4%B8%80%E5%A5%97%E9%85%8D%E7%BD%AE.md">分享自己使用 ShellCrash 搭配 geodata 方案的一套配置</a></td>
  </tr>
  <tr>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%88%86%E4%BA%AB%E7%AF%87/%E5%88%86%E4%BA%AB%E8%87%AA%E5%B7%B1%E4%BD%BF%E7%94%A8%20ShellCrash%EF%BC%88fake-ip%20%E6%A8%A1%E5%BC%8F%EF%BC%89%E6%90%AD%E9%85%8D%20AdGuardHome%20%E7%9A%84%E4%B8%80%E5%A5%97%E9%85%8D%E7%BD%AE.md">分享自己使用 ShellCrash（fake-ip 模式）搭配 AdGuardHome 的一套配置</a></td>
  </tr>
  <tr>
    <td rowspan="9">rule-set 方案</td>
    <td rowspan="3">基础篇</td>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E7%AD%96%E7%95%A5%E7%BB%84%E5%92%8C%E8%A7%84%E5%88%99%E7%9A%84%20Clash%20%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9B%B4%E9%93%BE-ruleset%20%E6%96%B9%E6%A1%88.md">生成带有自定义策略组和规则的 Clash 配置文件直链-ruleset 方案</a></td>
  </tr>
  <tr>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%9F%BA%E7%A1%80%E7%AF%87/ShellCrash%20%E9%85%8D%E7%BD%AE-ruleset%20%E6%96%B9%E6%A1%88.md">ShellCrash 配置-ruleset 方案</a></td>
  </tr>
  <tr>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%9F%BA%E7%A1%80%E7%AF%87/Clash%20Verge%20%E9%85%8D%E7%BD%AE-ruleset%20%E6%96%B9%E6%A1%88.md">Clash Verge 配置-ruleset 方案</a></td>
  </tr>
  <tr>
    <td rowspan="4">进阶篇</td>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E8%BF%9B%E9%98%B6%E7%AF%87/ShellCrash%20%E6%9C%AC%E5%9C%B0%E9%85%8D%E7%BD%AE%E8%87%AA%E5%AE%9A%E4%B9%89%E7%AD%96%E7%95%A5%E7%BB%84%E5%92%8C%E8%A7%84%E5%88%99-ruleset%20%E6%96%B9%E6%A1%88.md">ShellCrash 本地配置自定义策略组和规则-ruleset 方案</a></td>
  </tr>
  <tr>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E8%BF%9B%E9%98%B6%E7%AF%87/ShellCrash%20%E4%BD%BF%E7%94%A8%20mihomo%20%E5%86%85%E6%A0%B8%E8%BF%9B%E8%A1%8C%20DNS%20%E5%88%86%E6%B5%81%E6%95%99%E7%A8%8B-ruleset%20%E6%96%B9%E6%A1%88.md">ShellCrash 使用 mihomo 内核进行 DNS 分流教程-ruleset 方案</a></td>
  </tr>
  <tr>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E8%BF%9B%E9%98%B6%E7%AF%87/Clash%20Verge%20%E4%BD%BF%E7%94%A8%20mihomo%20%E5%86%85%E6%A0%B8%E8%BF%9B%E8%A1%8C%20DNS%20%E5%88%86%E6%B5%81%E6%95%99%E7%A8%8B-ruleset%20%E6%96%B9%E6%A1%88.md">Clash Verge 使用 mihomo 内核进行 DNS 分流教程-ruleset 方案</a></td>
  </tr>
  <tr>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E8%BF%9B%E9%98%B6%E7%AF%87/mihomo%20%E5%86%85%E6%A0%B8%E9%85%8D%E7%BD%AE%20DNS%20%E4%B8%8D%E6%B3%84%E9%9C%B2%E6%95%99%E7%A8%8B-ruleset%20%E6%96%B9%E6%A1%88.md">mihomo 内核配置 DNS 不泄露教程-ruleset 方案</a></td>
  </tr>
  <tr>
    <td rowspan="2">分享篇</td>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%88%86%E4%BA%AB%E7%AF%87/%E5%88%86%E4%BA%AB%E8%87%AA%E5%B7%B1%E4%BD%BF%E7%94%A8%20Clash%20Verge%20%E6%90%AD%E9%85%8D%20rule-set%20%E6%96%B9%E6%A1%88%E7%9A%84%E4%B8%80%E5%A5%97%E9%85%8D%E7%BD%AE.md">分享自己使用 Clash Verge 搭配 ruleset 方案的一套配置</a></td>
  </tr>
  <tr>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%88%86%E4%BA%AB%E7%AF%87/%E5%88%86%E4%BA%AB%E8%87%AA%E5%B7%B1%E4%BD%BF%E7%94%A8%20Clash.Meta%20for%20Android%20%E6%90%AD%E9%85%8D%20rule-set%20%E6%96%B9%E6%A1%88%E7%9A%84%E4%B8%80%E5%A5%97%E9%85%8D%E7%BD%AE.md">分享自己使用 Clash.Meta for Android 搭配 rule-set 方案的一套配置</a></td>
  </tr>
  </tr>
    <tr>
    <td rowspan="15">sing-box 教程合集</td>
    <td rowspan="6">geodata 方案</td>
    <td rowspan="2">基础篇</td>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E5%87%BA%E7%AB%99%E5%92%8C%E8%A7%84%E5%88%99%E7%9A%84%20sing-box%20%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9B%B4%E9%93%BE-geodata%20%E6%96%B9%E6%A1%88.md">生成带有自定义出站和规则的 sing-box 配置文件直链-geodata 方案</a></td>
  </tr>
  <tr>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%9F%BA%E7%A1%80%E7%AF%87/ShellCrash%20%E9%85%8D%E7%BD%AE-geodata%20%E6%96%B9%E6%A1%88.md">ShellCrash 配置-geodata 方案</a></td>
  </tr>
  <tr>
    <td rowspan="3">进阶篇</td>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E8%BF%9B%E9%98%B6%E7%AF%87/ShellCrash%20%E6%9C%AC%E5%9C%B0%E9%85%8D%E7%BD%AE%E8%87%AA%E5%AE%9A%E4%B9%89%E5%87%BA%E7%AB%99%E5%92%8C%E8%A7%84%E5%88%99-geodata%20%E6%96%B9%E6%A1%88.md">ShellCrash 本地配置自定义出站和规则-geodata 方案</a></td>
  </tr>
  <tr>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E8%BF%9B%E9%98%B6%E7%AF%87/ShellCrash%20%E4%BD%BF%E7%94%A8%20sing-box%20PuerNya%20%E7%89%88%E5%86%85%E6%A0%B8%E8%BF%9B%E8%A1%8C%20DNS%20%E5%88%86%E6%B5%81%E6%95%99%E7%A8%8B-geodata%20%E6%96%B9%E6%A1%88.md">ShellCrash 使用 sing-box PuerNya 版内核进行 DNS 分流教程-geodata 方案</a></td>
  </tr>
  <tr>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E8%BF%9B%E9%98%B6%E7%AF%87/sing-box%20PuerNya%20%E7%89%88%E5%86%85%E6%A0%B8%E9%85%8D%E7%BD%AE%20DNS%20%E4%B8%8D%E6%B3%84%E9%9C%B2%E6%95%99%E7%A8%8B-geodata%20%E6%96%B9%E6%A1%88.md">sing-box PuerNya 版内核配置 DNS 不泄露教程-geodata 方案</a></td>
  </tr>
  <tr>
    <td>分享篇</td>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%88%86%E4%BA%AB%E7%AF%87/%E5%88%86%E4%BA%AB%E8%87%AA%E5%B7%B1%E4%BD%BF%E7%94%A8%20ShellCrash%20%E6%90%AD%E9%85%8D%20geodata%20%E6%96%B9%E6%A1%88%E7%9A%84%E4%B8%80%E5%A5%97%E9%85%8D%E7%BD%AE.md">分享自己使用 ShellCrash 搭配 geodata 方案的一套配置</a></td>
  </tr>
  <tr>
    <td rowspan="9">rule_set 方案</td>
    <td rowspan="2">基础篇</td>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E5%87%BA%E7%AB%99%E5%92%8C%E8%A7%84%E5%88%99%E7%9A%84%20sing-box%20%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9B%B4%E9%93%BE-ruleset%20%E6%96%B9%E6%A1%88.md">生成带有自定义出站和规则的 sing-box 配置文件直链-ruleset 方案</a></td>
  </tr>
  <tr>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%9F%BA%E7%A1%80%E7%AF%87/ShellCrash%20%E9%85%8D%E7%BD%AE-ruleset%20%E6%96%B9%E6%A1%88.md">ShellCrash 配置-ruleset 方案</a></td>
  </tr>
  <tr>
    <td rowspan="3">进阶篇</td>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E8%BF%9B%E9%98%B6%E7%AF%87/ShellCrash%20%E6%9C%AC%E5%9C%B0%E9%85%8D%E7%BD%AE%E8%87%AA%E5%AE%9A%E4%B9%89%E5%87%BA%E7%AB%99%E5%92%8C%E8%A7%84%E5%88%99-ruleset%20%E6%96%B9%E6%A1%88.md">ShellCrash 本地配置自定义出站和规则-ruleset 方案</a></td>
  </tr>
  <tr>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E8%BF%9B%E9%98%B6%E7%AF%87/ShellCrash%20%E4%BD%BF%E7%94%A8%20sing-box%20PuerNya%20%E7%89%88%E5%86%85%E6%A0%B8%E8%BF%9B%E8%A1%8C%20DNS%20%E5%88%86%E6%B5%81%E6%95%99%E7%A8%8B-ruleset%E6%96%B9%E6%A1%88.md">ShellCrash 使用 sing-box PuerNya 版内核进行 DNS 分流教程-ruleset方案</a></td>
  </tr>
  <tr>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E8%BF%9B%E9%98%B6%E7%AF%87/sing-box%20PuerNya%20%E7%89%88%E5%86%85%E6%A0%B8%E9%85%8D%E7%BD%AE%20DNS%20%E4%B8%8D%E6%B3%84%E9%9C%B2%E6%95%99%E7%A8%8B-ruleset%20%E6%96%B9%E6%A1%88.md">sing-box PuerNya 版内核配置 DNS 不泄露教程-ruleset 方案</a></td>
  </tr>
  <tr>
    <td rowspan="4">分享篇</td>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%88%86%E4%BA%AB%E7%AF%87/%E5%88%86%E4%BA%AB%E8%87%AA%E5%B7%B1%E4%BD%BF%E7%94%A8%20ShellCrash%20%E6%90%AD%E9%85%8D%20rule_set%20%E6%96%B9%E6%A1%88%E7%9A%84%E4%B8%80%E5%A5%97%E9%85%8D%E7%BD%AE.md">分享自己使用 ShellCrash 搭配 rule_set 方案的一套配置</a></td>
  </tr>
  <tr>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%88%86%E4%BA%AB%E7%AF%87/%E5%88%86%E4%BA%AB%E8%87%AA%E5%B7%B1%E4%BD%BF%E7%94%A8%20ShellCrash%EF%BC%88fakeip%20%E6%A8%A1%E5%BC%8F%EF%BC%89%E6%90%AD%E9%85%8D%20AdGuardHome%20%E7%9A%84%E4%B8%80%E5%A5%97%E9%85%8D%E7%BD%AE.md">分享自己使用 ShellCrash（fakeip 模式）搭配 AdGuardHome 的一套配置</a></td>
  </tr>
  <tr>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%88%86%E4%BA%AB%E7%AF%87/%E5%88%86%E4%BA%AB%E8%87%AA%E5%B7%B1%E4%BD%BF%E7%94%A8%20sing-box%20for%20Windows%EF%BC%88%E8%A3%B8%E6%A0%B8%EF%BC%89%E6%90%AD%E9%85%8D%20rule_set%20%E6%96%B9%E6%A1%88%E7%9A%84%E4%B8%80%E5%A5%97%E9%85%8D%E7%BD%AE.md">分享自己使用 sing-box for Windows（裸核）搭配 rule_set 方案的一套配置</a></td>
  </tr>
  <tr>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%88%86%E4%BA%AB%E7%AF%87/%E5%88%86%E4%BA%AB%E8%87%AA%E5%B7%B1%E4%BD%BF%E7%94%A8%20sing-box%20for%20Android%20%E6%90%AD%E9%85%8D%20rule_set%20%E6%96%B9%E6%A1%88%E7%9A%84%E4%B8%80%E5%A5%97%E9%85%8D%E7%BD%AE.md">分享自己使用 sing-box for Android 搭配 rule_set 方案的一套配置</a></td>
  </tr>
  </tr>
</table>

# 给作者加鸡腿：
## 支付宝
<img src="https://user-images.githubusercontent.com/45238096/219877760-b385af34-ebbd-438e-a31f-cd2b985047bb.png" width=20%/>
