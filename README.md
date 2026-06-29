# 🔍 AI Tool Scout

> 自动发现 AI 工具 → Safari 浏览器实测 → 生成深度调研报告 → 输出视频选题建议

[![SKILL.md Valid](https://github.com/treyxu23/ai-tool-scout/actions/workflows/validate.yml/badge.svg)](https://github.com/treyxu23/ai-tool-scout/actions/workflows/validate.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Platform: macOS](https://img.shields.io/badge/platform-macOS-lightgrey)](https://github.com/achiya-automation/safari-mcp)
[![Hermes Skill](https://img.shields.io/badge/Hermes-Skill-blue)](https://github.com/nousresearch/hermes-agent)
[![Depends on](https://img.shields.io/badge/depends-safari--web--agent-orange)](https://github.com/treyxu23/safari-web-agent)

---

**中文** | [English](#english)

## 🤔 痛点

做 AI 工具教程的人，每天重复同一件事：

```
打开 ProductHunt → 翻 20 个工具
  → 点开 5 个官网 → 手动截图
    → 提取功能、价格、亮点
      → 对比竞品
        → 写选题建议
          → 1 小时过去了
```

花叔说他「一行代码不会写」，但这套流程他会。你不会？现在也不用会了。

## 🦊 解法

**AI Tool Scout** — 你告诉它想看什么赛道，它自动搞定剩下的。

```bash
$ hermes
> 帮我扫描一下这周新出的 AI 视频工具

AI Tool Scout 启动：
  🔍 Phase 1: 扫描 ProductHunt + GitHub + HN
  → 找到 6 个候选工具
  → 过滤掉 2 个（ChatGPT wrapper / 企业版）

  🦊 Phase 2: Safari 实测
  → ClipStudio AI     | 打开官网 | 截图 | 提取功能+价格 | ✅
  → VideoForge        | 打开官网 | 截图 | 提取功能+价格 | ✅
  → MotionCraft       | 打开官网 | 需要登录 → 有限调研 | ⚠️

  📊 Phase 3: 生成报告
  → 工具库/ClipStudio AI 深度调研.md
  → 工具库/VideoForge 深度调研.md
  → 工具库/MotionCraft 深度调研.md
  → 选题池.md 追加 3 条

  🎬 选题建议:
  🔥🔥🔥 ClipStudio AI — 免费 + 有 demo + 视觉效果好 → 明天就录
  🔥🔥 VideoForge — 有试用 + 功能清晰 → 排入下周
  🔥 MotionCraft — 登录墙 → 低优先级

✅ 3 份深度调研 + 1 份对比表 + 选题建议，全程 5 分钟
```

## ⚡ 能做什么

![Terminal Demo](assets/demo-terminal.svg)

### 1. 赛道扫描

```
> 最近 AI 编程工具有什么新东西？
> 帮我找 AI 设计工具的竞品
> 扫描一下 AI 客服赛道
```

自动搜 ProductHunt、GitHub Trending、Hacker News、网页搜索。去重、过滤、排序。

### 2. 单工具深度调研

```
> 深度调研一下 Cursor
```

打开官网 → 提取功能列表 → 截图 → 找价格 → 按标准模板生成调研报告，存到 Obsidian。

### 3. 多工具横向对比

```
> 对比一下 Claude Code 和 Cursor
```

同时打开两个官网 → 提取各自卖点 → 生成对比表 → 告诉你哪个更适合做视频。

### 4. 选题生成

```
> 今天有什么值得做的选题？
```

扫描最新工具 → 按「免费 + 有 demo + 视觉效果好 + 话题性」打分 → 输出优先级排序。

## 🏗️ 架构

```
ai-tool-scout
    │
    ├─ Phase 1: 发现（并行）
    │   ├─ ProductHunt
    │   ├─ GitHub Trending
    │   ├─ Hacker News
    │   └─ Web Search
    │
    ├─ Phase 2: 深度调研（串行，每个工具）
    │   ├─ safari_navigate → 打开官网
    │   ├─ safari_read_page → 提取内容
    │   ├─ safari_evaluate → 结构化数据
    │   └─ safari_screenshot → 截图存证
    │
    ├─ Phase 3: 报告生成
    │   ├─ 结构化 Markdown（深度调研模板）
    │   ├─ 竞品对比表
    │   └─ 选题建议（三平台）
    │
    └─ 输出 → Obsidian vault
```

## 📦 安装

### 前置条件

- [safari-web-agent](https://github.com/treyxu23/safari-web-agent)（提供 Safari MCP 浏览器操控）
- Obsidian vault（报告输出路径）

### 安装 Skill

```bash
git clone https://github.com/treyxu23/ai-tool-scout.git \
  ~/.hermes/profiles/<profile>/skills/ai-tool-scout/
```

## 📚 文档

| 文件 | 内容 |
|------|------|
| [SKILL.md](SKILL.md) | 核心 Skill — 四阶段工作流、触发条件、常见坑 |
| [REAL-WORLD.md](REAL-WORLD.md) | 真实使用场景 |
| [CHANGELOG.md](CHANGELOG.md) | 版本记录 |
| [references/deep-research-format.md](references/deep-research-format.md) | 深度调研文档模板 |
| [references/source-strategies.md](references/source-strategies.md) | 各数据源搜索策略 |
| [templates/comparison-report.md](templates/comparison-report.md) | 多工具对比模板 |
| [examples/ai-video-tools-scan.md](examples/ai-video-tools-scan.md) | 完整示例：扫描 AI 视频工具 |

---

## English

### What is this?

A Hermes Skill that automates the AI tool discovery and research pipeline: scan ProductHunt/GitHub/HN → open tools in real Safari → extract features/pricing → generate structured reports → suggest video content angles.

### Quick Start

```bash
git clone https://github.com/treyxu23/ai-tool-scout.git \
  ~/.hermes/profiles/<profile>/skills/ai-tool-scout/
```

### Requires

- [safari-web-agent](https://github.com/treyxu23/safari-web-agent) — for real Safari browser automation
- macOS with Safari MCP installed

---

## License

MIT

## Credits

Built on [safari-web-agent](https://github.com/treyxu23/safari-web-agent) and [Safari MCP](https://github.com/achiya-automation/safari-mcp).  
Inspired by 花叔's product-line strategy: turn daily workflow into reusable skills.
