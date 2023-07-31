# 说明：
1. 本教程中 **[AdGuardHome](https://github.com/AdguardTeam/AdGuardHome) 安装目录为`/data/AdGuardHome`**
2. 查看 CPU 架构可连接 SSH 后执行命令 `uname -ms`，若执行结果是“linux aarch64”，就是搭载的 ARMv8 架构  
3. 以下所有命令均可全部复制后直接粘贴执行
---
# 一、 安装 [ShellClash](https://github.com/juewuy/ShellClash)
执行如下命令：
```
curl -o /tmp/ShellClash.tar.gz -L https://cdn.jsdelivr.net/gh/juewuy/ShellClash@master/bin/ShellClash.tar.gz
mkdir -p /tmp/SC_tmp && tar -zxf '/tmp/ShellClash.tar.gz' -C /tmp/SC_tmp/ && source /tmp/SC_tmp/init.sh
```
选择 1 安装到/data 目录（推荐，支持软固化功能）
# 二、 安装 [Clash.Meta](https://github.com/MetaCubeX/Clash.Meta) 内核
- 注：如果使用的是搭载非 ARMv8 架构 CPU 的路由器，需要修改一下链接后缀，其余部分不需要任何修改就可以正常识别

比如搭载的是 ARMv7 架构的 CPU，就修改为：  
`https://cdn.jsdelivr.net/gh/DustinWin/clash-tools@main/Clash.Meta-release/clash.meta-linux-armv7`  
其它架构 CPU 的链接后缀对应如下：

|CPU 架构|链接后缀|
|-----|-----|
|AMD64|`Clash.Meta-linux-amd64`|
|ARMv5|`Clash.Meta-linux-armv5`|
|ARMv6|`Clash.Meta-linux-armv6`|
|ARMv7|`Clash.Meta-linux-armv7`|
|ARMv8|`Clash.Meta-linux-armv8`|
|mips-softfloat|`Clash.Meta-linux-mips-softfloat`|
|mipsle-hardfloat|`Clash.Meta-linux-mipsle-hardfloat`|
|mipsle-softfloat|`Clash.Meta-linux-mipsle-softfloat`|
|Windows-AMD64|`clash.meta-windows-amd64.exe`|

执行如下命令：
```
curl -o /tmp/clash.meta-linux-armv8 -L https://cdn.jsdelivr.net/gh/DustinWin/clash-tools@main/Clash.Meta-release/clash.meta-linux-armv8 && clash
```
此时脚本会自动“发现可用的内核文件”，选择 1 加载，后选择 3 Clash.Meta 内核  
或者执行如下命令：
```
curl -o $clashdir/clash -L https://cdn.jsdelivr.net/gh/DustinWin/clash-tools@main/Clash.Meta-release/clash.meta-linux-armv8
chmod +x $clashdir/clash && $clashdir/start.sh restart
```

# 三、 安装 AdGuardHome
- 注：DNS 服务器监听端口须与命令中的端口保持一致，此处设为 5625

## 1. 安装 AdGuardHome
- 注：如果使用的是搭载非 ARMv8 架构 CPU 的路由器，需要修改一下链接后缀，其余部分不需要任何修改就可以正常识别

比如搭载的是 ARMv7 架构的 CPU，就修改为：  
`https://cdn.jsdelivr.net/gh/DustinWin/clash-tools@main/AdGuardHome/AdGuardHome_linux_armv7`  
其它架构 CPU 的链接后缀对应如下：

|CPU 架构|链接后缀|
|-----|-----|
|AMD64|`AdGuardHome_linux_amd64`|
|ARMv5|`AdGuardHome_linux_armv5`|
|ARMv6|`AdGuardHome_linux_armv6`|
|ARMv7|`AdGuardHome_linux_armv7`|
|ARMv8|`AdGuardHome_linux_armv8`|
|mips-softfloat|`AdGuardHome_linux_mips_softfloat`|
|mipsle-softfloat|`AdGuardHome_linux_mipsle_softfloat`|
|Windows-AMD64|`AdGuardHome_windows_amd64.exe`|

执行如下命令：
```
mkdir -p /data/AdGuardHome
curl -o /data/AdGuardHome/AdGuardHome -L https://cdn.jsdelivr.net/gh/DustinWin/clash-tools@main/AdGuardHome/AdGuardHome_linux_armv8
chmod +x /data/AdGuardHome/AdGuardHome
/data/AdGuardHome/AdGuardHome -s install
/data/AdGuardHome/AdGuardHome -s start
iptables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 5625
iptables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 5625
ip6tables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 5625
ip6tables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 5625
vi /data/auto_ssh/auto_ssh.sh
```
## 2. 添加开机启动
按一下 Ins 键（Insert 键），在最下方粘贴如下命令：
```
/data/AdGuardHome/AdGuardHome -s install
/data/AdGuardHome/AdGuardHome -s start
iptables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 5625
iptables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 5625
ip6tables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 5625
ip6tables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 5625
```
按一下 Esc 键（退出键），输入英文冒号“:”，继续输入“wq”并回车
# 四、 升级 AdGuardHome
执行如下命令：
```
curl -o /data/AdGuardHome/AdGuardHome -L https://ghproxy.com/https://raw.githubusercontent.com/DustinWin/clash-tools/main/AdGuardHome/AdGuardHome_linux_armv8
chmod +x /data/AdGuardHome/AdGuardHome && reboot
```
