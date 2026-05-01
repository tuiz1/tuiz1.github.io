---
title: Claude Code 配置 DeepSeek-V4-pro 教程
date: 2026-04-29 20:00:00 +0800
categories: [教程]
tags: [Claude Code, DeepSeek, AI, 开发工具]
---
{% raw %}

## 为什么用 DeepSeek-V4-pro？

DeepSeek-V4-pro 是目前性价比最高的模型之一，推理能力对标 Claude Sonnet 4.5，成本却低了两个数量级。配合 Claude Code 的插件体系，完全可以把它设为主力模型，复杂任务再切回 Claude Opus，兼顾效率和省钱。

## CC Switch 是什么

[CC Switch](https://github.com/farion1231/cc-switch) 是一个跨平台的 AI CLI 配置管理工具，支持 Windows/macOS/Linux。它本质上是个图形界面，帮你在 Claude Code 里一键切换模型供应商，不用每次手改配置文件。

核心功能一览：

- 内置 **50+ 供应商预设**，DeepSeek、SiliconFlow、OpenAI 等开箱即用
- **一键切换**模型，终端无需重启
- 用量统计 + 费用监控
- 支持 MCP 配置管理、云同步等

![CC Switch 主界面](/assets/img/ccswitch-main.png)
*[需截图：CC Switch 主界面，显示已添加的供应商列表]*

## 前置环境

开始之前，确认你的环境满足以下条件：

| 依赖 | 说明 | 验证命令 |
|------|------|----------|
| Node.js ≥ v20 (LTS) | Claude Code 运行环境 | `node -v` |
| Git | Claude Code 依赖 | `git --version` |
| WebView2 Runtime | CC Switch 界面渲染 | [下载地址](https://developer.microsoft.com/en-us/microsoft-edge/webview2/) |

> Windows 用户如果打不开 CC Switch，大概率是没装 WebView2 Runtime，点上面链接下载安装即可。

## 第一步：安装 Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

安装完成后在终端输入 `claude` 验证是否能启动。

## 第二步：跳过登录验证

因为我们要用第三方 API，不需要 Anthropic 账号登录。创建或编辑 `~/.claude.json`：

```json
{
  "hasCompletedOnboarding": true
}
```

> `~` 表示用户主目录，Windows 上通常是 `C:\Users\你的用户名\`。

## 第三步：安装 CC Switch

去 [GitHub Releases](https://github.com/farion1231/cc-switch/releases) 下载最新版：

| 平台 | 安装方式 |
|------|----------|
| Windows | 下载 `.msi` 或 `.exe` 安装 |
| macOS | `brew tap farion1231/ccswitch && brew install --cask cc-switch` |
| Linux | 下载 `.deb`，`sudo dpkg -i cc-switch_*.deb` |

安装后启动 CC Switch，在顶部应用选择栏切到 **Claude**。

## 第四步：获取 DeepSeek API Key

去 [DeepSeek 开放平台](https://platform.deepseek.com/api_keys) 注册并创建一个 API Key，复制备用。

![DeepSeek API Key 页面](/assets/img/deepseek-apikey.png)
*[需截图：DeepSeek 开放平台的 API Key 创建/复制页面]*

> 注意：模型名需填写 `DeepSeek-V4-pro` 或 `deepseek-v4-flash`，与 DeepSeek 开放平台的实际模型标识一致。

## 第五步：在 CC Switch 中添加 DeepSeek 供应商

1. 点击界面左下角的 **+** 按钮
2. 在预设列表中搜索 **DeepSeek**，选中
3. 填入你的 **API Key**
4. 模型映射：日常任务选 `deepseek-v4-flash`，复杂任务选 `DeepSeek-V4-pro`（三个模型槽位可以分别设置）
5. 点击 **Add**，然后点击供应商卡片上的开关将其 **启用**

![CC Switch 添加供应商](/assets/img/ccswitch-add-provider.png)
*[需截图：CC Switch 添加供应商的弹窗界面，填好 API Key 和模型映射]*

此时 CC Switch 已经帮你在后台写好了 Claude Code 的 `settings.json` 配置。如果想手动确认，可以检查：

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.deepseek.com/anthropic",
    "ANTHROPIC_AUTH_TOKEN": "sk-your-key",
    "ANTHROPIC_MODEL": "DeepSeek-V4-pro",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "deepseek-v4-flash",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "DeepSeek-V4-pro",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "DeepSeek-V4-pro"
  }
}
```

## 第六步：验证可用

在终端任意路径打开 Claude Code：

```bash
claude
```

看到 `Powered by DeepSeek-V4-pro` 就说明配置成功了。

![Claude Code 启动成功](/assets/img/claude-deepseek-success.png)
*[需截图：终端中 Claude Code 启动，显示 Powered by DeepSeek-V4-pro]*

试着问一句话，确认有正常回复即可。

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

### 配置完 CC Switch，打开 Claude 仍提示登录

按以下顺序排查：

1. **开启 CC Switch 本地代理**：设置 → 代理 → 开启 `Enable Local Proxy`，完全退出 CC Switch 后重启
2. **确认 `.claude.json` 已配置**：检查 `~/.claude.json` 中有 `"hasCompletedOnboarding": true`
3. **CC Switch 中已启用供应商**：确保供应商卡片显示为"已启用"状态（开关为开）
4. **完全退出终端和 Claude Code 后重新打开**

### API 测试报 404 或"端口不存在"

请求地址填写不完整。**解决**：API 端点必须填写完整 URL `https://api.deepseek.com/anthropic`，不要只填根路径，也不要以斜杠结尾。

### 切换模型后不生效

- **Claude Code**：即时生效（支持热重载），切换后开新终端即可
- **Codex / OpenCode / OpenClaw**：需关闭终端重新打开

### 提示"模型不存在或没有访问权限"

在 CC Switch 中先点击「测试连接」，获取模型列表后再选择正确的模型名。推荐模型：

| 模型 | 适用场景 |
|------|----------|
| `deepseek-v4-flash` | 日常编码、改 Bug、速度快 |
| `DeepSeek-V4-pro` | 复杂重构、架构设计、强推理 |

## VSCode 插件额外配置

如果你在 VSCode 中使用 Claude Code 插件，还需要额外一步。编辑 `.claude/config.json`，添加：

```json
{
  "primaryApiKey": "any"
}
```

> 这个值可以随便填，因为实际 API Key 已由 CC Switch 接管，VSCode 插件只需要读到一个非空值就不再弹登录提示。

![VSCode 配置文件](/assets/img/vscode-config.png)
*[需截图：VSCode 中打开 .claude/config.json，显示 primaryApiKey 配置]*

## 推荐使用策略

| 场景 | 推荐模型 |
|------|----------|
| 日常编码、改 Bug、搜文件 | `deepseek-v4-flash`（便宜 + 快） |
| 复杂重构、架构设计、深度Bug | `DeepSeek-V4-pro`（推理能力强） |
| 需要最高质量输出 | 切回 Claude Opus（官方模型） |

**一句话**：Flash 当主力干杂活，Pro 啃硬骨头，Opus 兜底。

## 小结

CC Switch + DeepSeek-V4-pro 的组合很适合日常开发场景。日常编码、搜索、文件操作交给 DeepSeek，省钱且速度不慢；遇到需要深度推理的任务时，一键切回 Claude Opus。两个模型互补，体验很顺畅。

有问题欢迎评论区交流。
{% endraw %}
