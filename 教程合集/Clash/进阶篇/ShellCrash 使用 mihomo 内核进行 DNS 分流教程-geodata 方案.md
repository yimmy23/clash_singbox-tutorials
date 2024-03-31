# [ShellCrash](https://github.com/juewuy/ShellCrash) 使用 [mihomo 内核](https://github.com/MetaCubeX/mihomo)进行 DNS 分流教程-geodata 方案
注：
- 1. 此方案适用于 [Clash](https://github.com/Dreamacro/clash)，采用 GEOSITE 和 GEOIP 规则搭配 geosite.dat 和 geoip.dat（或 Country.mmdb）[路由规则文件](https://github.com/MetaCubeX/meta-rules-dat)
- 2. 只有 **DNS 模式选用 `reidir-host`（`fake-ip-filter: ['+.*']` 也算 redir-host 模式）** 时才需要进行 DNS 分流
- 3. 搭配 [AdGuardHome](https://github.com/AdguardTeam/AdGuardHome) 并将 AdGuardHome 作为上游时不要使用该方法
- 4. DNS 分流简单来说就是**指定国内域名走国内 DNS 解析，其余域名走国外 DNS 解析**
- 5. 此方案自定义规则参考 [MetaCubeX/meta-rules-dat](https://github.com/MetaCubeX/meta-rules-dat)
- 6. 所有步骤完成后，请连接 SSH 执行命令 `$CRASHDIR/start.sh restart` 后生效
---
# 一、 导入 mihomo 内核和路由规则文件
可参考《[ShellCrash 配置-geodata 方案](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%9F%BA%E7%A1%80%E7%AF%87/ShellCrash%20%E9%85%8D%E7%BD%AE-geodata%20%E6%96%B9%E6%A1%88.md)》里的步骤《一、二》进行操作
# 二、 额外编辑配置文件
在《[生成带有自定义策略组和规则的 Clash 配置文件直链-geodata 方案/添加模板](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E7%AD%96%E7%95%A5%E7%BB%84%E5%92%8C%E8%A7%84%E5%88%99%E7%9A%84%20Clash%20%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9B%B4%E9%93%BE-geodata%20%E6%96%B9%E6%A1%88.md#%E4%BA%8C-%E6%B7%BB%E5%8A%A0%E6%A8%A1%E6%9D%BF)》编辑 .yaml 配置文件时，将 `rules` 参数里的所有 `GEOIP` 规则末尾加上 `no-resolve`，即修改为：
- 注：若遇兼容性问题（如国内游戏无法登录），可删除所有的 `no-resolve`
```
  - GEOIP,telegram,📲 电报消息,no-resolve
  - GEOIP,private,🔒 私有网络,no-resolve
  - GEOIP,cn,🇨🇳 国内 IP,no-resolve
```
# 三、 ShellCrash 设置
1. 进入 ShellCrash->7 内核进阶设置->6 配置内置 DNS 服务，将“当前基础 DNS”和“PROXY-DNS”都设置为“null”  
<img src="https://github.com/DustinWin/clash_singbox-tutorials/assets/45238096/4ac7a9ce-2c04-4adc-bbaf-8a1281886f5e" width="60%"/>

2. 其它设置可参考《[ShellCrash 配置-geodata 方案](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%9F%BA%E7%A1%80%E7%AF%87/ShellCrash%20%E9%85%8D%E7%BD%AE-geodata%20%E6%96%B9%E6%A1%88.md)》
# 四、 导入 user.yaml 文件
## 1. DNS 模式为 `fake-ip`
- 注：该模式不需要进行 DNS 分流，推荐导入我生成的 fakeip-user.yaml（集成 [fake-ip 地址过滤列表](https://github.com/juewuy/ShellCrash/blob/dev/public/fake_ip_filter.list)，提高了兼容性）

连接 SSH 后执行如下命令：
```
curl -o $CRASHDIR/yamls/user.yaml -L https://cdn.jsdelivr.net/gh/DustinWin/clash_singbox-tutorials@clash/fakeip-user.yaml && $CRASHDIR/start.sh restart
```
## 2. DNS 模式为 `redir-host`
连接 SSH 后执行命令 `vi $CRASHDIR/yamls/user.yaml`，按一下 Ins 键（Insert 键），粘贴如下内容：
```
sniffer:
  enable: true
  parse-pure-ip: true
  sniff: {HTTP: {ports: [80, 8080-8880]}, TLS: {ports: [443, 8443]}, QUIC: {ports: [443, 8443]}}
  skip-domain: [Mijia Cloud]

dns:
  enable: true
  prefer-h3: true
  ipv6: true
  listen: 0.0.0.0:1053
  fake-ip-range: 198.18.0.1/16
  enhanced-mode: fake-ip
  fake-ip-filter: ['+.*']
  nameserver:
    - 'https://223.5.5.5/dns-query#h3=true'
    - https://1.12.12.12/dns-query
  nameserver-policy:
    'geosite:category-ads-all': rcode://refused
    'geosite:microsoft@cn,apple-cn,google-cn,category-games@cn,cn,private': ['https://223.5.5.5/dns-query#h3=true', https://1.12.12.12/dns-query]
    'geosite:geolocation-!cn': ['https://8.8.8.8/dns-query#🪜 代理域名', 'https://1.1.1.1/dns-query#🪜 代理域名']
```
按一下 Esc 键（退出键），输入英文冒号 `:`，继续输入 `wq` 并回车
