# ClashLink
用于生成 Clash 配置文件直链

## Clash 配置文件格式
### 1. 节点
```
proxy-providers:
  [节点名称 1]:
    type: [节点类型：http]
    path: [节点下载路径：./proxies/airplane.yaml]
    url: [订阅链接：https://example.com]
    interval: [节点更新间隔（秒）：86400]
    filter: [过滤器："VIP|01"]
    health-check:
      enable: [启用：true]
      url: [策略延迟测试网址：http://www.gstatic.com/generate_204]
      interval: [策略延迟测试间隔（毫秒）：300]
```
### 2. 策略组
```
proxy-groups:
  - name: [策略名称 1]
    type: [策略类型：select]
    proxies:
      - [策略名称 2]
    use:
      - [节点名称 1]

  - name: [策略名称 2]
    type: [策略类型：url-test]
    url: [策略延迟测试网址：http://www.gstatic.com/generate_204]
    interval: [策略延迟测试间隔（毫秒）：300]
    use:
      - [节点名称 1]

  - name: [策略名称 3]
    type: select
    proxies:
      - [策略名称 1]
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
  - DOMAIN-KEYWORD,[域名关键字],[策略名称、DIRECT 或 REJECT]
  - DOMAIN-SUFFIX,[域名后缀],[策略名称、DIRECT 或 REJECT]
  - RULE-SET,[供应商名称],[策略名称、DIRECT 或 REJECT]
  - GEOIP,LAN,[策略名称、DIRECT 或 REJECT],[是否进行 DNS 解析：no-resolve]
  - GEOIP,CN,[策略名称、DIRECT 或 REJECT],[是否进行 DNS 解析：no-resolve]
  - MATCH,[策略名称（漏网之鱼）]
```
