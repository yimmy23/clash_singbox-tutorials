# 说明：
1. 本教程中 **[AdGuardHome](https://github.com/AdguardTeam/AdGuardHome) 安装目录为`/data/AdGuardHome`**
2. 本教程中的下载链接以 CPU 架构 ARMv8 为例，请注意修改链接后缀
3. 查看 CPU 架构可连接 SSH 后执行命令 `uname -ms`，若执行结果是“linux aarch64”，就是搭载的 ARMv8 架构
4. 以下所有命令均可全部复制后直接粘贴执行
---
# 一、 安装 [ShellCrash](https://github.com/juewuy/ShellCrash)
## 1. 本地安装
连接 SSH 后执行如下命令：
```
curl -o /tmp/ShellCrash.tar.gz -L https://cdn.jsdelivr.net/gh/juewuy/ShellCrash@master/bin/ShellCrash.tar.gz
mkdir -p /tmp/SC_tmp && tar -zxf '/tmp/ShellCrash.tar.gz' -C /tmp/SC_tmp/ && source /tmp/SC_tmp/init.sh
```
## 2. 在线安装
连接 SSH 后执行如下命令：
```
export url='https://cdn.jsdelivr.net/gh/juewuy/ShellCrash@master' && sh -c "$(curl -kfsSl $url/install.sh)" && source /etc/profile &> /dev/null
```
# 二、 安装 [Clash.Meta 内核](https://github.com/MetaCubeX/mihomo) 或 [sing-box 内核](https://github.com/SagerNet/sing-box)
**Clash.Meta 内核下载链接后缀和 CPU 架构对应关系如下：**
|CPU 架构|AMD64|ARMv5|ARMv6|ARMv7|ARMv8|mips-softfloat|mipsle-hardfloat|mipsle-softfloat|
|-----|-----|-----|-----|-----|-----|-----|-----|-----|
|**链接后缀**|`amd64`|`armv5`|`armv6`|`armv7`|`armv8`|`mips-softfloat`|`mipsle-hardfloat`|`mipsle-softfloat`|

**sing-box 内核下载链接后缀和 CPU 架构对应关系如下：**
|CPU 架构|AMD64|AMD64v3|ARMv7|ARMv8|
|-----|-----|-----|-----|-----|
|**链接后缀**|`amd64`|`amd64v3`|`armv7`|`armv8`|

连接 SSH 后执行如下命令：
```
# Clash.Meta 内核 Release 版
curl -o /tmp/clash.meta-linux-armv8 -L https://cdn.jsdelivr.net/gh/DustinWin/clash_singbox-tools@main/Clash.Meta-release/clash.meta-linux-armv8 && clash
# Clash.Meta 内核 Alpha 版
curl -o /tmp/clash.meta-linux-armv8 -L https://cdn.jsdelivr.net/gh/DustinWin/clash_singbox-tools@main/Clash.Meta-alpha/clash.meta-linux-armv8 && clash
# sing-box 内核 Release 版
curl -o /tmp/sing-box-linux-armv8 -L https://cdn.jsdelivr.net/gh/DustinWin/clash_singbox-tools@main/sing-box-release/sing-box-linux-armv8 && clash
# sing-box 内核 Pre-release 版
curl -o /tmp/sing-box-linux-armv8 -L https://cdn.jsdelivr.net/gh/DustinWin/clash_singbox-tools@main/sing-box-prerelease/sing-box-linux-armv8 && clash
```
此时脚本会自动“发现可用的内核文件”，选择 1 加载，后选择对应的内核类型  
或者执行如下命令：
```
# Clash.Meta 内核 Release 版
curl -o $CRASHDIR/CrashCore -L https://cdn.jsdelivr.net/gh/DustinWin/clash_singbox-tools@main/Clash.Meta-release/clash.meta-linux-armv8
# Clash.Meta 内核 Alpha 版
curl -o $CRASHDIR/CrashCore -L https://cdn.jsdelivr.net/gh/DustinWin/clash_singbox-tools@main/Clash.Meta-alpha/clash.meta-linux-armv8
# sing-box 内核 Release 版
curl -o $CRASHDIR/CrashCore -L https://cdn.jsdelivr.net/gh/DustinWin/clash_singbox-tools@main/sing-box-release/sing-box-linux-armv8
# sing-box 内核 Pre-release 版
curl -o $CRASHDIR/CrashCore -L https://cdn.jsdelivr.net/gh/DustinWin/clash_singbox-tools@main/sing-box-prerelease/sing-box-linux-armv8
chmod +x $CRASHDIR/CrashCore && $CRASHDIR/start.sh restart
```
# 三、 安装 AdGuardHome
## 1. 安装 AdGuardHome
AdGuardHome CPU 架构和链接后缀对应关系如下：
|CPU 架构|AMD64|ARMv5|ARMv6|ARMv7|ARMv8|mips-softfloat|mipsle-softfloat|
|-----|-----|-----|-----|-----|-----|-----|-----|
|链接后缀|`amd64`|`armv5`|`armv6`|`armv7`|`armv8`|`mips-softfloat`|`mipsle-softfloat`|

执行如下命令：
```
mkdir -p /data/AdGuardHome
# AdGuardHome Release 版
curl -o /data/AdGuardHome/AdGuardHome -L https://cdn.jsdelivr.net/gh/DustinWin/clash-tools@main/AdGuardHome-release/AdGuardHome_linux_armv8
# AdGuardHome Pre-release 版
curl -o /data/AdGuardHome/AdGuardHome -L https://cdn.jsdelivr.net/gh/DustinWin/clash-tools@main/AdGuardHome-prerelease/AdGuardHome_linux_armv8
chmod +x /data/AdGuardHome/AdGuardHome
/data/AdGuardHome/AdGuardHome -s install
/data/AdGuardHome/AdGuardHome -s start
vi /data/auto_ssh/auto_ssh.sh
```
## 2. 添加开机启动
按一下 Ins 键（Insert 键），在最下方粘贴如下命令：
- 注：保留首尾的空行

```

/data/AdGuardHome/AdGuardHome -s install
/data/AdGuardHome/AdGuardHome -s start

```
按一下 Esc 键（退出键），输入英文冒号“:”，继续输入“wq”并回车
## 3. 升级 AdGuardHome
- 注：留意链接后缀是否与 CPU 架构匹配

执行如下命令：
```
# AdGuardHome Release 版
curl -o /data/AdGuardHome/AdGuardHome -L https://mirror.ghproxy.com/https://raw.githubusercontent.com/DustinWin/clash-tools/main/AdGuardHome-release/AdGuardHome_linux_armv8
# AdGuardHome Pre-release 版
curl -o /data/AdGuardHome/AdGuardHome -L https://mirror.ghproxy.com/https://raw.githubusercontent.com/DustinWin/clash-tools/main/AdGuardHome-prerelease/AdGuardHome_linux_armv8
/data/AdGuardHome/AdGuardHome -s restart
```
