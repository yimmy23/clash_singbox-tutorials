**修改了几乎所有链接地址，请注意！最终方案已稳定（2023-09-10），以后仅针对新增功能进行修改**  
**ShellClash（fake-ip 模式）搭配 AdGuardHome 的完美方案，现已[出炉](https://github.com/DustinWin/clash-tutorials/tree/main#2-%E5%88%86%E4%BA%AB%E8%87%AA%E5%B7%B1%E4%BD%BF%E7%94%A8-shellclashfake-ip-%E6%A8%A1%E5%BC%8F%E6%90%AD%E9%85%8D-adguardhome-%E7%9A%84%E4%B8%80%E5%A5%97%E9%85%8D%E7%BD%AE)，强烈推荐！**

# 置顶教程：
## [全网最详细的解锁 SSH ShellClash 搭配 AdGuardHome 安装和配置教程](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%85%A8%E7%BD%91%E6%9C%80%E8%AF%A6%E7%BB%86%E7%9A%84%E8%A7%A3%E9%94%81%20SSH%20ShellClash%20%E6%90%AD%E9%85%8D%20AdGuardHome%20%E5%AE%89%E8%A3%85%E5%92%8C%E9%85%8D%E7%BD%AE%E6%95%99%E7%A8%8B.md)
## [ShellClash 和 AdGuardHome 快速安装教程](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/ShellClash%20%E5%92%8C%20AdGuardHome%20%E5%BF%AB%E9%80%9F%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B.md)
---
# Clash 教程合集
# 一、 geox 方案
注：
- 1. 此方案采用 `GEOSITE` 和 `GEOIP` 规则搭配 geosite.dat 和 geoip.dat（或 Country.mmdb） 路由规则文件
- 2. 此方案更适用于路由器等无法判断进程的设备，配置方便简单，对小白用户友好
## 1. 基础篇
### ① [生成带有自定义规则和代理组的配置文件 yaml 直链 geox 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E8%A7%84%E5%88%99%E5%92%8C%E4%BB%A3%E7%90%86%E7%BB%84%E7%9A%84%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%20yaml%20%E7%9B%B4%E9%93%BE%20geox%20%E6%96%B9%E6%A1%88.md)
### ② [ShellClash 配置 geox 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/ShellClash%20%E9%85%8D%E7%BD%AE%20geox%20%E6%96%B9%E6%A1%88.md)
### ③ [Clash Verge 配置 geox 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/Clash%20Verge%20%E9%85%8D%E7%BD%AE%20geox%20%E6%96%B9%E6%A1%88.md)
## 2. 进阶篇
### ① [ShellClash 本地配置自定义规则和代理组 geox 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E8%BF%9B%E9%98%B6%E7%AF%87/ShellClash%20%E6%9C%AC%E5%9C%B0%E9%85%8D%E7%BD%AE%E8%87%AA%E5%AE%9A%E4%B9%89%E8%A7%84%E5%88%99%E5%92%8C%E4%BB%A3%E7%90%86%E7%BB%84%20geox%20%E6%96%B9%E6%A1%88.md)
### ② [ShellClash 使用 Clash.Meta 内核进行 DNS 分流教程 geox 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E8%BF%9B%E9%98%B6%E7%AF%87/ShellClash%20%E4%BD%BF%E7%94%A8%20Clash.Meta%20%E5%86%85%E6%A0%B8%E8%BF%9B%E8%A1%8C%20DNS%20%E5%88%86%E6%B5%81%E6%95%99%E7%A8%8B%20geox%20%E6%96%B9%E6%A1%88.md)
### ③ [Clash Verge 使用 Clash.Meta 内核进行 DNS 分流教程 geox 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E8%BF%9B%E9%98%B6%E7%AF%87/Clash%20Verge%20%E4%BD%BF%E7%94%A8%20Clash.Meta%20%E5%86%85%E6%A0%B8%E8%BF%9B%E8%A1%8C%20DNS%20%E5%88%86%E6%B5%81%E6%95%99%E7%A8%8B%20geox%20%E6%96%B9%E6%A1%88.md)
## 3. 分享篇
### 1. [分享自己使用 ShellClash 搭配 geox 方案的一套配置](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%88%86%E4%BA%AB%E7%AF%87/%E5%88%86%E4%BA%AB%E8%87%AA%E5%B7%B1%E4%BD%BF%E7%94%A8%20ShellClash%20%E6%90%AD%E9%85%8D%20geox%20%E6%96%B9%E6%A1%88%E7%9A%84%E4%B8%80%E5%A5%97%E9%85%8D%E7%BD%AE.md)
### 2. [分享自己使用 ShellClash（fake-ip 模式）搭配 AdGuardHome 的一套配置](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%88%86%E4%BA%AB%E7%AF%87/%E5%88%86%E4%BA%AB%E8%87%AA%E5%B7%B1%E4%BD%BF%E7%94%A8%20ShellClash%EF%BC%88fake-ip%20%E6%A8%A1%E5%BC%8F%EF%BC%89%E6%90%AD%E9%85%8D%20AdGuardHome%20%E7%9A%84%E4%B8%80%E5%A5%97%E9%85%8D%E7%BD%AE.md)
# 二、 rule-set 方案
注：
- 1. 此方案采用 `RULE-SET` 规则搭配 `rule-providers` 配置项
- 2. 此方案适用于对分流规则要求比较严格的用户，按需配置且配置灵活
## 1. 基础篇
### ① [生成带有自定义规则和代理组的配置文件 yaml 直链 rule-set 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/%E7%94%9F%E6%88%90%E5%B8%A6%E6%9C%89%E8%87%AA%E5%AE%9A%E4%B9%89%E8%A7%84%E5%88%99%E5%92%8C%E4%BB%A3%E7%90%86%E7%BB%84%E7%9A%84%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%20yaml%20%E7%9B%B4%E9%93%BE%20rule-set%20%E6%96%B9%E6%A1%88.md)
### ② [ShellClash 配置 rule-set 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/ShellClash%20%E9%85%8D%E7%BD%AE%20rule-set%20%E6%96%B9%E6%A1%88.md)
### ③ [Clash Verge 配置 rule-set 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%9F%BA%E7%A1%80%E7%AF%87/Clash%20Verge%20%E9%85%8D%E7%BD%AE%20rule-set%20%E6%96%B9%E6%A1%88.md)
## 2. 进阶篇
### ① [ShellClash 本地配置自定义规则和代理组 rule-set 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E8%BF%9B%E9%98%B6%E7%AF%87/ShellClash%20%E6%9C%AC%E5%9C%B0%E9%85%8D%E7%BD%AE%E8%87%AA%E5%AE%9A%E4%B9%89%E8%A7%84%E5%88%99%E5%92%8C%E4%BB%A3%E7%90%86%E7%BB%84%20rule-set%20%E6%96%B9%E6%A1%88.md)
### ② [ShellClash 使用 Clash.Meta 内核进行 DNS 分流教程 rule-set 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E8%BF%9B%E9%98%B6%E7%AF%87/ShellClash%20%E4%BD%BF%E7%94%A8%20Clash.Meta%20%E5%86%85%E6%A0%B8%E8%BF%9B%E8%A1%8C%20DNS%20%E5%88%86%E6%B5%81%E6%95%99%E7%A8%8B%20rule-set%20%E6%96%B9%E6%A1%88.md)
### ③ [Clash Verge 使用 Clash.Meta 内核进行 DNS 分流教程 rule-set 方案](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E8%BF%9B%E9%98%B6%E7%AF%87/Clash%20Verge%20%E4%BD%BF%E7%94%A8%20Clash.Meta%20%E5%86%85%E6%A0%B8%E8%BF%9B%E8%A1%8C%20DNS%20%E5%88%86%E6%B5%81%E6%95%99%E7%A8%8B%20rule-set%20%E6%96%B9%E6%A1%88.md)
## 3. 分享篇
### ① [分享自己使用 Clash Verge 搭配 rule-set 方案的一套配置](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%88%86%E4%BA%AB%E7%AF%87/%E5%88%86%E4%BA%AB%E8%87%AA%E5%B7%B1%E4%BD%BF%E7%94%A8%20Clash%20Verge%20%E6%90%AD%E9%85%8D%20rule-set%20%E6%96%B9%E6%A1%88%E7%9A%84%E4%B8%80%E5%A5%97%E9%85%8D%E7%BD%AE.md)
### ② [分享自己使用 Clash.Meta for Android 搭配 rule-set 方案的一套配置](https://github.com/DustinWin/clash-tutorials/blob/main/%E6%95%99%E7%A8%8B%E5%90%88%E9%9B%86/%E5%88%86%E4%BA%AB%E7%AF%87/%E5%88%86%E4%BA%AB%E8%87%AA%E5%B7%B1%E4%BD%BF%E7%94%A8%20Clash.Meta%20for%20Android%20%E6%90%AD%E9%85%8D%20rule-set%20%E6%96%B9%E6%A1%88%E7%9A%84%E4%B8%80%E5%A5%97%E9%85%8D%E7%BD%AE.md)
# 给作者加鸡腿：
## 支付宝
![167673823486183](https://user-images.githubusercontent.com/45238096/219877760-b385af34-ebbd-438e-a31f-cd2b985047bb.png)
# 机场推荐
## [bitz](https://cs.getbitzapp.com/#/register?code=HT0ALWZq)：新用户 9 折优惠码 `new9`；8 月 13 日起涨价
