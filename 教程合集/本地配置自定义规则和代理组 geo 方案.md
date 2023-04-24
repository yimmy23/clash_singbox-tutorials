# 本地配置自定义规则和代理组
此方案采用 `GEOSITE` 和 `GEOIP` 规则搭配 geosite.dat 和 geoip.dat（或 Country.mmdb） 路由规则文件
# 前言
1. 本教程只适用于 [ShellClash](https://github.com/juewuy/ShellClash)
2. 不支持节点筛选，可使用 ShellClash 脚本配置->6->1->2 或 3 进行筛选
3. 自定义规则参考 [Loyalsoldier/v2ray-rules-dat](https://github.com/Loyalsoldier/clash-rules)
---
# 一、 ShellClash 配置
进入 ShellClash 脚本配置->6->1->4，选择 4 [Acl4SSR](https://acl4ssr-sub.github.io/) 极简版（适合自建节点）  
![QQ截图20230315130212](https://user-images.githubusercontent.com/45238096/225292060-270091da-324b-4c84-8f94-74c2fcb2dc75.png)  
然后在线生成 [Clash](https://github.com/Dreamacro/clash/wiki) 配置文件
# 二、 选择模式
## 1. [白名单模式](https://cdn.jsdelivr.net/gh/DustinWin/Clash-Tutorials@main/Rule-Templates/geo-mode/template_whitelist.yaml)
没有命中规则的网络流量，统统使用代理，适用于服务器线路网络质量稳定、快速，不缺服务器流量的用户  
运行如下命令：
```
curl -o $clashdir/proxy-groups.yaml -L https://cdn.jsdelivr.net/gh/DustinWin/Clash-Tutorials@main/Local-Rules/geo-mode/whitelist-mode/proxy-groups.yaml
curl -o $clashdir/rules.yaml -L https://cdn.jsdelivr.net/gh/DustinWin/Clash-Tutorials@main/Local-Rules/geo-mode/whitelist-mode/rules.yaml
$clashdir/start.sh restart
```
## 2. [黑名单模式](https://cdn.jsdelivr.net/gh/DustinWin/Clash-Tutorials@main/Rule-Templates/geo-mode/template_blacklist.yaml)
只有命中规则的网络流量，才使用代理，适用于服务器线路网络质量不稳定或不够快，或服务器流量紧缺的用户。通常也是软路由用户、家庭网关用户的常用模式  
运行如下命令：
```
curl -o $clashdir/proxy-groups.yaml -L https://cdn.jsdelivr.net/gh/DustinWin/Clash-Tutorials@main/Local-Rules/geo-mode/blacklist-mode/proxy-groups.yaml
curl -o $clashdir/rules.yaml -L https://cdn.jsdelivr.net/gh/DustinWin/Clash-Tutorials@main/Local-Rules/geo-mode/blacklist-mode/rules.yaml
$clashdir/start.sh restart
```
# 三、 修改规则或代理组
**举例：我想添加一个规则，使奈飞走日本和新加坡节点**  
① 进入 [v2fly/domain-list-community/data](https://github.com/v2fly/domain-list-community/tree/master/data) 后按 Ctrl+F 组合键搜索“netflix”  
② 进入 [Loyalsoldier/geoip/text](https://github.com/Loyalsoldier/geoip/tree/release/text) 后按 Ctrl+F 组合键搜索“netflix”  
③ 在上述两个网站中都能**精确搜索到结果**，那么就可以进行如下操作：  
注：
- 1. **一定要保证缩进对齐！一定要保证缩进对齐！一定要保证缩进对齐！**
- 2. 以下只是节选，请酌情套用
- 3. 推荐使用 [VSCode 编辑器](https://code.visualstudio.com/Download) 或其它专业文本编辑器

## 1. 修改 user.yaml 文件
连接 SSH，执行命令 `vi $clashdir/user.yaml`，按一下 Ins 键（Insert 键），粘贴如下内容：
```
proxy-providers:
  # 获取机场订阅链接内的所有节点
  🛫 我的机场:
    type: http
    # 机场订阅链接，使用 Clash 链接
    url: 'https://example.com/xxx/clash'
    path: ./proxies/airport.yaml
    interval: 86400
    health-check:
      enable: true
      interval: 600
      # 未选择到当前策略组时，不会进行测试
      # lazy: true
      url: 'https://www.gstatic.com/generate_204'
```
按一下 Esc 键（退出键），输入英文冒号“:”，继续输入“wq”并回车
## 2. 修改 proxy-groups.yaml 文件
连接 SSH，执行命令 `vi $clashdir/proxy-groups.yaml`，按一下 Ins 键（Insert 键），粘贴如下内容：
```
- name: 🎥 奈飞节点
  type: url-test
  # 容差大于 100ms 就会切换到延迟低的那个节点
  tolerance: 100
  use:
    # 使用 proxy-providers 中的节点名称
    - 🛫 我的机场
  # 筛选出日本和新加坡节点
  filter: "日本|新加坡"
```
按一下 Esc 键（退出键），输入英文冒号“:”，继续输入“wq”并回车
## 3. 修改 rules.yaml 文件
连接 SSH，执行命令 `vi $clashdir/rules.yaml`，按一下 Ins 键（Insert 键），**优先在最上方**粘贴如下内容：
```
# 自定义规则优先放前面
- GEOSITE,netflix,🎥 奈飞节点
- GEOIP,netflix,🎥 奈飞节点
```
按一下 Esc 键（退出键），输入英文冒号“:”，继续输入“wq”并回车
# 四、 添加小规则
仅添加特定网址走直连或走代理，连接 SSH 后执行命令 `vi $clashdir/rules.yaml`，按一下 Ins 键（Insert 键），在**最上方**粘贴如下内容：  
注：
- 1. 以下内容只是举例，请根据自身需要进行增删改
- 2. 其它规则请参考《[Clash.Meta Wiki](https://clash-meta.wiki/config/rules)》

```
# 以 googleapis.cn 为后缀的所有域名走代理
- DOMAIN-SUFFIX,googleapis.cn,🚀 节点选择

# 与哔哩哔哩相关的所有域名走直连
- GEOSITE,bilibili,DIRECT

# 含有 ipv6 关键字的所有域名走直连
- DOMAIN-KEYWORD,ipv6,DIRECT
```
按一下 Esc 键（退出键），输入英文冒号“:”，继续输入“wq”并回车
# 五、 使规则或代理组生效
启动 Clash 服务即可，也可连接 SSH 后执行如下命令：
```
$clashdir/start.sh restart
```
# 六、 更新 geoste.dat、geoip.dat 和 Country.mmdb
1. 若配置文件内含有 `geodata-mode: true` 这一项，连接 SSH 后，执行如下命令：
```
curl -o $clashdir/GeoSite.dat -L https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/geosite.dat
curl -o $clashdir/GeoIP.dat -L https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/geoip.dat
```
2. 若配置文件内没有 `geodata-mode: true` 这一项或含有 `geodata-mode: false`，连接 SSH 后，执行如下命令：
```
curl -o $clashdir/GeoSite.dat -L https://cdn.jsdelivr.net/gh/Loyalsoldier/v2ray-rules-dat@release/geosite.dat
curl -o $clashdir/Country.mmdb -L https://cdn.jsdelivr.net/gh/Loyalsoldier/geoip@release/Country.mmdb
```
