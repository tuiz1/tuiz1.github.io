---
title: Claude Code 配置 DeepSeek-V4-pro 教程
date: 2026-04-29 20:00:00 +0800
categories: [教程]
tags: [Claude Code, DeepSeek, AI, 开发工具]
---

## 为什么用 DeepSeek-V4-pro？

DeepSeek-V4-pro 是目前性价比最高的模型之一，推理能力对标 Claude Sonnet 4.5，成本却低了两个数量级。配合 Claude Code 的插件体系，完全可以把它设为主力模型，复杂任务再切回 Claude Opus，兼顾效率和省钱。

## CC Switch 是什么

[CC Switch](https://github.com/farion1231/cc-switch) 是一个跨平台的 AI CLI 配置管理工具，支持 Windows/macOS/Linux。它本质上是个图形界面，帮你在 Claude Code 里一键切换模型供应商，不用每次手改配置文件。

核心功能一览：

- 内置 **50+ 供应商预设**，DeepSeek、SiliconFlow、OpenAI 等开箱即用
- **一键切换**模型，终端无需重启
- 用量统计 + 费用监控
- 支持 MCP 配置管理、云同步等

## 第一步：安装 CC Switch

**Windows**：去 [GitHub Releases](https://github.com/farion1231/cc-switch/releases) 下载 `.exe` 或 `.zip` 安装。

**macOS (Homebrew)**：

```bash
brew tap farion1231/ccswitch
brew install --cask cc-switch
```

**Linux**：

```bash
# 下载 .deb
sudo dpkg -i cc-switch_*.deb
```

安装后启动 CC Switch，在顶部应用选择栏切到 **Claude**。

## 第二步：获取 DeepSeek API Key

去 [DeepSeek 开放平台](https://platform.deepseek.com/api_keys) 注册并创建一个 API Key，复制备用。

> DeepSeek-V4-pro 目前需要在 API 请求中指定模型名 `deepseek-chat`。

## 第三步：在 CC Switch 中添加 DeepSeek 供应商

1. 点击界面左下角的 **+** 按钮
2. 在预设列表中搜索 **DeepSeek**，选中
3. 填入你的 **API Key**
4. 模型映射（Model Mapping）不用改，默认将三个模型槽位都映射到 `deepseek-chat`
5. 点击 **Add**，然后点击供应商卡片上的开关将其 **启用**

此时 CC Switch 已经帮你在后台写好了 Claude Code 的 `settings.json` 配置。如果想手动确认，可以检查：

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.deepseek.com/anthropic",
    "ANTHROPIC_AUTH_TOKEN": "sk-your-key",
    "ANTHROPIC_MODEL": "deepseek-chat",
    "ANTHROPIC_SMALL_FAST_MODEL": "deepseek-chat",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "deepseek-chat",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "deepseek-chat",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "deepseek-chat"
  }
}
```

## 第四步：验证可用

在终端任意路径打开 Claude Code：

```bash
claude
```

看到 `Powered by DeepSeek-V4-pro` 就说明配置成功了。试着问一句话，确认有正常回复即可。

## 常见问题

### 遇到 "thinking type should be enabled or disabled" 错误

这是因为新版 Claude Code 发送了 `thinking: {type: "adaptive"}` 请求头，DeepSeek 的 Anthropic 兼容接口不认识。

**解决**：在 CC Switch 的 Claude 通用配置编辑器中，添加环境变量：

```
claude_code_disable_adaptive_thinking=1
```

或者直接编辑 `settings.json`：

```json
{
  "env": {
    "claude_code_disable_adaptive_thinking": "1"
  }
}
```

### 想切回 Claude 官方模型

在 CC Switch 中点击 Anthropic 预设卡片的开关即可，所有配置实时生效，无需重启终端。

## 小结

CC Switch + DeepSeek-V4-pro 的组合很适合日常开发场景。日常编码、搜索、文件操作交给 DeepSeek，省钱且速度不慢；遇到需要深度推理的任务时，一键切回 Claude Opus。两个模型互补，体验很顺畅。

有问题欢迎评论区交流。
