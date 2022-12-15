# ClashLink
用于生成 Clash 配置文件直链

## Clash 配置文件格式
### 1. 节点
```
proxies:
  - name: [节点名称]
    type: [节点类型：ss]
    server: [节点服务地址]
    port: [节点端口号]
    password: [节点服务密码]
    cipher: [节点服务加密方式：aes-256-gcm]
    udp: [节点启用 UDP：true 或 false]

  - name: [节点名称]
    type: [节点类型：vmess]
    server: [节点服务地址]
    port: [节点端口号]
    uuid: [节点服务密码]
    alterId: [额外 ID：0]
    cipher: [节点服务加密方式：chacha20-poly1305]
```
### 2. 策略组
```
proxy-groups:
  - name: [策略名称]
    type: [策略类型：select]
    proxies:
      - [节点名称 1]
      - [节点名称 2]
      - [策略名称 1]
      - [策略名称 2]

  - name: [策略名称]
    type: [策略类型：url-test]
    url: [策略延迟测试网址：http://www.gstatic.com/generate_204]
    interval: [策略延迟测试间隔（毫秒）：300]
    proxies:
      - [节点名称 1]
      - [节点名称 2]
      - [节点名称 3]

  - name: [策略名称]
    type: select
    proxies:
      - [节点名称]
      - [策略名称]
      - DIRECT
      - REJECT
```
### 3. 供应商
```
rule-providers:
  [供应商名称]:
    type: [供应商类型：http]
    behavior: [供应商行为：classical、domain 或 ipcidr]
    url: [供应商下载地址（带引号）]
    path: [供应商下载路径：./ruleset/xxx.yaml]
    interval: [供应商下载间隔（秒）：86400]
```
### 4. 规则
```
rules:
  - RULE-SET,[供应商名称],[策略名称、DIRECT 或 REJECT]
  - DOMAIN-KEYWORD,[域名关键字],[策略名称、DIRECT 或 REJECT]
  - DOMAIN-SUFFIX,[域名后缀],[策略名称、DIRECT 或 REJECT]
  - GEOIP,LAN,[策略名称、DIRECT 或 REJECT],[是否进行 DNS 解析：no-resolve]
  - GEOIP,CN,[策略名称、DIRECT 或 REJECT],[是否进行 DNS 解析：no-resolve]
  - MATCH,[策略名称（漏网之鱼）]
```
