# 分享自己使用 [ShellClash](https://github.com/juewuy/ShellClash) 搭配 [AdGuardHome](https://github.com/AdguardTeam/AdGuardHome) 的一套配置
# 声明：
1. 此方案采用 ShellClash 作为上游，AdGuardHome 作为下游的模式
2. 此方案采用 GEOSITE 和 GEOIP 规则搭配 geosite.dat 和 geoip.dat（或 Country.mmdb）[路由规则文件](https://github.com/DustinWin/clash-geosite)，属高度定制，仅供参考
3. 规则参考 [DustinWin/clash-geosite](https://github.com/DustinWin/clash-geosite)
4. 请根据自身情况进行修改，适合自己的方案才是最好的方案，如无特殊需求，可以照搬
5. 此方案中 ShellClash 采用的 DNS 模式为 fake-ip（仍与 AdGuardHome 配合完美）
6. 此方案适用于 ShellClash（以 arm64 架构为例）
7. 此方案适用于 AdGuardHome（以 arm64 架构为例）
---
# 一、 生成配置文件 .yaml 文件直链
具体方法此处不再赘述，请看《[生成带有自定义规则和代理组的配置文件 yaml 直链 geo 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E8%A7%84%E5%88%99%E5%92%8C%E4%BB%A3%E7%90%86%E7%BB%84%E7%9A%84%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%20yaml%20%E7%9B%B4%E9%93%BE%20geo%20%E6%96%B9%E6%A1%88.md)》，贴一下我使用的配置：
- 注：`rules` 部分的 `geosite` 和 `geoip` 内容须与 `geox-url` 中的路由规则文件相匹配

```
proxy-providers:
  🛫 我的机场:
    type: http
    # 修改为你的 Clash 订阅链接
    url: "https://example.com/xxx/xxx&flag=clash"
    path: ./proxies/airport.yaml
    interval: 43200
    filter: "香港|台湾|日本|韩国|新加坡|美国"
    health-check:
      enable: true
      url: "https://www.gstatic.com/generate_204"
      interval: 600

proxy-groups:
  - {name: 🚀 节点选择, type: select, proxies: [🇭🇰 香港节点, 🇹🇼 台湾节点, 🇯🇵 日本节点, 🇰🇷 韩国节点, 🇸🇬 新加坡节点, 🇺🇸 美国节点]}

  - {name: 📈 网络测试, type: select, proxies: [🎯 全球直连, 🇭🇰 香港节点, 🇹🇼 台湾节点, 🇯🇵 日本节点, 🇰🇷 韩国节点, 🇸🇬 新加坡节点, 🇺🇸 美国节点]}

  - {name: 🐟 漏网之鱼, type: select, proxies: [🚀 节点选择, 🎯 全球直连]}

  - {name: ⚡ 直连域名, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🪜 代理域名, type: select, proxies: [🚀 节点选择, 🎯 全球直连]}

  - {name: 🎮 国区游戏, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: Ⓜ️ Microsoft 中国, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🗽 Google 中国, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🍎 Apple 中国, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: 🇨🇳 国内 IP, type: select, proxies: [🎯 全球直连, 🚀 节点选择]}

  - {name: ✈️ Telegram IP, type: select, proxies: [🚀 节点选择]}

  - {name: 🏠 私有网络, type: select, proxies: [🎯 全球直连]}

  - {name: 🎯 全球直连, type: select, proxies: [DIRECT]}

  # 采用节点负载均衡策略，优点是更稳定，速度可能有提升；推荐在节点复用比较多的情况下使用
  - {name: 🇭🇰 香港节点, type: load-balance, strategy: consistent-hashing, lazy: true, use: [🛫 我的机场], filter: "香港"}

  - {name: 🇹🇼 台湾节点, type: load-balance, strategy: consistent-hashing, lazy: true, use: [🛫 我的机场], filter: "台湾"}

  - {name: 🇯🇵 日本节点, type: load-balance, strategy: consistent-hashing, lazy: true, use: [🛫 我的机场], filter: "日本"}

  - {name: 🇰🇷 韩国节点, type: load-balance, strategy: consistent-hashing, lazy: true, use: [🛫 我的机场], filter: "韩国"}

  - {name: 🇸🇬 新加坡节点, type: load-balance, strategy: consistent-hashing, lazy: true, use: [🛫 我的机场], filter: "新加坡"}

  - {name: 🇺🇸 美国节点, type: load-balance, strategy: consistent-hashing, lazy: true, use: [🛫 我的机场], filter: "美国"}

rules:
  - GEOSITE,lan,🏠 私有网络
  - GEOSITE,networktest,📈 网络测试
  - GEOSITE,microsoft-cn,Ⓜ️ Microsoft 中国
  - GEOSITE,apple-cn,🍎 Apple 中国
  - GEOSITE,google-cn,🗽 Google 中国
  - GEOSITE,games-cn,🎮 国区游戏
  - GEOSITE,proxy,🪜 代理域名
  - GEOSITE,cn,⚡ 直连域名
  - GEOIP,telegramip,✈️ Telegram IP
  - GEOIP,lanip,🏠 私有网络,no-resolve
  - GEOIP,cn,🇨🇳 国内 IP
  - MATCH,🐟 漏网之鱼
```
# 二、 导入 [Clash.Meta 内核](https://github.com/MetaCubeX/Clash.Meta)
我用的 Alpha 版，目前一直很稳定  
连接 SSH 后运行如下命令：
```
curl -o /tmp/clash.meta-linux-armv8 -L https://ghproxy.com/https://github.com/DustinWin/clash-tools/releases/download/latest/clash.meta-linux-armv8
```
# 三、 导入路由规则文件和 user.yaml
注：
- 1. **路由规则文件和 user.yaml 都属高度定制，牵一发而动全身**
- 2. geosite.dat 和 user.yaml 来源于 [DustinWin/clash-geosite](https://github.com/DustinWin/clash-geosite)
- 3. geoip.dat 来源于 [DustinWin/clash-geoip](https://github.com/DustinWin/clash-geoip)

连接 SSH 后运行如下命令：  
注：
- 1. 由于 ShellClash 采用的 DNS 模式为 fake-ip，当 AdGuardHome 的黑名单下载地址在 `🪜 代理域名`或 `🐟 漏网之鱼`内时，ShellClash 传给 AdGuardHome 的该下载地址对应 IP 为假 IP，所以会造成黑名单下载失败
- 2. AdHuardHome **自带的添加黑名单列表**中的下载域名都是“adguardteam.github.io”，所以我定制的 user.yaml 中 `fake-ip-filter` 列表内含有 `- adguardteam.github.io` 域名，并添加了 `- anti-ad.net`（下载地址 URL 可手动修改为 https://anti-ad.net/easylist.txt ，下载更稳定），黑名单可正常下载，添加了 `- 'static.adtidy.org'` 以解决右下角报错“检查更新失败”的问题

```
curl -o $clashdir/GeoSite.dat -L https://ghproxy.com/https://github.com/DustinWin/clash-geosite/releases/download/latest/geosite-lite.dat
curl -o $clashdir/GeoIP.dat -L https://ghproxy.com/https://github.com/DustinWin/clash-geoip/releases/download/latest/geoip.dat
curl -o $clashdir/Country.mmdb -L https://ghproxy.com/https://github.com/DustinWin/clash-geoip/releases/download/latest/Country.mmdb
curl -o $clashdir/yamls/user.yaml -L https://ghproxy.com/https://github.com/DustinWin/clash-geosite/releases/download/latest/fake-ip-user.yaml
```
# 四、 添加定时任务
连接 SSH 后运行 `crontab -e`，按一下 Ins 键（Insert 键），在最下方粘贴如下内容：
```
30 3 * * * curl -o /data/clash/clash -L https://ghproxy.com/https://github.com/DustinWin/clash-tools/releases/download/latest/clash.meta-linux-armv8 && chmod +x /data/clash/clash && /data/clash/start.sh restart >/dev/null 2>&1 #每天早上 3 点半更新 Clash.Meta 内核
0 4 * * * curl -o /data/clash/GeoSite.dat -L https://ghproxy.com/https://github.com/DustinWin/clash-geosite/releases/download/latest/geosite-lite.dat && curl -o /data/clash/GeoIP.dat -L https://ghproxy.com/https://github.com/DustinWin/clash-geoip/releases/download/latest/geoip.dat && curl -o /data/clash/Country.mmdb -L https://ghproxy.com/https://github.com/DustinWin/clash-geoip/releases/download/latest/Country.mmdb && /data/clash/start.sh restart >/dev/null 2>&1 #每天早上 4 点更新路由规则文件
30 4 * * * curl -o /data/AdGuardHome/AdGuardHome -L https://ghproxy.com/https://raw.githubusercontent.com/DustinWin/clash-tools/main/AdGuardHome/AdGuardHome_linux_armv8 && chmod +x /data/AdGuardHome/AdGuardHome && /data/AdGuardHome/AdGuardHome -s start >/dev/null 2>&1 #每天早上 4 点半更新 AdGuardHome
0 5 * * 1,3,5 curl -o /data/clash/yamls/user.yaml -L https://ghproxy.com/https://github.com/DustinWin/clash-geosite/releases/download/latest/fake-ip-user.yaml && /data/clash/start.sh updateyaml && /data/clash/start.sh restart >/dev/null 2>&1 #每周一、三、五早上 5 点更新 user.yaml 和订阅
```
按一下 Esc 键（退出键），输入英文冒号“:”，继续输入“wq”并回车，运行如下命令：
```
/etc/init.d/cron restart
```
# 五、 设置部分
## 1. ShellClash 设置
① 设置可参考《[ShellClash 配置 geo 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/ShellClash%20%E9%85%8D%E7%BD%AE%20geo%20%E6%96%B9%E6%A1%88.md)》，此处只列举配置的不同之处  
② 进入主菜单->2 clash功能设置，设置如下：  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/031ab115-bca8-4427-819a-200b19274e75" width="60%"/>  

③ 进入主菜单->5 设置定时任务，查看定时任务是否添加成功  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/1755d7d8-d011-4119-a52a-4c213c138427" width="60%"/>  

④ 进入主菜单->6 导入配置文件->6 配置文件覆写->1 自定义端口及秘钥，设置如下：  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/feea34a4-3b25-4c3d-b814-c4bbd8186636" width="60%"/>  

⑤ 进入主菜单->7 clash 进阶设置->6 配置内置 DNS 服务，设置如下：  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/4ec93fb8-1200-4baa-b3b4-29d164d7a743" width="60%"/>  

⑥ 进入主菜单->6 导入配置文件->2 导入 Clash 配置文件链接，粘贴第一步中生成的配置文件 .yaml 文件直链，启动 clash 服务即可
## 2. AdGuardHome 设置
设置可参考《[AdGuardHome 配置](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%85%A8%E7%BD%91%E6%9C%80%E8%AF%A6%E7%BB%86%E7%9A%84%E8%A7%A3%E9%94%81%20SSH%20ShellClash%20%E6%90%AD%E9%85%8D%20AdGuardHome%20%E5%AE%89%E8%A3%85%E5%92%8C%E9%85%8D%E7%BD%AE%E6%95%99%E7%A8%8B.md#2-adguardhome-%E9%85%8D%E7%BD%AE)》
# 六、 在线 Dashboard 面板
推荐使用在线 Dashboard 面板 [Yacd-meta](https://github.com/MetaCubeX/Yacd-meta)，访问地址：https://yacd.metacubex.one
1. 需要设置该网站“允许不安全内容”，以 Chrome 浏览器为例，进入设置->隐私和安全->网站设置->更多内容设置->不安全内容（或者直接打开 `chrome://settings/content/insecureContent` 进行设置），在“允许显示不安全内容”内添加 `https://yacd.metacubex.one`  
<img src="https://user-images.githubusercontent.com/45238096/235448980-52331db5-6b9f-4b0c-a876-1509d34db51a.png" width="60%"/>  

2. 首次进入 https://yacd.metacubex.one 需要添加“API Base URL”，输入 `http://192.168.31.1:9090` 并点击“Add”，最后点击下方新增的 http://192.168.31.1:9090 即可访问 Dashboard 面板  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/086aa876-109a-4a6f-9e9b-22269a869b4f" width="60%"/>
