### 前言：
1. 本教程可以生成扩展名为 yaml 的直链，可以一键导入 ShellClash（6-2） 和各平台的 Clash 客户端
2. 生成的订阅链接地址不会改变，支持更新订阅，支持同步机场节点
3. 生成的订阅链接带有规则，规则参考 https://github.com/Loyalsoldier/clash-rules
4. 强烈建议生成订阅链接后先导入 Clash for Windows 进行测试，测试通过后再进入 ShellClash-6-2 导入（不过也有 Clash for Windows 测试不通过，但导入 ShellClash 成功的情况，请仔细斟酌）
5. 请先确定自己机场的订阅链接是否支持 Clash，若不支持，可前往 https://acl4ssr-sub.github.io 网站进行生成，参数全部默认即可，再将生成后的订阅链接添加到.yaml 文件中
### 一、 注册 Gist
进入 https://gist.github.com 网站并注册
### 二、 准备编辑.yaml 文件
1. 登录 Gist 后打开 https://gist.github.com 链接可以直接编辑文件，或者鼠标点击页面右上角头像左边的“+”图标
2. “Gist description...”输入描述，随意填写；“Filename including extension...”输入完整文件名包括扩展名，如 clashlink.yaml
![QQ截图20230217162956](https://user-images.githubusercontent.com/45238096/219593234-64833fcd-5200-4bea-849f-a1865d341fd2.png)
### 三、 编辑.yaml 文件
1. 白名单模式（没有命中规则的网络流量，统统使用代理，适用于服务器线路网络质量稳定、快速，不缺服务器流量的用户）  
模板文件地址：https://gist.githubusercontent.com/DustinWin/a6d67d1c2c5da5ece004efcd791e4bf4/raw/template_whitelist.yaml  
将模板文件中的所有内容复制到自己 Gist 新建的.yaml 文件中
2. 黑名单模式（只有命中规则的网络流量，才使用代理，适用于服务器线路网络质量不稳定或不够快，或服务器流量紧缺的用户。通常也是软路由用户、家庭网关用户的常用模式）  
模板文件地址：https://gist.githubusercontent.com/DustinWin/cb01b32ce4463da29e22ec764815902a/raw/template_blacklist.yaml  
将模板文件中的所有内容复制到自己 Gist 新建的.yaml 文件中
3. 需要修改的内容  
① 首先确定自己机场中有哪些国家或地区的节点，对模板文件中“proxy-providers”、“proxy-groups”和“proxy-groups”中“🔰 节点选择”下的“proxies”里面的国家和地区进行增删改  
注：三者中的国家或地区必须一一对应，新增就全部新增，删除就全部删除，修改就全部修改（重要）  
② 将“proxy-providers”中的所有“url”链接全部改成自己机场的订阅链接（必须支持 Clash，详见《前言：5》）  
③ “proxy-providers”中的“filter”支持正则表达式，可以更加精确地筛选出机场中的国家或地区节点  
例如：我想筛选出所有带有“香港”二字的节点，但节点名称结尾不能有“#”，“filter”可以这样写：
`filter: "(香港).[^#]$"`  
④ 在“proxy-groups”中“🔰 节点选择”下的“proxies”里，可以将最稳定的节点放在最前面，这样重启路由器后可以默认选择最稳定的节点
### 四、 生成.yaml 文件链接
1. 生成链接  
编辑完成后，点击右下角的“Create secret gist”按钮，然后点击右上角的“Raw”按钮
![QQ截图20230217171809](https://user-images.githubusercontent.com/45238096/219603714-534fe617-35b2-4f5d-acea-b2e691c50bed.png)
2. 修改链接  
删除地址栏中网址后面的一串随机码，如：  
这是原地址：  
`https://gist.githubusercontent.com/DustinWin/a6d67d1c2c5da5ece004efcd791e4bf4/raw/df770aae2001b2eab426a385ea10bbbb35a35c52/template_whitelist.yaml`  
将后面的一串随机码（当前编辑该文件生成的随机码）“df770aae2001b2eab426a385ea10bbbb35a35c52”删除，变成：  
`https://gist.githubusercontent.com/DustinWin/a6d67d1c2c5da5ece004efcd791e4bf4/raw/template_whitelist.yaml`  
之后的.yaml 文件链接就是最终生成的订阅链接  
注：该订阅链接地址不会改变，在不更改文件名的情况下即使编辑该.yaml 文件并提交了 n 次也不会改变
### 五、 机场订阅链接换了或者直接更换了机场
可直接编辑该.yaml 文件，重复《三》中的步骤即可，然后在 ShellClash（6-4 手动更新或 5-5 添加自动更新）和各个平台的 Clash 客户端中更新订阅就可以了
### 六、 私人定制
到了这里，相信你对里面的机制有了一定的认识，那么我们可以对自己的需求进行定制了。最常见的有：我购买的机场支持奈飞，但仅日本节点和新加坡节点支持，这个规则怎么写？  
注：以下只是节选，请酌情套用
```
proxy-providers:
  🎥 奈飞:
    type: http
    filter: "日本|新加坡"
    url: https://example.com/xxx/clash
    path: ./proxies/netflix.yaml
    interval: 86400
    health-check:
      enable: true
      url: http://www.gstatic.com/generate_204
      interval: 300

proxy-groups:
  - name: 🎥 奈飞节点
    type: url-test
    url: http://www.gstatic.com/generate_204
    interval: 300
    use:
      - 🎥 奈飞

rule-providers:
  netflix:
    type: http
    behavior: classical
    url: "https://ghproxy.com/https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/Clash/Netflix/Netflix.yaml"
    path: ./ruleset/netflix.yaml
    interval: 86400

rules:
  # 此条写在 rules 最前面
  - RULE-SET,netflix,🎥 奈飞节点
```
