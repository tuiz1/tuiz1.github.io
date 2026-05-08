---
title: 机场使用教程及安全性检测报告
date: 2026-05-08 00:00:00 +0800
categories: [教程]
tags: [机场, FlClash, 魔戒, 安全检测, 使用教程]
description: 魔戒机场的安全性检测报告，附 FlClash 下载安装与使用教程，Windows 和 Android 均可使用。
---
# 机场安全检测报告

**检测时间**：2026-05-08
**检测对象**：魔戒（mojie.net）
**订阅链接**：作者自用，已隐去 token
**节点数**：30 个

---

## 结论

**安全性：一般。可日常使用，不适合敏感场景。**

```
通过：
  ✓ 无恶意脚本
  ✓ 无规则注入
  ✓ 无不认识的证书

注意：
  ⚠ 无 TLS 加密 — 流量特征明显，易被识别
  ⚠ 30 个节点实质是 1 个 — 故障时无真正冗余
  ⚠ VMess 单协议 — 协议单一，被针对时无备选
```

**建议**：不要在此机场上做银行、支付类操作；规则模式只代理需要翻墙的域名，国内网站走直连。

---

## 跨网访问方式说明

本教程采用 **机场 + FlClash 客户端** 方案。跨网访问不止这一种方法，常见的还有：

| 方式 | 说明 | 适合人群 |
|---|---|---|
| 机场 + 开源客户端 | 本教程方式，上手最快 | 日常上网 |
| 自建 VPS + WireGuard | 自行购买海外 VPS 搭建，完全可控 | 有 Linux 基础 |
| 自建 VPS + Xray / Hysteria2 | 协议更丰富，抗封锁更强 | 有折腾意愿 |
| 企业 VPN | 公司提供，合法合规 | 跨国办公 |

选择哪种取决于你的需求：图省事选机场，图安全选自建。

---

## 使用教程（Windows + FlClash）

> FlClash 为开源项目（[GitHub](https://github.com/chen08209/FlClash)），代码公开可审计，安全性较高。

**下载客户端**：

| 平台 | 下载 | 校验 |
|---|---|---|
| Windows | [FlClash-0.8.92-windows-amd64-setup.exe](https://github.com/chen08209/FlClash/releases/download/v0.8.92/FlClash-0.8.92-windows-amd64-setup.exe) | [SHA256](https://github.com/chen08209/FlClash/releases/download/v0.8.92/FlClash-0.8.92-windows-amd64-setup.exe.sha256) |
| Android | [FlClash-0.8.92-android-arm64-v8a.apk](https://github.com/chen08209/FlClash/releases/download/v0.8.92/FlClash-0.8.92-android-arm64-v8a.apk) | — |

**获取订阅链接**：前往魔戒官网注册后即可获取。作者邀请链接：[注册入口](https://mojie.xn--yrs494l.com/register?aff=Y7OSEu7d)

**导入步骤**：

1. 打开 FlClash，进入仪表盘页面

   ![](/assets/img/flclash1.png)

2. 点击第三行的**配置**按钮

   ![](/assets/img/添加配置.png)

3. 点击 **URL 获取**

   ![](/assets/img/url获取.png)

4. 粘贴从机场复制的订阅链接，确认导入

   ![](/assets/img/导入订阅链接.png)

5. 导入完成后点击**启动**按钮即可连接

   ![](/assets/img/flclash1.png)
---

## 检测结果明细

| 项目 | 结果 | 风险 |
|---|---|---|
| 恶意脚本 | 无 | 安全 |
| 内嵌规则 | 无 | 安全 |
| 协议类型 | 全部 VMess | 一般 |
| TLS 加密 | **无** | 注意 |
| 节点独立性 | **30 个共用同一 UUID** | 注意 |
| 入口域名 | 3 个 | 尚可 |

---

## 逐项说明

### 恶意脚本：通过

订阅内容无 `script`、`javascript`、`eval`。导入 FlClash 安全。

### 内嵌规则：通过

订阅内容无规则注入。流量走向全由本地 FlClash 规则控制，机场没有偷偷指定。

### 加密强度：弱

全部节点 TLS 为空。VMess 自身有一层加密，但没有外层 TLS。

```
有 TLS： VMess + TLS = 双层保护，流量像 HTTPS
无 TLS： VMess = 单层保护，流量特征明显
```

### 节点独立性：差

30 个节点共用同一个 UUID：`4dfb76c8-8e09-4997-8bdc-b715713451c3`。这 30 个"节点"实质是同一后端的 30 个不同入口。后端一挂全挂。

### 入口域名

三个入口，指向同一后端，CDN 伪装域名 `mobgslb.tbcache.com`：

```
planb.mojcn.com
m.cnmjin.net
t.cnmjcn.cyou
```
