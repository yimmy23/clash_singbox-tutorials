# 声明
1. 此方案采用 `RULE-SET` 规则，属高度定制，仅供参考
2. 请根据自身情况进行修改，**适合自己的方案才是最好的方案**，如无特殊需求，可以照搬
3. 此方案适用于 [ShellClash](https://github.com/juewuy/ShellClash)（arm64 架构），以及其它使用 [Clash.Meta 内核](https://github.com/MetaCubeX/Clash.Meta)的平台，如 [Clash Verge](https://github.com/zzzgydi/clash-verge) 等
4. 此方案已摒弃 [AdGuardHome](https://github.com/AdguardTeam/AdGuardHome)，但拦截广告效果依然强劲
# 一、 生成配置文件.yaml 文件直链
具体方法此处不再赘述，请看《[生成带有自定义规则和代理组的配置文件 yaml 直链 ruleset 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E8%A7%84%E5%88%99%E5%92%8C%E4%BB%A3%E7%90%86%E7%BB%84%E7%9A%84%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%20yaml%20%E7%9B%B4%E9%93%BE%20ruleset%20%E6%96%B9%E6%A1%88.md)》，贴一下我使用的配置，此配置参考 [DustinWin/clash-ruleset](https://github.com/DustinWin/clash-ruleset)
- 注：若不使用 TUN 模式，请删除 `tun` 部分

```
proxy-providers:
  🛫 我的机场:
    type: http
    # 修改为你的 Clash 订阅链接
    url: 'https://example.com/xxxxx/clash'
    path: ./proxies/airport.yaml
    interval: 86400
    filter: "VIP|IPV6|中港专线|台湾 IEPL|台湾 IPLC|沪日IEPL|沪日IPLC|韩国 IEPL|新加坡|美国"
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

  - name: 🇹🇼 台湾节点
    type: url-test
    tolerance: 100
    lazy: true
    use:
      - 🛫 我的机场
    filter: "台湾"

  - name: 🇯🇵 日本节点
    type: url-test
    tolerance: 100
    lazy: true
    use:
      - 🛫 我的机场
    filter: "沪日"

  - name: 🇰🇷 韩国节点
    type: url-test
    tolerance: 100
    lazy: true
    use:
      - 🛫 我的机场
    filter: "韩国"

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
    behavior: domain
    url: 'https://cdn.jsdelivr.net/gh/privacy-protection-tools/anti-AD@master/anti-ad-clash.yaml'
    path: ./ruleset/reject.yaml
    interval: 86400

  lan:
    type: http
    behavior: classical
    url: 'https://cdn.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/Lan/Lan.yaml'
    path: ./ruleset/lan.yaml
    interval: 86400

  networktest:
    type: http
    behavior: classical
    url: 'https://fastly.jsdelivr.net/gh/DustinWin/clash-ruleset@release/networktest.yaml'
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
    url: 'https://fastly.jsdelivr.net/gh/DustinWin/clash-ruleset@release/google-cn.yaml'
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
  - RULE-SET,speedtest,📈 网络测试
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
curl -o /tmp/clash.meta-linux-arm64 -L https://cdn.jsdelivr.net/gh/DustinWin/clash-tools@main/Clash.Meta-release/clash.meta-linux-arm64
```
② Alpha 版  
连接 SSH 后运行如下命令：
```
curl -o /tmp/clash.meta-linux-arm64 -L https://cdn.jsdelivr.net/gh/DustinWin/clash-tools@release/clash.meta-linux-arm64
```
## 3. user.yaml
连接 SSH 后运行如下命令：
```
curl -o $clashdir/user.yaml -L https://cdn.jsdelivr.net/gh/DustinWin/clash-ruleset@release/user.yaml
```
## 4. 添加定时任务
连接 SSH 后运行 `crontab -e`，按一下 Ins 键（Insert 键），在最下方粘贴如下内容：
- 注：我更新的是 Clash.Meta 内核 Alpha 版

```
30 3 * * 1,3,5 curl -o /data/clash/clash -L https://fastly.jsdelivr.net/gh/DustinWin/clash-tools@main/Clash.Meta-release/clash.meta-linux-arm64 && chmod +x /data/clash/clash && /data/clash/start.sh restart >/dev/null 2>&1 #每周一、三、五早上 3 点半更新 Clash.Meta 内核
0 4 * * * curl -o /data/clash/user.yaml -L https://fastly.jsdelivr.net/gh/DustinWin/clash-ruleset@release/user.yaml && /data/clash/start.sh restart >/dev/null 2>&1 #每天早上 4 点更新 user.yaml
30 4 * * 2,4,6 /data/clash/start.sh updateyaml && /data/clash/start.sh restart >/dev/null 2>&1 #每周二、四、六早上 4 点半更新订阅并重启 Clash 服务
```
按一下 Esc 键（退出键），输入英文冒号“:”，继续输入“wq”并回车，运行如下命令：
```
/etc/init.d/cron restart
```
# 三、 设置部分
1. 连接 SSH 后运行 `clash` 命令打开 ShellClash 配置脚本  
首次打开会进入新手引导，选择 1 路由设备配置局域网透明代理  
选择 1 在 */data/clash/ui* 目录安装  
根据需要是否选择 1 确认导入配置文件（此处选择 0）  
根据需要是否选择 1 立即启动 clash 服务（此处选择 0）  
输入 0 回车可返回到上级菜单（下同）  
2. 此时脚本会自动“发现可用的内核文件”，选择 1 加载，后选择 3 Clash.Meta 内核
3. 进入主菜单后，选择 9 更新/卸载，进入 7 切换安装源及安装版本，选择 5 公测版&Jsdelivr-CDN 源（推荐）
4. 返回到主菜单，选择 2 clash功能设置，设置如下：
<img src="https://user-images.githubusercontent.com/45238096/231971374-f0b7e674-1e88-4b2e-987a-39667bc5d127.png" width="60%"/>  

特别说明：“5 过滤局域网设备”建议将“过滤方式”切换为“白名单模式”，然后添加需要代理的设备，以此减轻路由器压力  
<img src="https://user-images.githubusercontent.com/45238096/231971027-159c6549-4282-458a-b973-0919739de1f0.png" width="60%"/>  

5. 返回到主菜单，进入 4 clash 启动设置，选择 1 允许 clash 开机启动
6. 返回到主菜单，选择 5 设置定时任务，查看定时任务是否添加成功
<img src="https://user-images.githubusercontent.com/45238096/235452184-3ba22a23-0b3f-4855-95a4-babec8326201.png" width="60%"/>  

7. 返回到主菜单，选择 7 clash 进阶设置，进入 6 配置内置 DNS 服务，设置如下：
<img src="https://user-images.githubusercontent.com/45238096/232890411-b717ddae-1af2-4b20-9829-792f02c3e77e.png" width="60%"/>  

返回到 7 clash 进阶设置，进入 8 手动指定相关端口、秘钥及本机 host，设置如下：  
<img src="https://user-images.githubusercontent.com/45238096/232890495-69f31bc0-360f-468b-89e8-10ec1ae771dc.png" width="60%"/>  

8. 返回到主菜单，选择 6 导入配置文件，进入 2 导入 Clash 配置文件链接，粘贴第一步中生成的配置文件.yaml 文件直链，启动 clash 服务即可  
9. 推荐使用在线面板 [Yacd-meta](https://github.com/MetaCubeX/Yacd-meta)，访问地址：https://yacd.metacubex.one  
① 需要设置该网站“允许不安全内容”，以 Chrome 浏览器为例，进入设置-->隐私和安全-->网站设置-->更多内容设置-->不安全内容（或者直接打开 chrome://settings/content/insecureContent 进行设置），在“允许显示不安全内容”内添加 `https://yacd.metacubex.one`  
<img src="https://user-images.githubusercontent.com/45238096/235448980-52331db5-6b9f-4b0c-a876-1509d34db51a.png" width="60%"/>  

② 首次进入 https://yacd.metacubex.one 需要添加“API Base URL”，输入 `http://192.168.31.1:9999` 并点击“Add”，最后点击下方新增的 http://192.168.31.1:9999 即可访问 Dashboard 面板  
<img src="https://user-images.githubusercontent.com/45238096/235449312-5e046b33-bcd1-4019-a0f7-4f9c224db2c8.png" width="60%"/>
