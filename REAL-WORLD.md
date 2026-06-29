# AI Tool Scout — 真实场景

## 场景 1：日常选题扫描

**用户**：AI 工具教程创作者  
**需求**：每天需要找到 1-2 个值得做视频的新工具

```
$ hermes
> 今天有什么 AI 工具值得做视频？

🔍 Phase 1: 扫描 ProductHunt + GitHub + HN
  → 发现 6 个候选
  → 过滤：2 个 ChatGPT wrapper，1 个企业版 $999/月

🦊 Phase 2: Safari 实测 3 个工具
  → Tool A: 免费，有 Demo 页面，视觉效果好 → 🔥🔥🔥
  → Tool B: 有试用，功能清晰 → 🔥🔥
  → Tool C: 登录墙，首页信息有限 → 🔥

📊 Phase 3: 生成 3 份调研报告 + 选题池追加
  🎬 今日首选: Tool A — 明天就录
```

**省了什么**：原来每天手动翻 ProductHunt 30 分钟 + 打开 5 个官网 20 分钟 + 写报告 30 分钟 = 1.5 小时 → 5 分钟。

## 场景 2：赛道竞品分析

**用户**：需要决定用什么 AI 编程工具  
**需求**：对比 Claude Code 和 Cursor

```
$ hermes
> 对比一下 Claude Code 和 Cursor，哪个更适合做视频

🔍 Phase 1: 先搜一下社区评价
  → web_search: "Claude Code vs Cursor 2026"

🦊 Phase 2: 同时打开两个官网
  → safari_navigate("cursor.com") → 提取功能+价格
  → safari_new_tab("claude.ai/code") → 提取功能+价格

📊 Phase 3: 生成对比表
  | 维度 | Claude Code | Cursor |
  | 价格 | $20/mo | 免费+$20 Pro |
  | 上手 | 中级 | 初级 |
  | ...
  🎬 选题: 「Claude Code vs Cursor：谁更好用？」 — 对比测评
```

## 场景 3：垂直赛道扫描

**用户**：想知道某个细分赛道的生态  
**需求**：AI 客服工具有哪些？

```
$ hermes
> 扫描一下 AI 客服工具赛道

🔍 Phase 1: 多源搜索
  → ProductHunt: 3 个工具
  → GitHub: 2 个开源方案
  → Web: Zendesk AI, Intercom Fin, etc.

📊 Phase 3: 赛道地图
  企业级: Zendesk AI, Intercom Fin
  开源: Chatwoot, Papercups
  新兴: 3 个 ProductHunt 新品
  🎬 选题: 「AI 客服工具大盘点：开源 vs 企业 vs 新品」
```
