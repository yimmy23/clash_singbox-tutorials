# [Clash Verge](https://github.com/MetaCubeX/clash-verge)（Windows 端）配置-ruleset 方案
- 注：此方案此方案适用于 [Clash](https://github.com/Dreamacro/clash)，采用 `RULE-SET` 规则搭配 `rule-providers` 配置项
---
# 一、 导入或更新 Clash Meta 内核
以管理员身份运行 CMD，执行如下命令：
```
taskkill /f /t /im "Clash Verge*"
taskkill /f /t /im Clash-Verge*
taskkill /f /t /im clash-meta*
# Clash.Meta 内核 Release 版
curl -o "%LOCALAPPDATA%\Clash Verge\clash-meta.exe" -L https://mirror.ghproxy.com/https://raw.githubusercontent.com/DustinWin/clash_singbox-tools/main/Clash.Meta-release/clash.meta-windows-amd64.exe
# Clash.Meta 内核 Alpha 版
curl -o "%LOCALAPPDATA%\Clash Verge\clash-meta-alpha.exe" -L https://mirror.ghproxy.com/https://raw.githubusercontent.com/DustinWin/clash_singbox-tools/main/Clash.Meta-alpha/clash.meta-windows-amd64.exe
```
# 二、 导入配置
## 1. 导入配置文件
① 进入 Clash Verge->订阅，在“订阅文件链接”处粘贴《[生成带有自定义策略组和规则的 Clash 配置文件直链-ruleset 方案](https://github.com/DustinWin/clash_singbox-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/Clash/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E7%AD%96%E7%95%A5%E7%BB%84%E5%92%8C%E8%A7%84%E5%88%99%E7%9A%84%20Clash%20%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9B%B4%E9%93%BE-ruleset%20%E6%96%B9%E6%A1%88.md)》中生成的配置文件 .yaml 文件直链  
② 右击导入的配置文件，选择“编辑信息”，“更新间隔”设置为“1440”，然后“保存”
## 2. 新建自定义配置
① 进入 Clash Verge->订阅，点击“新建”，类型选择“Merge”，完成后点击“保存”，右击新建的 Merge 文件，选择“启用”  
② 再次右击新建的 Merge 文件，选择“编辑文件”，粘贴如下内容并“保存”：
```
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
  default-nameserver:
    - https://1.12.12.12/dns-query
    - https://223.5.5.5/dns-query
  nameserver:
    - https://dns.alidns.com/dns-query
    - https://doh.pub/dns-query
  nameserver-policy:
    'rule-set:microsoft-cn,apple-cn,google-cn,games-cn': [https://dns.alidns.com/dns-query, https://doh.pub/dns-query]
    'rule-set:cn,private': [https://dns.alidns.com/dns-query, https://doh.pub/dns-query]
    'rule-set:proxy': ['https://dns.google/dns-query#🪜 代理域名', 'https://cloudflare-dns.com/dns-query#🪜 代理域名']
```
# 三、 启动 Clash
1. 进入 Clash Verge->设置->系统设置，点击“服务模式”右边的盾牌图标，点击“INSTALL”，完成后启用“服务模式”
2. 进入设置->Clash 设置->Clash 字段，勾选带有感叹号的字段，“保存”即可
3. 进入设置->系统设置，启用“Tun 模式”
# 五、 在线 Dashboard 面板
推荐使用在线面板 [metacubexd](https://github.com/metacubex/metacubexd)，访问地址：https://d.metacubex.one 
1. 需要设置该网站“允许不安全内容”，以 [Chrome 浏览器](https://www.google.com/chrome)为例，进入设置-->隐私和安全-->网站设置-->更多内容设置-->不安全内容（或者直接打开 `chrome://settings/content/insecureContent` 进行设置），在“允许显示不安全内容”内添加 `https://d.metacubex.one`  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/3d1ed229-1d3a-4ccc-a7b4-adecc8fee8b4" width="60%"/>

2. 首次进入 https://d.metacubex.one 需要添加“后端地址”，输入 `http://192.168.31.1:9090` 并点击“添加”即可访问 Dashboard 面板  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/bb27d6e2-d72b-4a4a-a038-0fd6d085a573" width="60%"/>
