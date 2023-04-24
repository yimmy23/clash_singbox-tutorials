# 声明
1. 此方案属高度定制，仅供参考，请根据自身情况进行修改，**适合自己的方案才是最好的方案**，如无特殊需求，可以照搬
2. 此方案适用于 [ShellClash](https://github.com/juewuy/ShellClash)（arm64 架构），以及其它使用 [Clash.Meta 内核](https://github.com/MetaCubeX/Clash.Meta)的平台，如 [Clash Verge](https://github.com/zzzgydi/clash-verge) 等
3. 此方案已摒弃 [AdGuardHome](https://github.com/AdguardTeam/AdGuardHome)，但拦截广告效果依然强劲
# 一、 生成配置文件.yaml 文件直链
具体方法此处不再赘述，请看《[生成带有自定义规则和代理组的配置文件.yaml 直链](https://github.com/DustinWin/Clash-Tutorials/blob/main/README.md)》，贴一下我使用的配置，此配置参考 [DustinWin/clash-ruleset](https://github.com/DustinWin/clash-ruleset)
- 注：若不使用 TUN 模式，请删除 `tun` 部分

```
proxy-providers:
  🛫 我的机场:
    type: http
    # 修改为你的 Clash 订阅链接
    url: 'https://example.com/xxxxx/clash'
    path: ./proxies/airport.yaml
    interval: 86400
    health-check:
      enable: true
      interval: 600
      url: 'https://www.gstatic.com/generate_204'

unified-delay: false
tcp-concurrent: true
global-client-fingerprint: chrome

profile:
  store-selected: false
  store-fake-ip: true

# 若不使用 TUN 模式，请删除此部分
tun:
  enable: true
  # 若虚拟网卡不支持 system，可以改为 gvisor
  stack: system
  dns-hijack:
    - 'any:53'
  auto-route: true
  auto-detect-interface: true

proxy-groups:
  - name: 🚀 节点选择
    type: select
    proxies:
      - 👑 VIP 节点
      - 🌐 IPv6 节点
      - 🇭🇰 中港专线节点
      - 🇭🇰 香港节点
      - 🇹🇼 台湾节点
      - 🇯🇵 日本节点
      - 🇰🇷 韩国节点
      - 🇸🇬 新加坡节点
      - 🇺🇸 美国节点

  - name: 📈 网络测试
    type: select
    proxies:
      - 🎯 全球直连
      - 🚀 节点选择

  - name: 🐟 漏网之鱼
    type: select
    proxies:
      - 🚀 节点选择
      - 🎯 全球直连

  - name: 🇨🇳 国内域名
    type: select
    proxies:
      - 🎯 全球直连
      - 🚀 节点选择

  - name: 🪜 代理域名
    type: select
    proxies:
      - 🚀 节点选择
      - 🎯 全球直连

  - name: 🎮 国区游戏
    type: select
    proxies:
      - 🎯 全球直连
      - 🚀 节点选择

  - name: Ⓜ️ Microsoft 中国
    type: select
    proxies:
      - 🎯 全球直连
      - 🚀 节点选择

  - name: 🗽 Google 中国
    type: select
    proxies:
      - 🎯 全球直连
      - 🚀 节点选择

  - name: 🍎 Apple 中国
    type: select
    proxies:
      - 🎯 全球直连
      - 🚀 节点选择

  - name: ⛓️ BT 下载
    type: select
    proxies:
      - 🎯 全球直连

  - name: 🏠 私有网络
    type: select
    proxies:
      - 🎯 全球直连

  - name: ⛔️ 广告域名
    type: select
    proxies:
      - 🛑 全球拦截

  - name: 🎯 全球直连
    type: select
    proxies:
      - DIRECT

  - name: 🛑 全球拦截
    type: select
    proxies:
      - REJECT

  - name: 👑 VIP 节点
    type: url-test
    tolerance: 100
    lazy: true
    use:
      - 🛫 我的机场
    filter: "VIP"

  - name: 🌐 IPv6 节点
    type: url-test
    tolerance: 100
    lazy: true
    use:
      - 🛫 我的机场
    filter: "IPV6"

  - name: 🇭🇰 中港专线节点
    type: url-test
    tolerance: 100
    lazy: true
    use:
      - 🛫 我的机场
    filter: "中港专线"

  - name: 🇭🇰 香港节点
    type: url-test
    tolerance: 100
    lazy: true
    use:
      - 🛫 我的机场
    filter: "香港 IEPL"

  - name: 🇹🇼 台湾节点
    type: url-test
    tolerance: 100
    lazy: true
    use:
      - 🛫 我的机场
    filter: "台湾 IEPL|台湾 IPLC"

  - name: 🇯🇵 日本节点
    type: url-test
    tolerance: 100
    lazy: true
    use:
      - 🛫 我的机场
    filter: "沪日IEPL|沪日IPLC"

  - name: 🇰🇷 韩国节点
    type: url-test
    tolerance: 100
    lazy: true
    use:
      - 🛫 我的机场
    filter: "韩国 IEPL"

  - name: 🇸🇬 新加坡节点
    type: url-test
    tolerance: 100
    lazy: true
    use:
      - 🛫 我的机场
    filter: "新加坡"

  - name: 🇺🇸 美国节点
    type: url-test
    tolerance: 100
    lazy: true
    use:
      - 🛫 我的机场
    filter: "美国"

rule-providers:
  reject:
    type: http
    behavior: classical
    url: 'https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/advertising.yaml'
    path: ./ruleset/reject.yaml
    interval: 86400

  lan:
    type: http
    behavior: classical
    url: 'https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/Lan/Lan.yaml'
    path: ./ruleset/lan.yaml
    interval: 86400

  tracker:
    type: http
    behavior: classical
    url: 'https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/tracker.yaml'
    path: ./ruleset/tracker.yaml
    interval: 86400

  networktest:
    type: http
    behavior: classical
    url: 'https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/networktest.yaml'
    path: ./ruleset/networktest.yaml
    interval: 86400

  microsoft-cn:
    type: http
    behavior: domain
    url: 'https://rules.kr328.app/microsoft@cn.yaml'
    path: ./ruleset/microsoft-cn.yaml
    interval: 86400

  apple-cn:
    type: http
    behavior: domain
    url: 'https://rules.kr328.app/apple@cn.yaml'
    path: ./ruleset/apple-cn.yaml
    interval: 86400

  google-cn:
    type: http
    behavior: domain
    url: 'https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/google-cn.yaml'
    path: ./ruleset/google-cn.yaml
    interval: 86400

  games-cn:
    type: http
    behavior: domain
    url: 'https://rules.kr328.app/category-games@cn.yaml'
    path: ./ruleset/games-cn.yaml
    interval: 86400

  proxy:
    type: http
    behavior: classical
    url: 'https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/Proxy/Proxy_Classical.yaml'
    path: ./ruleset/proxy.yaml
    interval: 86400

  direct:
    type: http
    behavior: classical
    url: 'https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/ChinaMax/ChinaMax_Classical.yaml'
    path: ./ruleset/direct.yaml
    interval: 86400

rules:
  - RULE-SET,reject,⛔️ 广告域名
  - RULE-SET,lan,🏠 私有网络
  - RULE-SET,tracker,⛓️ BT 下载
  - RULE-SET,networktest,📈 网络测试
  - RULE-SET,microsoft-cn,Ⓜ️ Microsoft 中国
  - RULE-SET,apple-cn,🍎 Apple 中国
  - RULE-SET,google-cn,🗽 Google 中国
  - RULE-SET,games-cn,🎮 国区游戏
  - RULE-SET,proxy,🪜 代理域名
  - RULE-SET,direct,🇨🇳 国内域名
  - MATCH,🐟 漏网之鱼
```
# 二、 安装和导入
## 1. 安装 ShellClash
连接 SSH 后运行如下命令：
```
curl -o /tmp/ShellClash.tar.gz -L https://cdn.jsdelivr.net/gh/juewuy/ShellClash@master/bin/ShellClash.tar.gz
mkdir -p /tmp/SC_tmp && tar -zxf '/tmp/ShellClash.tar.gz' -C /tmp/SC_tmp/ && source /tmp/SC_tmp/init.sh
```
## 2. 导入 Clash.Meta 内核
① Release 版  
连接 SSH 后运行如下命令：
```
curl -o /tmp/clash.meta-linux-arm64 -L https://cdn.jsdelivr.net/gh/DustinWin/Clash-Files@main/Clash.Meta-Release/clash.meta-linux-arm64
```
② Alpha 版（每天早上 3 点更新）  
连接 SSH 后运行如下命令：
```
curl -o /tmp/clash.meta-linux-arm64 -L https://cdn.jsdelivr.net/gh/DustinWin/Clash-Files@release/clash.meta-linux-arm64
```
## 3. user.yaml
- 注：user.yaml 来源于 [DustinWin/Clash-Files](https://github.com/DustinWin/clash-ruleset)

连接 SSH 后运行如下命令：
```
curl -o $clashdir/user.yaml -L https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/user.yaml
```
## 4. 添加定时任务
连接 SSH 后运行 `crontab -e`，按一下 Ins 键（Insert 键），在最下方粘贴如下内容：
- 注：我更新的是 Clash.Meta 内核 Alpha 版

```
30 3 * * *  curl -o /data/clash/clash -L https://cdn.jsdelivr.net/gh/DustinWin/Clash-Files@release/clash.meta-linux-arm64 && chmod +x /data/clash/clash && /data/clash/start.sh restart >/dev/null 2>&1 #每天早上 3 点半更新 Clash.Meta 内核
0 4 * * * curl -o /data/clash/user.yaml -L https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/user.yaml && /data/clash/start.sh restart >/dev/null 2>&1 #每天早上 4 点更新 user.yaml
30 4 * * 1,3,5 /data/clash/start.sh updateyaml && /data/clash/start.sh restart >/dev/null 2>&1 #每周一、三、五早上 4 点半更新订阅并重启 Clash 服务
```
按一下 Esc 键（退出键），输入英文冒号“:”，继续输入“wq”并回车，运行如下命令：
```
/etc/init.d/cron restart
```
# 三、 设置部分
1. 连接 SSH 后运行 `clash` 命令打开 ShellClash 配置脚本  
首次打开会进入新手引导，选择 1 路由设备配置局域网透明代理  
选择 1 在 */data/clash/ui* 目录安装  
选择 1 确认启用软固化 SSH  
根据需要是否选择 1 确认导入配置文件（此处选择 0）  
根据需要是否选择 1 立即启动 clash 服务（此处选择 0）  
输入 0 回车可返回到上级菜单（下同）  
2. 此时脚本会自动“发现可用的内核文件”，选择 1 加载，后选择 3 Clash.Meta 内核
3. 返回到主菜单，选择 9 更新/卸载，进入 7 切换安装源及安装版本，选择 5 公测版&Jsdelivr-CDN 源（推荐）
4. 返回到 9 更新/卸载，进入 4 安装本地 Dashboard 面板，选择 4 安装 Yacd-Meta 魔改面板
5. 返回到主菜单，选择 2 clash功能设置，设置如下：
<img src="https://user-images.githubusercontent.com/45238096/231971374-f0b7e674-1e88-4b2e-987a-39667bc5d127.png" width="60%"/>  

特别说明：“5 过滤局域网设备”建议将“过滤方式”切换为“白名单模式”，然后添加需要代理的设备，以此减轻路由器压力  
<img src="https://user-images.githubusercontent.com/45238096/231971027-159c6549-4282-458a-b973-0919739de1f0.png" width="60%"/>  

6. 返回到主菜单，选择 4 clash 启动设置，设置如下：
<img src="https://user-images.githubusercontent.com/45238096/231971112-73956e69-ae25-44cb-842e-6f32084de34f.png" width="60%"/>  

7. 返回到主菜单，选择 5 设置定时任务，查看定时任务是否添加成功
<img src="https://user-images.githubusercontent.com/45238096/233792908-db0a368f-1c67-42a0-acdf-8e9fa2becd8a.png" width="60%"/>  

8. 返回到主菜单，选择 7 clash 进阶设置，进入 6 配置内置 DNS 服务，设置如下：
<img src="https://user-images.githubusercontent.com/45238096/232890411-b717ddae-1af2-4b20-9829-792f02c3e77e.png" width="60%"/>  

返回到 7 clash 进阶设置，进入 8 手动指定相关端口、秘钥及本机 host，设置如下：  
<img src="https://user-images.githubusercontent.com/45238096/232890495-69f31bc0-360f-468b-89e8-10ec1ae771dc.png" width="60%"/>  

9. 返回到主菜单，选择 6 导入配置文件，进入 2 导入 Clash 配置文件链接，粘贴第一步中生成的配置文件.yaml 文件直链，启动 clash 服务即可  
面板 Dashboard 打开链接为 http://192.168.31.1:9999/ui
