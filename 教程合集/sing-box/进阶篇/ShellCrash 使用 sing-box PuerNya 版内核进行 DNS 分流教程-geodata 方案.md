# [ShellCrash](https://github.com/juewuy/ShellCrash) 使用 [sing-box PuerNya 版内核](https://github.com/PuerNya/sing-box)进行 DNS 分流教程-geodata 方案
注：
- 1. 此方案适用于 [sing-box](https://github.com/SagerNet/sing-box)，采用 geosite 和 geoip 规则搭配 geosite.db 和 geoip.db [路由规则文件](https://github.com/MetaCubeX/meta-rules-dat)
- 2. 搭配 [AdGuardHome](https://github.com/AdguardTeam/AdGuardHome) 并将 AdGuardHome 作为上游时不要使用该方法
- 3. DNS 分流简单来说就是**指定国内域名走国内 DNS 解析，其它域名包括国外域名走 fakeip**
- 5. 此方案自定义规则参考 [MetaCubeX/meta-rules-dat](https://github.com/MetaCubeX/meta-rules-dat)
- 6. 所有步骤完成后，请连接 SSH 执行命令 `$CRASHDIR/start.sh restart` 后生效
---
# 一、 导入 sing-box PuerNya 版内核
可参考《[ShellCrash 配置-geodata 方案/导入 sing-box PuerNya 版内核](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%9F%BA%E7%A1%80%E7%AF%87/ShellCrash%20%E9%85%8D%E7%BD%AE-geodata%20%E6%96%B9%E6%A1%88.md#%E4%B8%80-%E5%AF%BC%E5%85%A5-sing-box-puernya-%E7%89%88%E5%86%85%E6%A0%B8)》里的步骤进行操作
# 二、 额外编辑配置文件
在《[生成带有自定义策略组和规则的 Clash 配置文件直链-geodata 方案/添加模板和配置文件](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E5%87%BA%E7%AB%99%E5%92%8C%E8%A7%84%E5%88%99%E7%9A%84%20sing-box%20%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9B%B4%E9%93%BE-geodata%20%E6%96%B9%E6%A1%88.md#%E4%BA%8C-%E6%B7%BB%E5%8A%A0%E6%A8%A1%E6%9D%BF%E5%92%8C%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)》编辑 .json 配置文件时，将 `route.rules` 参数里的所有 `geoip` 规则末尾加上 `"skip_resolve": true`，即修改为：
- 注：若遇兼容性问题（如国内游戏无法登录），可删除所有的 `"skip_resolve": true`
```
      { "geoip": [ "telegram" ], "outbound": "📲 电报消息", "skip_resolve": true },
      { "geoip": [ "private" ],  "outbound": "🔒 私有网络", "skip_resolve": true },
      { "geoip": [ "cn" ], "outbound": "🇨🇳 国内 IP", "skip_resolve": true }
```
# 三、 ShellCrash 设置
1. 进入 ShellCrash->7 内核进阶设置->6 配置内置 DNS 服务，将“当前基础 DNS”和“PROXY-DNS”都设置为“null”  
<img src="https://github.com/DustinWin/clash_singbox-tutorials/assets/45238096/4ac7a9ce-2c04-4adc-bbaf-8a1281886f5e" width="60%"/>

2. 其它设置可参考《[ShellCrash 配置-geodata 方案](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/sing-box/%E5%9F%BA%E7%A1%80%E7%AF%87/ShellCrash%20%E9%85%8D%E7%BD%AE-geodata%20%E6%96%B9%E6%A1%88.md)》
# 四、 导入 dns.json 文件
注：
- 1. `dns.rules` 中已添加[常用 fake-ip 地址过滤列表](https://github.com/juewuy/ShellCrash/blob/dev/public/fake_ip_filter.list)（已添加 AdGuardHome 相关域名，包括：adguardteam.github.io、adrules.top、anti-ad.net 和 static.adtidy.org，防止作为下游时检查更新和下载“DNS 黑名单”失败），提高兼容性
- 2. `dns.rules` 中已添加 [TrackersList](https://github.com/XIU2/TrackersListCollection/blob/master/all.txt)（udp 域名），防止 [BT 下载](https://github.com/c0re100/qBittorrent-Enhanced-Edition)无法连接 TrackersList UDP 协议

连接 SSH 后执行如下命令：
```
curl -o $CRASHDIR/jsons/dns.json -L https://cdn.jsdelivr.net/gh/DustinWin/clash_singbox-tutorials@sing-box/ruleset-dns.json && $CRASHDIR/start.sh restart
```
