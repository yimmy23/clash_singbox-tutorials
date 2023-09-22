# 分享自己使用 [ShellClash](https://github.com/juewuy/ShellClash)（fake-ip 模式）搭配 [AdGuardHome](https://github.com/AdguardTeam/AdGuardHome) 的一套配置
# 声明：
1. 此方案采用 ShellClash 作为上游，AdGuardHome 作为下游的模式
2. 此方案采用 GEOSITE 和 GEOIP 规则搭配 geosite.dat 和 geoip.dat（或 Country.mmdb）[路由规则文件](https://github.com/DustinWin/clash-geosite)，属高度定制，仅供参考
3. 规则参考 [DustinWin/clash-geosite](https://github.com/DustinWin/clash-geosite)
4. 请根据自身情况进行修改，适合自己的方案才是最好的方案，如无特殊需求，可以照搬
5. 此方案中 ShellClash 采用的 **DNS 模式为 fake-ip**（仍与 AdGuardHome 配合完美）
6. 此方案适用于 ShellClash（以 arm64 架构为例）
7. 此方案适用于 AdGuardHome（以 arm64 架构为例）
---
# 一、 生成配置文件 .yaml 文件直链
具体方法此处不再赘述，请看《[生成带有自定义规则和代理组的配置文件 yaml 直链 geox 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E8%A7%84%E5%88%99%E5%92%8C%E4%BB%A3%E7%90%86%E7%BB%84%E7%9A%84%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%20yaml%20%E7%9B%B4%E9%93%BE%20geox%20%E6%96%B9%E6%A1%88.md)》，贴一下我使用的配置：
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

  - {name: ✈️ Telegram, type: select, proxies: [🚀 节点选择]}

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
  - GEOSITE,private,🏠 私有网络
  - GEOSITE,networktest,📈 网络测试
  - GEOSITE,microsoft-cn,Ⓜ️ Microsoft 中国
  - GEOSITE,apple-cn,🍎 Apple 中国
  - GEOSITE,google-cn,🗽 Google 中国
  - GEOSITE,games-cn,🎮 国区游戏
  - GEOSITE,proxy,🪜 代理域名
  - GEOSITE,cn,⚡ 直连域名
  - GEOIP,telegram,✈️ Telegram
  - GEOIP,private,🏠 私有网络,no-resolve
  - GEOIP,cn,🇨🇳 国内 IP
  - MATCH,🐟 漏网之鱼
```
# 二、 导入 [Clash.Meta 内核](https://github.com/MetaCubeX/Clash.Meta)  
连接 SSH 后运行如下命令：
```
curl -o /tmp/clash.meta-linux-armv8 -L https://fastly.jsdelivr.net/gh/DustinWin/clash-tools@release/clash.meta-linux-armv8 && clash
```
此时脚本会自动“发现可用的内核文件”，选择 1 加载，后选择 3 Clash.Meta 内核
# 三、 导入路由规则文件和 user.yaml
注：
- 1. **路由规则文件和 user.yaml 都属高度定制，牵一发而动全身**
- 2. geosite.dat 和 user.yaml 来源于 [DustinWin/clash-geosite](https://github.com/DustinWin/clash-geosite)
- 3. geoip.dat 来源于 [DustinWin/clash-geoip](https://github.com/DustinWin/clash-geoip)

连接 SSH 后运行如下命令：  
注：
- 1. 由于 ShellClash 采用的 DNS 模式为 fake-ip，**ShellClash 传给 AdGuardHome 的域名对应 IP 为假 IP**，会造成“DNS 黑名单”下载失败
- 2. 定制的 user.yaml 中的 `fake-ip-filter` 列表内新增 `- 'adguardteam.github.io'` 域名，以**解决在 AdGuardHome 自带的“DNS 黑名单”列表中添加规则失败的问题**
- 3. 定制的 user.yaml 中的 `fake-ip-filter` 列表内新增 `- 'anti-ad.net'` 域名，在添加“DNS 黑名单”时可以修改成该域名，下载更稳定
- 4. 定制的 user.yaml 中的 `fake-ip-filter` 列表内有 `- 'static.adtidy.org'` 域名（属 AdGuardHome 自带的版本检查域名），以**解决 AdGuardHome 右下角报错“检查更新失败”的问题**，但不推荐使用自带更新去升级，更推荐[添加定时任务](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%88%86%E4%BA%AB%E7%AF%87/%E5%88%86%E4%BA%AB%E8%87%AA%E5%B7%B1%E4%BD%BF%E7%94%A8%20ShellClash%20%E6%90%AD%E9%85%8D%20AdGuardHome%20%E7%9A%84%E4%B8%80%E5%A5%97%E9%85%8D%E7%BD%AE.md#%E5%9B%9B-%E6%B7%BB%E5%8A%A0%E5%AE%9A%E6%97%B6%E4%BB%BB%E5%8A%A1)去自动更新（AdGuardHome 程序已被压缩，节省空间）
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/fa87cd47-f74b-40f1-a105-cc660e2f44ee" width="60%"/>

```
curl -o $clashdir/GeoSite.dat -L https://fastly.jsdelivr.net/gh/DustinWin/clash-geosite@release/geosite-lite.dat
curl -o $clashdir/GeoIP.dat -L https://fastly.jsdelivr.net/gh/DustinWin/clash-geoip@release/geoip-lite.dat
curl -o $clashdir/Country.mmdb -L https://fastly.jsdelivr.net/gh/DustinWin/clash-geoip@release/Country-lite.mmdb
curl -o $clashdir/yamls/user.yaml -L https://fastly.jsdelivr.net/gh/DustinWin/clash-geosite@release/fake-ip-user.yaml
```
# 四、 添加定时任务
连接 SSH 后运行 `crontab -e`，按一下 Ins 键（Insert 键），在最下方粘贴如下内容：
```
30 3 * * 1,3,5 curl -o /data/clash/clash -L https://fastly.jsdelivr.net/gh/DustinWin/clash-tools@release/clash.meta-linux-armv8 \
&& chmod +x /data/clash/clash && /data/clash/start.sh restart >/dev/null 2>&1 #每周一、三、五早上 3 点半更新 Clash.Meta 内核
0 4 * * 1，3，5 curl -o /data/AdGuardHome/AdGuardHome -L https://fastly.jsdelivr.net/gh/DustinWin/clash-tools@release/AdGuardHome_linux_armv8 \
&& chmod +x /data/AdGuardHome/AdGuardHome && /data/AdGuardHome/AdGuardHome -s restart >/dev/null 2>&1 #每周一、三、五早上 4 点更新 AdGuardHome
30 4 * * * curl -o /data/clash/GeoSite.dat -L https://fastly.jsdelivr.net/gh/DustinWin/clash-geosite@release/geosite-lite.dat \
&& curl -o /data/clash/GeoIP.dat -L https://fastly.jsdelivr.net/gh/DustinWin/clash-geoip@release/geoip-lite.dat \
&& curl -o /data/clash/Country.mmdb -L https://fastly.jsdelivr.net/gh/DustinWin/clash-geoip@release/Country-lite.mmdb \
&& curl -o /data/clash/yamls/user.yaml -L https://fastly.jsdelivr.net/gh/DustinWin/clash-geosite@release/fake-ip-user.yaml \
&& /data/clash/start.sh restart >/dev/null 2>&1 #每天早上 4 点半更新路由规则文件和 user.yaml
0 5 * * 1,3,5 /data/clash/start.sh updateyaml && /data/clash/start.sh restart >/dev/null 2>&1 #每周一、三、五早上 5 点更新订阅
```
按一下 Esc 键（退出键），输入英文冒号“:”，继续输入“wq”并回车，运行如下命令：
```
/etc/init.d/cron restart
```
# 五、 设置部分
## 1. ShellClash 设置
① 设置可参考《[ShellClash 配置 geox 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/ShellClash%20%E9%85%8D%E7%BD%AE%20geo%20%E6%96%B9%E6%A1%88.md)》，此处只列举配置的不同之处  
② 进入主菜单->2 clash功能设置，设置如下：  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/39920422-3d2b-4ee9-806a-5e9ac31fb7e9" width="60%"/>  
③ 进入主菜单->5 设置定时任务，查看定时任务是否添加成功  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/8d04d73d-b46b-4164-86ff-8b7512fe773b" width="60%"/>  
④ 进入主菜单->6 导入配置文件->6 配置文件覆写->1 自定义端口及秘钥，设置如下：  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/feea34a4-3b25-4c3d-b814-c4bbd8186636" width="60%"/>  
⑤ 进入主菜单->7 clash 进阶设置->6 配置内置 DNS 服务，设置如下：  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/4ec93fb8-1200-4baa-b3b4-29d164d7a743" width="60%"/>  
⑥ 进入主菜单->6 导入配置文件->2 导入 Clash 配置文件链接，粘贴第一步中生成的配置文件 .yaml 文件直链，启动 clash 服务即可
## 2. AdGuardHome 设置
设置可参考《[AdGuardHome 配置](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%85%A8%E7%BD%91%E6%9C%80%E8%AF%A6%E7%BB%86%E7%9A%84%E8%A7%A3%E9%94%81%20SSH%20ShellClash%20%E6%90%AD%E9%85%8D%20AdGuardHome%20%E5%AE%89%E8%A3%85%E5%92%8C%E9%85%8D%E7%BD%AE%E6%95%99%E7%A8%8B.md#2-adguardhome-%E9%85%8D%E7%BD%AE)》
# 六、 在线 Dashboard 面板
推荐使用在线 Dashboard 面板 [metacubexd](https://github.com/metacubex/metacubexd)，访问地址：https://d.metacubex.one
1. 需要设置该网站“允许不安全内容”，以 Chrome 浏览器为例，进入设置->隐私和安全->网站设置->更多内容设置->不安全内容（或者直接打开 `chrome://settings/content/insecureContent` 进行设置），在“允许显示不安全内容”内添加 `https://d.metacubex.one`  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/97ff3448-e6a1-45e2-aac9-46a5676e98fc" width="60%"/>

2. 首次进入 https://d.metacubex.one 需要添加“后端地址”，输入 `http://192.168.31.1:9090` 并点击“添加”即可访问 Dashboard 面板  
<img src="https://github.com/DustinWin/clash-tutorials/assets/45238096/5551bf56-4c3e-4b31-aed9-214074bf92e1" width="60%"/>
