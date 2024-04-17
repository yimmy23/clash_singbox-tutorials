# 特别说明：所有 sing-box 相关教程都是基于 sing-box PuerNya 版内核 v1.9.0+ 版本编写
**更新日志（2024-04-17）：**  
注：
- 1. [jsDelivr 源](https://www.jsdelivr.com/github)有延迟（约 12 小时），请耐心等待同步完成，或者使用其它代理加速工具，比如：https://mirror.ghproxy.com
- 2. 近期更新过 [DustinWin/ruleset_geodata/rule_set](https://github.com/DustinWin/ruleset_geodata?tab=readme-ov-file#-sing-box-%E5%86%85%E6%A0%B8) 项目内规则集的用户，请及时更新规则集文件（**必须使用 sing-box PuerNya 版内核 v1.9.0+ 版本**）

1. sing-box 新增规则集 `fakeip-filter`，并在 `dns.rules` 中设置其走国内 DNS 解析，兜底走 fakeip（类似于 [mihomo 内核](https://github.com/MetaCubeX/mihomo)中的 `fake-ip-filter` 设定），提高兼容性
2. sing-box 新增 IPv6 优先设置，使用上述方案时（国内域名走了 fakeip）需要配置（本地网络不支持 IPv6 可忽略）
3. 修改 Clash 和 sing-box 教程，适配新版 ShellCrash 的步骤
4. **修改 Clash 和 sing-box《全网最详细的解锁 SSH ShellCrash 搭配 AdGuardHome 安装和配置教程》，适配新版 ShellCrash 的步骤，并修改了诸多错误和过时内容**
5. `🎮 游戏平台` 改为 `🎮 游戏服务`（此规则集内都是国内能够直连或者国内直连效果更好的域名，所以与 `🍎 苹果服务` 等命名格式保持一致），`🔗 直连域名` 改为 `🇨🇳 直连域名`，`🇨🇳 国内 IP` 改为 `🇨🇳 直连 IP`
6. 其它文案优化

**更新日志（2024-04-04）：**  
1. 修改 sing-box 配置，进阶篇分流设置和分享篇兜底走 `fakeip`（没有命中的域名通通走 `fakeip`，`dns.rules` 中的 `"rule_set": [ "private" ]` 已添加[常用 fake-ip 地址过滤列表](https://github.com/juewuy/ShellClash/blob/dev/public/fake_ip_filter.list)里的所有 AdGuardHome 相关域名，防止作为下游时检查更新和下载“DNS 黑名单”失败；`dns.rules` 中的 `"rule_set": [ "private" ]` 已添加 [TrackersList](https://github.com/XIU2/TrackersListCollection/blob/master/all.txt)，防止 [BT 下载](https://github.com/c0re100/qBittorrent-Enhanced-Edition)无法连接 TrackersList UDP 协议）
2. 修改 Clash 和 sing-box 设置，删除进阶篇分流设置中跳过 DNS 解析的步骤
3. 优化 sing-box for Android 的 `dns.rules` 配置
---
**ShellCrash（fake-ip 模式）搭配 AdGuardHome 的完美方案，现已[出炉](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%88%86%E4%BA%AB%E7%AF%87/%E5%88%86%E4%BA%AB%E8%87%AA%E5%B7%B1%E4%BD%BF%E7%94%A8%20ShellCrash%EF%BC%88fake-ip%20%E6%A8%A1%E5%BC%8F%EF%BC%89%E6%90%AD%E9%85%8D%20AdGuardHome%20%E7%9A%84%E4%B8%80%E5%A5%97%E9%85%8D%E7%BD%AE.md)，强烈推荐！**  
**ShellCrash（mix 模式）搭配 AdGuardHome 的完美方案，现已[出炉](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%88%86%E4%BA%AB%E7%AF%87/%E5%88%86%E4%BA%AB%E8%87%AA%E5%B7%B1%E4%BD%BF%E7%94%A8%20ShellCrash%EF%BC%88mix%20%E6%A8%A1%E5%BC%8F%EF%BC%89%E6%90%AD%E9%85%8D%20AdGuardHome%20%E7%9A%84%E4%B8%80%E5%A5%97%E9%85%8D%E7%BD%AE.md)，强烈推荐！**

# 置顶教程：
## [全网最详细的解锁 SSH ShellCrash 搭配 AdGuardHome 安装和配置教程-Clash 方案](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%85%A8%E7%BD%91%E6%9C%80%E8%AF%A6%E7%BB%86%E7%9A%84%E8%A7%A3%E9%94%81%20SSH%20ShellCrash%20%E6%90%AD%E9%85%8D%20AdGuardHome%20%E5%AE%89%E8%A3%85%E5%92%8C%E9%85%8D%E7%BD%AE%E6%95%99%E7%A8%8B-Clash%20%E6%96%B9%E6%A1%88.md)
## [全网最详细的解锁 SSH ShellCrash 搭配 AdGuardHome 安装和配置教程-sing-box 方案](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%85%A8%E7%BD%91%E6%9C%80%E8%AF%A6%E7%BB%86%E7%9A%84%E8%A7%A3%E9%94%81%20SSH%20ShellCrash%20%E6%90%AD%E9%85%8D%20AdGuardHome%20%E5%AE%89%E8%A3%85%E5%92%8C%E9%85%8D%E7%BD%AE%E6%95%99%E7%A8%8B-sing-box%20%E6%96%B9%E6%A1%88.md)
## [ShellCrash 和 AdGuardHome 快速安装教程](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/ShellCrash%20%E5%92%8C%20AdGuardHome%20%E5%BF%AB%E9%80%9F%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B.md)
---
# 教程合集:
注：
- 1. geodata 方案更适用于路由器等无法判断非本机进程的设备，配置方便简单，对小白用户友好
- 2. ruleset 方案适用于对分流规则要求比较严格的用户，按需配置且配置灵活
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
    <td rowspan="13">sing-box 教程合集</td>
    <td rowspan="5">geodata 方案</td>
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
    <td rowspan="8">rule_set 方案</td>
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
    <td rowspan="3">分享篇</td>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%88%86%E4%BA%AB%E7%AF%87/%E5%88%86%E4%BA%AB%E8%87%AA%E5%B7%B1%E4%BD%BF%E7%94%A8%20ShellCrash%20%E6%90%AD%E9%85%8D%20rule_set%20%E6%96%B9%E6%A1%88%E7%9A%84%E4%B8%80%E5%A5%97%E9%85%8D%E7%BD%AE.md">分享自己使用 ShellCrash 搭配 rule_set 方案的一套配置</a></td>
  </tr>
  <tr>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%88%86%E4%BA%AB%E7%AF%87/%E5%88%86%E4%BA%AB%E8%87%AA%E5%B7%B1%E4%BD%BF%E7%94%A8%20ShellCrash%EF%BC%88mix%20%E6%A8%A1%E5%BC%8F%EF%BC%89%E6%90%AD%E9%85%8D%20AdGuardHome%20%E7%9A%84%E4%B8%80%E5%A5%97%E9%85%8D%E7%BD%AE.md">分享自己使用 ShellCrash（mix 模式）搭配 AdGuardHome 的一套配置</a></td>
  </tr>
  <tr>
    <td><a href="https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%88%86%E4%BA%AB%E7%AF%87/%E5%88%86%E4%BA%AB%E8%87%AA%E5%B7%B1%E4%BD%BF%E7%94%A8%20sing-box%20for%20Android%20%E6%90%AD%E9%85%8D%20rule_set%20%E6%96%B9%E6%A1%88%E7%9A%84%E4%B8%80%E5%A5%97%E9%85%8D%E7%BD%AE.md">分享自己使用 sing-box for Android 搭配 rule_set 方案的一套配置</a></td>
  </tr>
  </tr>
</table>

# 给作者加鸡腿：
## 支付宝
<img src="https://user-images.githubusercontent.com/45238096/219877760-b385af34-ebbd-438e-a31f-cd2b985047bb.png" width=20%/>
