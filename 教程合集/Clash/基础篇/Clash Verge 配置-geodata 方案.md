# [Clash Verge](https://github.com/clash-verge-rev/clash-verge-rev)（Windows 端）配置-geodata 方案
- 注：此方案适用于 [Clash](https://github.com/Dreamacro/clash)，采用 `GEOSITE` 和 `GEOIP` 规则搭配 geosite.dat 和 geoip.dat（或 Country.mmdb） [路由规则文件](https://github.com/MetaCubeX/meta-rules-dat)
---
# 一、 导入或更新 mihomo 内核
以管理员身份运行 CMD，执行如下命令：
```
taskkill /f /t /im "Clash Verge*"
taskkill /f /t /im Clash-Verge*
taskkill /f /t /im clash-meta*
# mihomo 内核 Meta 版
curl -o "%LOCALAPPDATA%\Clash Verge\clash-meta.exe" -L https://mirror.ghproxy.com/https://raw.githubusercontent.com/DustinWin/clash_singbox-tools/main/mihomo-meta/mihomo-windows-amd64.exe
# mihomo 内核 Alpha 版
curl -o "%LOCALAPPDATA%\Clash Verge\clash-meta-alpha.exe" -L https://mirror.ghproxy.com/https://raw.githubusercontent.com/DustinWin/clash_singbox-tools/main/mihomo-alpha/mihomo-windows-amd64.exe
```
# 二、 导入路由规则集文件
以管理员身份运行 CMD 命令提示符，执行如下命令：
```
taskkill /f /t /im "Clash Verge*"
taskkill /f /t /im Clash-Verge*
taskkill /f /t /im clash-meta*
curl -o %APPDATA%\io.github.clash-verge-rev.clash-verge-rev\geosite.dat -L https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geosite.dat
curl -o %APPDATA%\io.github.clash-verge-rev.clash-verge-rev\geoip.dat -L https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geoip.dat
curl -o %APPDATA%\io.github.clash-verge-rev.clash-verge-rev\Country.mmdb -L https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/Country.mmdb
```
# 三、 导入配置
## 1. 导入配置文件
① 进入 Clash Verge->订阅，在“订阅文件链接”处粘贴《[生成带有自定义策略组和规则的 Clash 配置文件直链-geodata 方案](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E7%AD%96%E7%95%A5%E7%BB%84%E5%92%8C%E8%A7%84%E5%88%99%E7%9A%84%20Clash%20%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9B%B4%E9%93%BE-geodata%20%E6%96%B9%E6%A1%88.md)》中生成的配置文件 .yaml 文件直链，然后点击“导入”  
② 配置文件下载完成后，右击导入的配置文件，选择“编辑信息”，“更新间隔”设置为“1440”，然后“保存”  
③ 再次右击导入的配置文件，点击“使用”
## 2. 新建自定义配置
① 进入 Clash Verge->订阅，点击“新建”，类型选择“Merge”，完成后点击“保存”  
② 右击新建的 Merge 文件，选择“编辑文件”，粘贴如下内容并“保存”：
```
mode: rule
log-level: silent
allow-lan: true
unified-delay: false
tcp-concurrent: true
external-controller-tls: 127.0.0.1:9090
find-process-mode: strict
global-client-fingerprint: chrome

geodata-mode: true
geox-url:
  geosite: "https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geosite.dat"
  geoip: "https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geoip.dat"
  mmdb: "https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/Country.mmdb"
geo-auto-update: true
geo-update-interval: 24

sniffer:
  enable: true
  parse-pure-ip: true
  sniff: {HTTP: {ports: [80, 8080-8880]}, TLS: {ports: [443, 8443]}, QUIC: {ports: [443, 8443]}}
  skip-domain: ['Mijia Cloud']

dns:
  enable: true
  prefer-h3: true
  ipv6: true
  listen: 0.0.0.0:1053
  fake-ip-range: 198.18.0.1/16
  enhanced-mode: fake-ip
  fake-ip-filter:
    - "*"
    - "+.lan"
    - "+.local"
  nameserver:
    - 'https://223.5.5.5/dns-queryh3=true'
    - https://1.12.12.12/dns-query
```
③ 再次右击新建的 Merge 文件，点击“启用”
# 四、 启动 Clash
1. 进入 Clash Verge->设置->系统设置->服务模式，点击右边的盾牌图标，然后点击“INSTALL”，完成后启用“服务模式”
2. 进入设置->系统设置->Tun 模式，点击右边的螺帽图标，“Tun 堆栈模式”选择“Mixed”，然后点击“保存”
3. 进入设置->Clash 设置，启用“局域网连接”和“IPv6”
4. 进入设置->Clash 设置->UWP 工具，点击“Exempt All”后再点击“Save Changes”
5. 进入设置->系统设置，启用“Tun 模式”
# 五、 在线 Dashboard 面板
推荐使用在线 Dashboard 面板 [metacubexd](https://github.com/metacubex/metacubexd)，访问地址：https://metacubex.github.io/metacubexd
1. 需要设置该网站“允许不安全内容”，以 Chrome 浏览器为例，进入设置->隐私和安全->网站设置->更多内容设置->不安全内容（或者直接打开 `chrome://settings/content/insecureContent` 进行设置），在“允许显示不安全内容”内添加 `https://metacubex.github.io`  
<img src="https://github.com/DustinWin/clash_singbox-tutorials/assets/45238096/b138cf9f-4b61-44e6-8b3b-0874ddcba8d8" width="60%"/>

2. 首次进入 https://metacubex.github.io/metacubexd 需要添加“后端地址”，输入 `http://192.168.31.1:9090` 并点击“添加”即可访问 Dashboard 面板  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/bb27d6e2-d72b-4a4a-a038-0fd6d085a573" width="60%"/>
